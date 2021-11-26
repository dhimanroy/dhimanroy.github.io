---
title: Python script for extracting data from ascii files
date: 2021-11-07
weight: 4
menu:
  codes:
    name: For extracting data from ascii files
    identifier: codes-python-extractData
    parent: codes-python
    weight: 41
---
{{< note title="Python script for extracting data from ascii files" >}}
This script reads ascii files, collects desired data, process and store processed data to a .dat (actually a csv) file. This can also be done using <a href="https://en.wikipedia.org/wiki/Grep" target="blank">grep</a> in linux terminal. </br></br>
Now, let's jump into the script. Necessary packages like pandas, natsort, pathlib are imported first.
```python
import os
import pandas as pd
from pathlib import Path
import natsort
```
</br>Changing the working directory to the directory in which the file is located such that input files  from this directory can be accessed.</br>
```python
os.chdir(os.path.dirname(os.path.abspath(__file__)))
CURR_DIR = os.getcwd()
```
</br>Finding the directory name (not the full path) and listing the input files.</br>
```python
dmnFileName = CURR_DIR.split('\\')[-1]

temp_files = Path(CURR_DIR).rglob('*.int') #reads .int files (input files) only

files = [x for x in temp_files]
```
</br>Making the skeleton.</br>
```python
values = []

# for A = 2.0
s2p0 = [a list of 33 numbers] % hidden data

# for A = 1.5
s1p5 = [a list of 33 numbers] % hidden data

# for A = 1.0
s1p0 = [a list of 33 numbers] % hidden data

# for A = 0.5
s0p5 = [a list of 33 numbers] % hidden data

n = float(dmnFileName.split('_')[1])

if n == 0.5:
    s = s0p5
elif n == 1.0:
    s = s1p0
elif n == 1.5:
    s = s1p5
elif n == 2.0:
    s = s2p0
```
</br>Reading each file, collecting information, processing and storing for future use.</br>
```python
for name in natsort.os_sorted(files):
    linesInHeader = 5
    file = open(name,'r')
    lines = file.readlines()[linesInHeader:]
    lines = [line.strip('\n') for line in lines]
    lines = [line.split(' ') for line in lines]
    integral = float(lines[-1][-1])
    values.append(integral)
    file.close()

values = [val * 1000 for val in values]

try:
    data1 = {"s": s[0:17],
            "value": values[0:17]}    
    df1 = pd.DataFrame(data1)
    print('\n', dmnFileName)
    print('\nIncreasing\n____________________')
    print(df1)
    df1.to_csv(dmnFileName + '-increasing.dat', index = False)

    data2 = {"s": s[16:33],
            "value": values[16:33]}    
    df2 = pd.DataFrame(data2)
    print('\nDecreasing\n____________________')
    print(df2)
    df2.to_csv(dmnFileName + '-decreasing.dat', index = False)    

    print("\n______Completed______\n")

except:
    print("\nPLEASE CHECK IF THE TOTAL NUMBERS OF .int FILES is 33.\n")

#input("\nHit Enter to return....\n")

```
{{< /note >}}