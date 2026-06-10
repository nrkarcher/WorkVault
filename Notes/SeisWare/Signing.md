Signing tool testing can be done with the Windows Signtool from the command line

Set the path to the folder of the signing tool:
	set PATH="C:\Program Files (x86)\Windows Kits\10\bin\10.0.19041.0\x64";%PATH%
Verify:
	signtool verify /pa /v "C:\\Users\\narcher\\Desktop\\SeisWare_10.8.3.107_x64_SII.exe"


**Possible SignTool Error: A certificate chain processed, but terminated in a root certificate which is not trusted by the trust provider.**  
  
**Answer:**  
    This is because of the 'verify' command you may have run: **signtool verify myfile.exe**. If you run this command, signtool will use the Windows Driver Verification Policy. In order for your file to 'verify' properly you need to include the **/pa** switch, so that SignTool uses the Default Authentication Verification Policy.  
  
**Example:  
    _signtool verify /pa myfile.exe_**

https://learn.microsoft.com/en-us/windows/win32/seccrypto/signtool

---

# Test Signing on All Output files

Run Powershell as Administrator

Basic Solution - All Files in the folder:
```powershell
Get-ChildItem -Path "C:\YourFolder" -Recurse | ForEach-Object {
    "C:\Program Files (x86)\Windows Kits\10\bin\10.0.19041.0\x86\signtool.exe verify /pa $_.FullName
}
```


Test .exe files, detailed errors:

Get-ChildItem -Path "C:\Program Files\SII 10.8\Program" -Filter .exe -Recurse | ForEach-Object {
    try {
        & "C:\Program Files (x86)\Windows Kits\10\bin\10.0.19041.0\x86\signtool.exe" verify /pa /v $_.FullName
    } catch {
        Write-Host "Error verifying file: (_.FullName)"
        Write-Host "Exception Message: (_.Exception.Message)"
        Write-Host "Inner Exception Message: (_.Exception.InnerException.Message)"
    }
} | Out-File -FilePath "C:\SigningVerification\verification_results.txt"



---

Sample Output Results for Installers:

Verifying: M:\mlazarevic\10.8.3\Rerecut\SeisWare_10.8.3.118_x64_SII.exe

Signature Index: 0 (Primary Signature)
Hash of file (sha256): 84D8AF0EA81680B13581072CCE9A62CE6B5751A26F0CCEAEB769375E59BF5EA9

Signing Certificate Chain:
    Issued to: AAA Certificate Services
    Issued by: AAA Certificate Services
    Expires:   Sun Dec 31 17:59:59 2028
    SHA1 hash: D1EB23A46D17D68FD92564C2F1F1601764D8E349

        Issued to: Sectigo Public Code Signing Root R46
        Issued by: AAA Certificate Services
        Expires:   Sun Dec 31 17:59:59 2028
        SHA1 hash: 329B78A5C9EBC2043242DE90CE1B7C6B1BA6C692

            Issued to: Sectigo Public Code Signing CA R36
            Issued by: Sectigo Public Code Signing Root R46
            Expires:   Fri Mar 21 17:59:59 2036
            SHA1 hash: 0BC5E76773D2E44FC9903D4DFEFE451553BBEC4A

                Issued to: SeisWare International Inc
                Issued by: Sectigo Public Code Signing CA R36
                Expires:   Mon Jan 25 17:59:59 2027
                SHA1 hash: FE5AAF09F2ED79B6894921B0BD859103FE88C728

The signature is timestamped: Thu Mar 14 16:35:34 2024
Timestamp Verified by:
    Issued to: USERTrust RSA Certification Authority
    Issued by: USERTrust RSA Certification Authority
    Expires:   Mon Jan 18 17:59:59 2038
    SHA1 hash: 2B8F1B57330DBBA2D07A6C51F70EE90DDAB9AD8E

        Issued to: Sectigo RSA Time Stamping CA
        Issued by: USERTrust RSA Certification Authority
        Expires:   Mon Jan 18 17:59:59 2038
        SHA1 hash: 02D65B95E28370C1570095FA88F923DD937FAD8F

            Issued to: Sectigo RSA Time Stamping Signer #4
            Issued by: Sectigo RSA Time Stamping CA
            Expires:   Wed Aug 02 17:59:59 2034
            SHA1 hash: AE62AF750A0CBD47D6461F7568E2BC8CE7CA4F94


Successfully verified: M:\mlazarevic\10.8.3\Rerecut\SeisWare_10.8.3.118_x64_SII.exe

Number of files successfully Verified: 1
Number of warnings: 0
Number of errors: 0

C:\Users\narcher>signtool verify /pa /v "M:\mlazarevic\10.8.3\Rerecut\SeisWare_10.8.3.118_x64_SII.msi"

Verifying: M:\mlazarevic\10.8.3\Rerecut\SeisWare_10.8.3.118_x64_SII.msi

Signature Index: 0 (Primary Signature)
Hash of file (sha256): A15D58881C69F6DF490F0FF8364CD2EF476B26C07F1CF84ECA028E7ED89E9CFA

Signing Certificate Chain:
    Issued to: AAA Certificate Services
    Issued by: AAA Certificate Services
    Expires:   Sun Dec 31 17:59:59 2028
    SHA1 hash: D1EB23A46D17D68FD92564C2F1F1601764D8E349

        Issued to: Sectigo Public Code Signing Root R46
        Issued by: AAA Certificate Services
        Expires:   Sun Dec 31 17:59:59 2028
        SHA1 hash: 329B78A5C9EBC2043242DE90CE1B7C6B1BA6C692

            Issued to: Sectigo Public Code Signing CA R36
            Issued by: Sectigo Public Code Signing Root R46
            Expires:   Fri Mar 21 17:59:59 2036
            SHA1 hash: 0BC5E76773D2E44FC9903D4DFEFE451553BBEC4A

                Issued to: SeisWare International Inc
                Issued by: Sectigo Public Code Signing CA R36
                Expires:   Mon Jan 25 17:59:59 2027
                SHA1 hash: FE5AAF09F2ED79B6894921B0BD859103FE88C728

The signature is timestamped: Thu Mar 14 16:35:24 2024
Timestamp Verified by:
    Issued to: USERTrust RSA Certification Authority
    Issued by: USERTrust RSA Certification Authority
    Expires:   Mon Jan 18 17:59:59 2038
    SHA1 hash: 2B8F1B57330DBBA2D07A6C51F70EE90DDAB9AD8E

        Issued to: Sectigo RSA Time Stamping CA
        Issued by: USERTrust RSA Certification Authority
        Expires:   Mon Jan 18 17:59:59 2038
        SHA1 hash: 02D65B95E28370C1570095FA88F923DD937FAD8F

            Issued to: Sectigo RSA Time Stamping Signer #4
            Issued by: Sectigo RSA Time Stamping CA
            Expires:   Wed Aug 02 17:59:59 2034
            SHA1 hash: AE62AF750A0CBD47D6461F7568E2BC8CE7CA4F94


Successfully verified: M:\mlazarevic\10.8.3\Rerecut\SeisWare_10.8.3.118_x64_SII.msi

Number of files successfully Verified: 1
Number of warnings: 0
Number of errors: 0

