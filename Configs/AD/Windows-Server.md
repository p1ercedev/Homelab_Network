Post-Install 
```powershell
devmgmt.msc
msinfo32
ncpa.cpl
sysdm.cpl
- Enable Remote Desktop

dism /online /Disable-Feature /FeatureName:WindowsMediaPlayer /norestart

```

## Autoattend Install
### ADK
![[Pasted image 20260106092102.png]]

**Answer FIle**
![[Pasted image 20260106092139.png]]


**Setup**

![[Pasted image 20260106092441.png]]

**Pre-Populate Product Key**

![[Pasted image 20260106092631.png]]

**AutoAttend**
![[Pasted image 20260106093026.png]]


## Configuring GPOs

**Current GPOs**
```powershell
PS C:\Users\Administrator> Get-GPO -All -Domain "TESTLAB.LOCAL"


DisplayName      : Default Domain Policy
DomainName       : TESTLAB.LOCAL
Owner            : TESTLAB\Domain Admins
Id               : 31b2f340-016d-11d2-945f-00c04fb984f9
GpoStatus        : AllSettingsEnabled
Description      :
CreationTime     : 12/21/2025 3:54:22 PM
ModificationTime : 12/22/2025 7:50:18 PM
UserVersion      : AD Version: 0, SysVol Version: 0
ComputerVersion  : AD Version: 3, SysVol Version: 3
WmiFilter        :

DisplayName      : Default Domain Controllers Policy
DomainName       : TESTLAB.LOCAL
Owner            : TESTLAB\Domain Admins
Id               : 6ac1786c-016f-11d2-945f-00c04fb984f9
GpoStatus        : AllSettingsEnabled
Description      :
CreationTime     : 12/21/2025 3:54:22 PM
ModificationTime : 12/21/2025 3:54:22 PM
UserVersion      : AD Version: 0, SysVol Version: 0
ComputerVersion  : AD Version: 1, SysVol Version: 1
WmiFilter        :
```

**New GPO**
![[Pasted image 20260106105131.png]]

**Disable Windows Defender**
![[Pasted image 20260106105106.png]]

```powershell
PS C:\Users\Administrator> New-GPLink -Guid fc79770f-ec7c-4295-9499-0992931e7c48 -Target "DC=TESTLAB,DC=LOCAL"


GpoId       : fc79770f-ec7c-4295-9499-0992931e7c48
DisplayName : Disable-Defender
Enabled     : True
Enforced    : False
Target      : DC=testlab,DC=local
Order       : 2


```

```powershell

PS C:\Users\Administrator> gpupdate /force
Updating policy...

Computer Policy update has completed successfully.
User Policy update has completed successfully.

```

```powershell

```
