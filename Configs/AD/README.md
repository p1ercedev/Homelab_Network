
### Bulk user create
```powershell
FirstName,LastName,SamAccountName,UserPrincipalName,OU,Password,Department,Title
John,Doe,jdoe,jdoe@testlab.local,"OU=Users,DC=testlab,DC=local",P@ssw0rd123!,IT,System Administrator
Michael,Brown,mbrown,mbrown@testlab.local,"OU=Users,DC=testlab,DC=local",P@ssw0rd123!,Finance,Financial Analyst
Emily,Johnson,ejohnson,ejohnson@testlab.local,"OU=Users,DC=testlab,DC=local",P@ssw0rd123!,Sales,Sales Associate
David,Wilson,dwilson,dwilson@testlab.local,"OU=Users,DC=testlab,DC=local",P@ssw0rd123!,Engineering,Software Engineer
```

```powershell
PS C:\Users\Administrator> Import-Csv .\users.csv | ForEach-Object { New-ADUser -Name "$($_.FirstName) $($_.LastName)" -GivenName $_.FirstName -Surname $_.LastName -SamAccountName $_.SamAccountName -UserPrincipalName $_.UserPrincipalName -Path "DC=testlab,DC=local" -Department $_.Department -Title $_.Title -AccountPassword (ConvertTo-SecureString $_.Password -AsPlainText -Force) -Enabled $true }
```