# 📥 Capture LDAP Logs for Troubleshooting

This guide walks you through collecting LDAP-related logs using **Process Monitor** and **Network Trace (NMCap)** for deep-dive troubleshooting.

---

## 🔍 1. Capture Logs with Process Monitor

### Step 1: [Download Process Monitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon)

### Step 2: Launch Process Monitor as Administrator

### Step 3: Configure Filters to Capture LDAP Events

* Open the **Filter** settings in Process Monitor.
* Add a filter keyword for `ldap` to capture relevant traffic.

![Procmon LDAP Filter](https://github.com/user-attachments/assets/aeedb5a3-2ef6-4c2b-8205-71ef8aed8968)

```
:ldap
```

Example filter view:

![Procmon Filter](https://github.com/user-attachments/assets/8857d58e-84f5-4eeb-9dd9-af1dc815cdb3)

Result preview:

![Procmon Results](https://github.com/user-attachments/assets/3bf22dc8-bbc3-4d83-9910-8379b79ec23a)

---

### Step 4: Limit Log File Size

* Go to **File > Backing Files...**
* Enable circular logging and set the **maximum file size** (e.g., 1 GB).

> Once the file size limit is reached, older data is overwritten to ensure continuous logging.

![Log Size Setting](https://github.com/user-attachments/assets/a7a585b3-f802-4f9e-9ae7-d46b26b7f0a1)

![Max Size](https://github.com/user-attachments/assets/24655d8c-15b9-457d-a892-49b1151af058)

---

### Step 5: Simulate LDAP Activity with PowerShell

Run the following PowerShell script to generate LDAP events (make sure to run as Administrator once to register the event source):

```powershell
# Add a custom event log source
if (-not [System.Diagnostics.EventLog]::SourceExists("MyLDAPScript")) {
    New-EventLog -LogName Application -Source "MyLDAPScript"
}

# Function to write to the event log
function Log-Event {
    param (
        [string]$message,
        [string]$eventType = "Information"
    )
    Write-EventLog -LogName Application -Source "MyLDAPScript" -EventID 1000 -EntryType $eventType -Message $message
}

# Perform LDAP query
$rootDSE = [ADSI]"LDAP://RootDSE"
$domainDN = $rootDSE.defaultNamingContext

$searcher = New-Object System.DirectoryServices.DirectorySearcher
$searcher.SearchRoot = [ADSI]"LDAP://$domainDN"
$searcher.Filter = "(objectClass=trustedDomain)"
$searcher.PropertiesToLoad.Add("cn") | Out-Null
$searcher.PropertiesToLoad.Add("trustPartner") | Out-Null

try {
    $results = $searcher.FindAll()
    Log-Event -message "LDAP search executed successfully."

    foreach ($result in $results) {
        $entry = $result.GetDirectoryEntry()
        [PSCustomObject]@{
            CN           = $entry.cn
            TrustPartner = $entry.trustPartner
        }
    }
} catch {
    Log-Event -message "An error occurred during LDAP search: $_" -eventType "Error"
    Write-Error "An error occurred: $_"
}
```

---

## 🌐 2. Capture Network Trace (LDAP Ports)

### Step 1: Run the Following Command as Administrator

```cmd
mkdir c:\mslogs

NMCap /network * /capture "(tcp.port == 389 or tcp.port == 636 or tcp.port==3268 or tcp.port == 3269)" /file c:\mslogs\test.cap:300M
```

### 📌 Explanation:

* `NMCap` is the network capture utility.
* It monitors all interfaces: `/network *`
* It captures traffic on common LDAP ports: 389 (LDAP), 636 (LDAPS), 3268/3269 (Global Catalog)
* The `:300M` parameter restricts each capture file to 300 MB. You can adjust this value as needed (e.g., 100M for light logs or 1G for heavy usage).

Example in action:
![NMCap Example](https://github.com/user-attachments/assets/a4f7bd5d-989c-47b0-a4fd-26385ec57517)

> ⚠️ **Important:**
>
> * **Do NOT close the CMD window** while the capture is running.
> * **Do NOT sign out or reboot the machine**, as this will interrupt the capture.
> * When the issue has been reproduced, press **Ctrl + C** in the CMD window to stop the trace.

---

## 📤 After Reproducing the Issue

Once the issue is reproduced:

1. Stop Process Monitor and save the `.PML` file.
2. Stop NMCap using `Ctrl + C`, and keep the `test.cap` file.
3. Collect the following:

   * Process Monitor logs (`.pml`)
   * Network trace (`c:\mslogs\test.cap`)
   * Any scan or diagnostic reports (if applicable)
4. Share the logs for further analysis.

---
