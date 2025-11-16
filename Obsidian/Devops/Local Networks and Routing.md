![[Pasted image 20251116173402.png]]

![[Pasted image 20251116175058.png]]

- You can Redirect any website that is going to open on your system or browser to the localhost  or any other website domain created by you , by overriding the host files. 
![[Pasted image 20251116180200.png]]

## How to modify the host files 
To override a system's **hosts file** entry for a specific domain using **PowerShell**, you'll need to use commands to either **add, modify, or remove** lines in the file, which is typically located at `C:\Windows\System32\drivers\etc\hosts`.

### 1. ✍️ Add or Override a Host Entry

This command appends a new entry to the end of the hosts file. If the domain already exists, this will result in the entry being defined **twice**, and the **first entry in the file will take precedence** unless you remove the old one.

The standard format is: `[IP_Address] [Domain_Name]`
```powershell
# Define the new entry
$NewEntry = "127.0.0.1 example.com"

# Define the hosts file path
$HostsFile = "C:\Windows\System32\drivers\etc\hosts"

# Append the new entry to the hosts file
Add-Content -Path $HostsFile -Value $NewEntry

# Display confirmation
Write-Host "Added or modified host entry: $NewEntry"
```

---
### 2. ✏️ Cleanly Modify/Remove an Old Entry (Recommended Override)
To truly "override" an existing entry without creating duplicates, you should first remove any existing lines for that specific domain.
This script removes all existing lines containing the domain (`example.com`) and then adds the new one.

```powershell
# --- Configuration ---
$Domain = "example.com"
$NewIP = "127.0.0.1"
$HostsFile = "C:\Windows\System32\drivers\etc\hosts"
# --- End Configuration ---

# 1. Read the current hosts file content
$Content = Get-Content -Path $HostsFile

# 2. Filter out (remove) any existing lines that contain the domain name
# The -notmatch ensures lines with the domain are excluded
$NewContent = $Content | Where-Object { $_ -notmatch $Domain }

# 3. Add the new entry to the filtered content
$NewContent += "$NewIP $Domain"

# 4. Overwrite the hosts file with the new content
# -Force is used in case the file is read-only
$NewContent | Set-Content -Path $HostsFile -Force

# Display confirmation
Write-Host "Successfully overrode $Domain to point to $NewIP."
```

---
### 3. ♻️ Flush the DNS Cache
After modifying the hosts file, your system's DNS resolver cache might still hold the old entry. You should **flush the DNS cache** to ensure the new hosts file entry is used immediately.

```powershell
# Flush the local DNS cache
ipconfig /flushdns

Write-Host "Local DNS cache has been flushed."
```
By following these steps, you can reliably use PowerShell to manage and override specific domain mappings in your system's hosts file.