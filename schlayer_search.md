Script to detect *OSX Schlayer* execution in splunk
Typical Schlayer Filepath: `/tmp/FCCC54B5-2618-46AE-AC6B-C9D12A6FB28E/2/20A83665-709A-4F7E-A340-BB0A547E5DC5/`
Remember ... this regex lookup is SPECIFICALLY for the above file path format and nothing else..

```
index=* /private/tmp
| regex TargetFileName="\/tmp\/[a-fA-F0-9]+-[0-9]+-[a-fA-F0-9]+-[a-fA-F0-9]+-[a-fA-F0-9]+\/[0-9]\/[a-fA-F0-9]+-[a-fA-F0-9]+-[a-fA-F0-9]+-[a-fA-F0-9]+-[a-fA-F0-9]+\/"
| transaction host cookie maxspan=30s maxpause=5s 
| stats count by ComputerName
| sort - count
| fillnull value="-"
```
