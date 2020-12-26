This first script is a typical splunk query to look for any activity to evilcorp.com 
We make it a little more efficient and less robust by piping everything into a 
transcation. Then table it out and sort it based on time. 

```
earliest=-1w index=* evilcorp.com 
| transaction host cookie maxspan=30s maxpause=5s 
| table _time, ComputerName,  FilePath, FileName, CommandLine, aip, event_simpleName, DomainName, LocalAddressIP4, DetectDescription, DnsRequests{}.DomainName
| sort _time
| fillnull value="-" 
```
Next we take the AID field from the output of the first script
and use that AID to pivot around +/- 10 minutes around the time of event
```
aid=e42a670c3d2644027bd7acdd91258f8e name!=TerminateProcessMac name!=CreateProcess* earliest=-30d 
| eval timestamp= /1000
| eval begin =-600
| eval end =+600
| where _time > begin AND _time < end
| table _time name ImageFileName CommandLine TargetFileName detectionCount BootArgs 
| sort -_time
```
