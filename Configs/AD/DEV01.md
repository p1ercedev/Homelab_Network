**Install winget**
SInce this is a TinyOS winget is not pre-installed

```powerhsell
PS C:\Windows\system32> Install-PackageProvider -Name NuGet                                                                                          
<SNIP>
Name    Version          Source           Summary
----                           -------          ------           -------
nuget                          2.8.5.208        https://cdn.o... NuGet provider for the OneGet meta-package manager


PS C:\Windows\system32> Install-Module -Name Microsoft.WinGet.Client -Force -Repository PSGallery
PS C:\Windows\system32> Repair-WinGetPackageManager -AllUsers
PS C:\Windows\system32> winget search msys2
The `msstore` source requires that you view the following agreements before using.
Terms of Transaction: https://aka.ms/microsoft-store-terms-of-transaction
The source requires the current machine's 2-letter geographic region to be sent to the backend service to function properly (ex. "US").

Do you agree to all the source agreements terms?
[Y] Yes  [N] No: Y
Name                Id                                   Version  Source
-------------------------------------------------------------------------
Azahar              AzaharEmulator.Azahar.MSYS2          2123.3   winget
MSYS2 Installer     MSYS2.MSYS2                          20251213 winget
<SNIP>
```

