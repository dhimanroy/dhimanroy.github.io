---
title: Python script for calculating average drag coefficient
weight: 24
menu:
  codes:
    name: For calculating average drag coefficient
    identifier: codes-python-calculateAverageDragCoefficient
    parent: codes-python
    weight: 24
---
{{< note title="Python script for calculating average drag coefficient" >}}
```python
#==========================================
# Title:  Average drag coefficient calculator
# Author: Dhiman Roy, ME-14, BUET
# Date:   March 28, 2021
# Licensed under Creative Commons.
#==========================================

import os
os.chdir(os.path.dirname(os.path.abspath(__file__)))
pardir = os.path.abspath(os.path.join(__file__, os.pardir))
resFilePath = "G:\Sumlation Results (Dhiman)"

def askToChange(parameter, val):
    print("%s is set to %f" %(parameter, val))
    prompt = input("Do you want to change? (Y/N): ")
    if prompt == 'y' or prompt == 'Y':
        val = input("Enter new value for %s: " %parameter)
    elif prompt == 'n' or prompt == 'N':
        val = val
    else:
        val = val
        #askToChange("Frontal Area", frontalArea)
    return float(val)

try:
    #Edit
    getValueFrom = 1000 #Start Claculation from
    getValueTo = 5000 #Upto which iteration
    linesInHeader = 2 #number of lines as header

    frontalArea = 0.74882341 #0.83443312 FOR SHIELD
    frontalArea = askToChange("Frontal Area", frontalArea)
    density = 1.225
    velocity = float(input("Enter velocity (m/s): "))

    #Think_before_making_any_change
    linesToSkip = getValueFrom + linesInHeader - 1

    file = open("cd-biker", "r")
    contents = file.readlines()[linesToSkip:(getValueTo + linesInHeader)]

    iter = []
    cd_each_iter = []
    #simpson = []
    i = 0
    sum = 0
    for line in contents:
        splitline = line.split("     ")
        iter.append(float(splitline[0]))
        cd_each_iter.append(float(splitline[1]))

    #print(len(iter))
    
    while i<len(iter) - 2:
        integral = (1 / 3) * (cd_each_iter[i] + 4 * cd_each_iter[i+1] + cd_each_iter[i+2])
        sum =  sum + integral
        #simpson.append(integral)
        i = i + 2
    
    avg_cd = sum / (iter[len(iter) - 1] - iter[0])
    dragForce = 0.5 * avg_cd * density * frontalArea * velocity * velocity
    #print("Sum is", sum)
    #print(iter)
    #print(cd_each_iter)
    #print(simpson)
    #print(sum)

    msg = "".join(["Average drag coefficient (from iteration ", str(getValueFrom), " to iteration ", str(getValueTo),") is ", str(avg_cd), " and drag on bike is ", str(dragForce), " N\n"])
    #print("Average drag coefficient from (iteration ", getValueFrom, " to iteration ", getValueTo,") is ", avg_cd,sep="")
    print("\n******************************************************************************\n", msg)

    try:
        savefile = open("avgerage_cd.txt", "a")
        savefile.write(msg)
        print(" Data stored!!\n******************************************************************************\n")
    except IOError:
        print("Couldn't open file to save!")
    finally:
        if "savefile" in locals():
            savefile.close()

except IOError:
    print("Couldn't open file!")
finally:
    if "file" in locals():
        file.close()

def writeAllRes():
    try:
        res = "".join([pardir, ",", str(avg_cd), ",", str(dragForce), "\n"])
        allResultToFile = open(resFilePath + "\All-Simulation_Result.csv", "a")
        allResultToFile.write(res)
        print('Allres saved')
    except IOError:
        print("Couldn't open 'All-Simulation_Result.csv' to store data!")
    finally:
        if "allResultToFile" in locals():
            allResultToFile.close()

try:
    os.mkdir(resFilePath)
    print('Directory ')
    writeAllRes()
except:
    writeAllRes()

    

input("Hit Enter to exit..........\n")
```
{{< /note >}}
