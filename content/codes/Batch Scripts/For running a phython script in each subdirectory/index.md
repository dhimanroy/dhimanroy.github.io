---
title: Batch script for running a phython script in each subdirectory
date: 2021-11-09
weight: 2
menu:
  codes:
    name: For running a phython script in each subdirectory
    identifier: codes-batchScripts-runPythonScript
    parent: codes-batchScripts
    weight: 21
---
{{< note title="Batch script for running a phython script in each subdirectory" >}}
This script runs a python script in each subdirectory of a parent folder. This code is developed to batch process simulation results without any commercial software. A tiny step towards building open source tools.
<br/>

```batch

@echo off
set back=%cd%
for /d %%i in (%~dp0\*) do (
xcopy "dataExtract.py" "%%i"
cd "%%i"
echo current directory:
cd
python3 dataExtract.py
)
cd %back%
for /d %%i in (%~dp0\*) do (
cd "%%i"
del dataExtract.py
)
cd %back%
pause

```
{{< /note >}}