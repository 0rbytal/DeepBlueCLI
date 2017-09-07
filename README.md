# DeepBlueCLI

DeepBlueCLI 0.3 Beta

Eric Conrad, Backshore Communications, LLC

deepblue `at` backshore `dot` net

Twitter: @eric_conrad

http://ericconrad.com

Sample evtx files are in the .\evtx directory

## Table of Contents  
- [Usage](#usage)  
- [Examples](#examples) 
- [Logging setup](#logging-setup)
- [VirusTotal and Whitelisting setup](#virustotal-and-whitelisting-setup)

## Usage:


`.\DeepBlue.ps1 <event log name> <evtx filename>`

If you see this error:

`.\DeepBlue.ps1 : File .\DeepBlue.ps1 cannot be loaded because running scripts is
disabled on this system. For more information, see about_Execution_Policies at
http://go.microsoft.com/fwlink/?LinkID=135170.`

You must run Set-ExecutionPolicy as Administrator, here is an example:

`Set-ExecutionPolicy RemoteSigned`

See `get-help Set-ExecutionPolicy` for more options.

## Examples:

### Process local Windows security event log:

`.\DeepBlue.ps1`

or:

`.\DeepBlue.ps1 -log security`

### Process local Windows system event log:

`.\DeepBlue.ps1 -log system`

or:

`.\DeepBlue.ps1 "" system`

### Process evtx file:

`.\DeepBlue.ps1 .\evtx\new-user-security.evtx`

or:

`.\DeepBlue.ps1 -file .\evtx\new-user-security.evtx`

## Windows Event Logs processed

- Windows Security 
- Windows System
- Windows Application
- Windows Powershell 
- Sysmon (new)

### Command Lines Logs processed

See 'Logging setup' section below for how to configure these logs

- Windows Security event ID 4688 
- Windows Powershell event IDs 4103 and 4104
- Sysmon event ID 1

## Logging setup

Setting up logging:

### Security event 4688 (Command line auditing):

Enable Windows command-line auditing: https://support.microsoft.com/en-us/kb/3004375 

### PowerShell auditing (PowerShell 5.0):

DeepBlueCLI uses module logging (PowerShell event 4013) and script block logging (4104). It does not use transcription.

See: https://www.fireeye.com/blog/threat-research/2016/02/greater_visibilityt.html

To get the PowerShell commandline (and not just script block) on Windows 7 through Windows 8.1, add the following to \Windows\System32\WindowsPowerShell\v1.0\profile.ps1
```
$LogCommandHealthEvent = $true
$LogCommandLifecycleEvent = $true
```
See the following for more information:
 - https://logrhythm.com/blog/powershell-command-line-logging/
 - http://hackerhurricane.blogspot.com/2014/11/i-powershell-logging-what-everyone.html

Thank you: @heinzarelli and @HackerHurricane

### Sysmon

Install Sysmon from Sysinternals: https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon

Log SHA1 hashes. Others are fine; DeepBlueCLI will use SHA1.

## VirusTotal and Whitelisting setup

Setting up VirusTotal hash submissions and whitelisting:

The hash checker requires Post-VirusTotal:

 - https://github.com/darkoperator/Posh-VirusTotal

It also requires a VirusTotal API key: 

 - https://www.virustotal.com/en/documentation/public-api/

The script assumes a personal API key, and waits 15 seconds between submissions.
