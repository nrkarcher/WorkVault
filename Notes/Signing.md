Signing tool testing can be done with the Windows Signtool from the command line
Set the path to the folder of the signing tool:
	set PATH="C:\Program Files (x86)\Windows Kits\10\bin\10.0.19041.0\x64";%PATH%
Verify:
	signtool verify /pa /v "C:\\Users\\narcher\\Desktop\\SeisWare_10.8.3.107_x64_SII.exe"

**SignTool Error: A certificate chain processed, but terminated in a root certificate which is not trusted by the trust provider.**  
  
**Answer:**  
    This is because of the 'verify' command you may have run: **signtool verify myfile.exe**. If you run this command, signtool will use the Windows Driver Verification Policy. In order for your file to 'verify' properly you need to include the **/pa** switch, so that SignTool uses the Default Authentication Verification Policy.  
  
**Example:  
    _signtool verify /pa myfile.exe_**

https://learn.microsoft.com/en-us/windows/win32/seccrypto/signtool