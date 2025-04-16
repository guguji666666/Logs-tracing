# Capture logs with Process monitor 
## 1. [Download process monitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon)
## 2. Run `Process monitor`


## 1. LDAP logs
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
