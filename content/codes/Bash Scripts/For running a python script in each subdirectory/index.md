---
title: Bash codes for running a python script in each subdirectory
weight: 1
menu:
  codes:
    name: For running a python script in each subdirectory
    identifier: codes-bashScripts-pyEachSubDir
    parent: codes-bashScripts
    weight: 11
---
{{< note title="Bash script for running a phython script in each subdirectory" >}}
This shell script which is developed to batch process simulation results, runs a python script in each subdirectory of a parent folder.
<br/>

```bash
storeDir="processedData"
pyFile="dataExtract.py"

if [ -d "$storeDir" ]; then
	rm -Rf "$storeDir"
fi

for dir in A*/; do
	cp "$pyFile" "$dir"
	cd "$dir"
	python3 "$pyFile"
	rm "$pyFile"
	cd ..
done

mkdir "$storeDir"

mv A*.dat "$storeDir"

echo "Files are moved to $storeDir."
echo "Done..."
```
{{< /note >}}