## Powershell

### Search for last Logins via RDP {#search_for_last_logins_via_rdp}

;

`$startDate = (Get-Date).AddDays(-14).ToString('MM/dd/yyyy HH:mm:ss')`\
`$endDate = (Get-Date).ToString('MM/dd/yyyy HH:mm:ss')`\
`Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational';  `\
`ID=1149; StartTime=$startDate; EndTime=$endDate} |`\
`   Where-Object {`\
`       $_.Properties[0].Value -ne $null -and`\
`       $_.Properties[0].Value -notin @('IUSR', 'LOCAL SERVICE', 'DWM-1')`\
`   } |`\
`   Select-Object TimeCreated, @{Name='Username'; Expression={$_.Properties[0].Value}}, @{Name='IPAddress'; Expression={$_.Properties[2].Value}}`

### Get Printers from Printserver {#get_printers_from_printserver}

;

`Get-WmiObject -Class Win32_Printer -ComputerName PRINTSERVERNAME | Select-Object Name, DriverName, PortName, ShareName, Location, Comment, Default, Network, Published, Shared, Status`

## Active Directory {#active_directory}

### Check Password never expires checkbox for Service Account {#check_password_never_expires_checkbox_for_service_account}

;

`Import-Module ActiveDirectory`\
`$users = Get-ADUser -Filter {PasswordNeverExpires -eq $false -and SamAccountName -like "sa_*"}`\
`foreach ($user in $users) {`\
`   Write-Output "User $($user.SamAccountName) / $($user.Name) has the Password never expires checkbox NOT checked! Please fix it!"`\
`}`

[Category:Windows](Category:Windows "Category:Windows"){.wikilink}
