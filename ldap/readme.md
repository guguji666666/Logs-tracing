# Capture ldap logs 

## 1. Logs with Process monitor 
### 1. [Download process monitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon)
### 2. Run `Process monitor`


### 3. Capture LDAP events' relating logs in Process monitor 
Set the filter in process monitor
![image](https://github.com/user-attachments/assets/aeedb5a3-2ef6-4c2b-8205-71ef8aed8968)

```
:ldap
```
![image](https://github.com/user-attachments/assets/8857d58e-84f5-4eeb-9dd9-af1dc815cdb3)

![image](https://github.com/user-attachments/assets/3bf22dc8-bbc3-4d83-9910-8379b79ec23a)

Restrict the size of each log file
![image](https://github.com/user-attachments/assets/a7a585b3-f802-4f9e-9ae7-d46b26b7f0a1)

Set the MAX. size of each log (we set 1 GB in sample), once the size is reached the old log will be replaced with new one <br>
![image](https://github.com/user-attachments/assets/24655d8c-15b9-457d-a892-49b1151af058)


Run powershell script below to genrate LDAP events
```powershell
# Add a custom event log source (run as Administrator once)
if (-not [System.Diagnostics.EventLog]::SourceExists("MyLDAPScript")) {
    New-EventLog -LogName Application -Source "MyLDAPScript"
}

# Define a function to create log entries
function Log-Event {
    param (
        [string]$message,
        [string]$eventType = "Information"
    )
    Write-EventLog -LogName Application -Source "MyLDAPScript" -EventID 1000 -EntryType $eventType -Message $message
}

# Get the RootDSE
$rootDSE = [ADSI]"LDAP://RootDSE"
$domainDN = $rootDSE.defaultNamingContext

# Create DirectorySearcher object
$searcher = New-Object System.DirectoryServices.DirectorySearcher
$searcher.SearchRoot = [ADSI]"LDAP://$domainDN"
$searcher.Filter = "(objectClass=trustedDomain)"
$searcher.PropertiesToLoad.Add("cn") | Out-Null
$searcher.PropertiesToLoad.Add("trustPartner") | Out-Null

try {
    # Perform the search
    $results = $searcher.FindAll()

    # Log success
    Log-Event -message "LDAP search executed successfully."
    
    # Process the results
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

## 2. Net trace using 
