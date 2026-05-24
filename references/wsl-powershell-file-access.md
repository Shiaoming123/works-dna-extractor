# WSL + PowerShell: Reading files from Windows paths with special characters

## Problem

When a Windows filesystem directory path contains `&`, certain Unicode emoji, or other characters that bash interprets as operators, WSL's `/mnt/` 9P mount may silently fail. Symptoms:

- `ls` returns only partial file listings with "Input/output error"
- `find` misses files
- `read_file` returns "File not found"
- The user says "there are 60+ files" but you only see 11

## Root cause

The `&` character in the path (e.g. `/mnt/e/酒馆角色卡/novel/伪娘&futa/`) is interpreted as the bash background operator even when the path is quoted. The 9P protocol layer between WSL and Windows hits an I/O error when accessing the directory.

## Fix: Use PowerShell directly via subprocess

### Method A: Python subprocess (array form — preferred in execute_code)

```python
import subprocess, json

# List files
result = subprocess.run(
    ['powershell.exe', '-NoProfile', '-Command', 
     'Get-ChildItem -File -Path "E:\\path\\to\\dir" | Select-Object Name | ConvertTo-Json -Compress'],
    capture_output=True, text=True, timeout=30
)
files = json.loads(result.stdout)

# Read a file's full content
fn = files[0]['Name']  # or any filename
result = subprocess.run(
    ['powershell.exe', '-NoProfile', '-Command',
     f'Get-Content "E:\\path\\to\\dir\\{fn}" -Encoding UTF8'],
    capture_output=True, text=True, timeout=120
)
content = result.stdout
```

**Critical**: Use `subprocess.run(..., shell=False)` (the default with array args). Do NOT use `shell=True` or pipe through `terminal()`—those go through bash and hit the same `&` issue.

### Method B: Inline PowerShell via terminal (for simple commands)

```bash
powershell.exe -Command "Get-ChildItem -File -Path 'E:\path\to\dir' | Select-Object Name"
```

Works for simple commands. For complex scripts, write a `.ps1` file, place it in the Windows temp directory, and execute it:

```python
# Write script
with open('/tmp/script.ps1', 'w') as f:
    f.write(powershell_script_content)

# Copy to Windows temp (find windows username first)
import subprocess
user = subprocess.run(['cmd.exe', '/c', 'echo %USERPROFILE%'], 
                      capture_output=True, text=True).stdout.strip().split('\\')[-1]

import shutil
shutil.copy('/tmp/script.ps1', f'/mnt/c/Users/{user}/AppData/Local/Temp/script.ps1')

# Execute
subprocess.run(
    ['powershell.exe', '-ExecutionPolicy', 'Bypass', 
     f'C:\\Users\\{user}\\AppData\\Local\\Temp\\script.ps1'],
    capture_output=True, text=True, timeout=120
)
```

## Finding the Windows username

The WSL username and Windows username often differ. Find the Windows username:

```bash
cmd.exe /c "echo %USERPROFILE%"
# Returns: C:\Users\ASUS  (username is "ASUS")
```

## Path encoding notes

- `E:\酒馆角色卡\novel\伪娘&futa` works fine in PowerShell
- The `&` in the path does NOT cause issues in PowerShell (only bash interprets it)
- Unicode characters in paths work fine in PowerShell
- Always use `-Encoding UTF8` when reading Chinese text files

## Complete working example

```python
import subprocess, json

# Step 1: List all files
r = subprocess.run(
    ['powershell.exe', '-NoProfile', '-Command',
     'Get-ChildItem -File -Path "E:\\酒馆角色卡\\novel\\伪娘&futa" | Select-Object Name, @{N=\'KB\';E={[math]::Round($_.Length/1KB)}} | ConvertTo-Json -Compress'],
    capture_output=True, text=True, timeout=30
)
files = json.loads(r.stdout)

# Step 2: Read each file
for f in files:
    fn = f['Name'].replace("'", "''")
    r = subprocess.run(
        ['powershell.exe', '-NoProfile', '-Command',
         f'$c = Get-Content "E:\\酒馆角色卡\\novel\\伪娘&futa\\{fn}" -Encoding UTF8; $total = $c.Count; $head = $c[0..[Math]::Min(79,$total-1)]; $tail = $c[-30..-1]; Write-Host "LINES:$total"; Write-Host "HEAD"; $head; Write-Host "TAIL"; $tail'],
        capture_output=True, text=True, timeout=120
    )
    # Parse output...
```

## Detection checklist

When you suspect the WSL special-character path issue:

- [ ] `ls` returns "Input/output error" on the directory?
- [ ] File count seems suspiciously low (e.g. 11 when user expects 60+)?
- [ ] Path contains `&`, `(`, `)`, `!`, or unusual Unicode?
- [ ] `read_file` returns "File not found" for files that `ls` listed?
- [ ] PowerShell/CMD can see files that WSL cannot?

If any of these are true → use the PowerShell subprocess method above.
