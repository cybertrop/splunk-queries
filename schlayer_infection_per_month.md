This is a script that can be used in tangent to the carbonblack API scripts in the carbonblack API folder
Once you clean up your data from CB API into a JSON format and pump it into splunk...you extract the "last_seen"
field as your main time field... you can than run this query and create a nice timechart of your choice to show
infections per month.

`source="carbon_black_schlayer.txt" host="whatever" sourcetype="cb-json" 
| timechart count(_time) 
| fillnull value="-"
`
