---
title: Python script for creating Java Script that populates a webform
date: 2021-04-12
weight: 42
menu:
  codes:
    name: For creating Java Script that populates a webform
    identifier: codes-python-creatingJavaScriptThatPopulatesAwebform
    parent: codes-python
    weight: 42
---
{{< note title="Python script for creating Java Script that populates a webform" >}}
This script reads the obtained marks of the students from an Excel file and creates a Java Script that can be used to populate a certain webform automatically. In short, data entry is not a problem anymore.
```python
#===================================================================#
#   Title:  JavaScript Maker for Marks input                        #
#   Author: Dhiman Roy, Dept. of ME (BUET)                          #
#   Date:   April 12, 2021                                          #
#   Licensed under Creative Commons                                 #
#===================================================================#

import os
import pandas as pd
from pathlib import Path

os.chdir(os.path.dirname(os.path.abspath(__file__)))

try:
    os.mkdir("JS")
    print("\n\tJS directory is created.")
except IOError:
    print("\n\tJS directory already exists.")


CURR_DIR = os.getcwd()
jsDir = os.path.join(CURR_DIR, "JS")
#print(CURR_DIR)
txt_folder = Path(CURR_DIR).rglob('*.xlsx') #reads .xlsx files only
#print(txt_folder)
files = [x for x in txt_folder]

#print(files)
#print("\n")

def midOrall():
    value = input("\n\tDo you want script for Mid (m) or All (a)? ")
    if value == 'm' or value == 'a':
        #print(value)
        return value
    else:
        print("\n\tPlease select one...")
        midOrall()

def quitOrNot():
    quitDmn = input("\t\tDo you want to quit? (Y/N): ")
    if quitDmn == 'y' or quitDmn == 'Y' or quitDmn == 'yes' or quitDmn == 'Yes' or quitDmn == 'YES':
        quit()
        
    elif quitDmn == 'n' or quitDmn == 'N' or quitDmn == 'no' or quitDmn == 'No' or quitDmn == 'NO':
        print()
    else:
        quitOrNot()
        
def mustQuit():
    input("\t\tHit enter to exit.")
    quit()

def stringconv(dmnMark, dmnRow, dmnSec, dmnFile, filename):
    try:
        return str(int(dmnMark))
    except:
        print("\t\tEmpty cell in SL %i, Sheet name: %s, Workbook: %s" %(dmnRow + 1, dmnSec, dmnFile))
        #quitOrNot()
        file.close()
        os.remove(filename)
        mustQuit()
        

if midOrall() == 'm':       
    for name in sorted(files):

        sheetForSection = pd.ExcelFile(name)
        section = sheetForSection.sheet_names
        fName = os.path.basename(name)
        
        substring = ['theory', 'THEORY', 'Theory']
        theoryfile = []
        if substring[0] in os.path.splitext(os.path.basename(name))[0] or substring[1] in os.path.splitext(os.path.basename(name))[0] or substring[2] in os.path.splitext(os.path.basename(name))[0]:
            header_val = 1
            theoryfile.append(os.path.basename(name)) 
        else:
            header_val = 0

        print("\n\tSections from %s" %fName)
        for sec in section:
            data = pd.read_excel(name, index_col = 0, usecols = "C:H", header = header_val, sheet_name = sec)
            #print(data)
            print("\t-%s" %sec)
            
            marks = data.values.tolist()
            #print(marks)
            filename = sec + "_" + os.path.splitext(os.path.basename(name))[0] + ".txt"
            #print(filename)
            if header_val == 1:
                file = open(os.path.join(jsDir, filename),"w")
                i = 0
                for each in marks:
                    if each[3] == 'abs':
                        file.write("document.getElementById('midterm_%d').value = '%s';\n" %(i, each[3]))               #mid absent
                    else:
                        file.write("document.getElementById('midterm_%d').value = '%s';\n" %(i, stringconv(each[3], i, sec, fName, filename)))     #mid
                    file.write("calTotal(midterm_%d);\n" %i)
                    i = i + 1
                
                file.close()
            else:
                print("\n\t...Sessionals are skipped...")
else:
    for name in sorted(files):

        sheetForSection = pd.ExcelFile(name)
        section = sheetForSection.sheet_names
        fName = os.path.basename(name)
        
        substring = ['theory', 'THEORY', 'Theory']
        theoryfile = []

        if substring[0] in os.path.splitext(os.path.basename(name))[0] or substring[1] in os.path.splitext(os.path.basename(name))[0] or substring[2] in os.path.splitext(os.path.basename(name))[0]:
            header_val = 1
            theoryfile.append(os.path.basename(name)) 
        else:
            header_val = 0

        print("\n\tSections from %s" %fName)
        for sec in section:
            data = pd.read_excel(name, index_col = 0, usecols = "C:H", header = header_val, sheet_name = sec)
            #print(data)
            print("\t-%s" %sec)
            
            marks = data.values.tolist()
            #print(marks)
            filename = sec + "_" + os.path.splitext(os.path.basename(name))[0] + ".txt"
            #print(filename)
            if header_val == 0:
                file = open(os.path.join(jsDir, filename),"w")
                i = 0
                for each in marks:
                    file.write("document.getElementById('attendance_%d').value = '%s';\n" %(i, stringconv(each[0], i, sec, fName, filename)))      #attendance
                    file.write("document.getElementById('performance_%d').value = '%s';\n" %(i, stringconv(each[1], i, sec, fName, filename)))       #classtest
                    if each[2] == 'abs':
                        file.write("document.getElementById('sessional_%d').value = '%s';\n" %(i, each[2]))               #sessional final absent
                    else:
                        file.write("document.getElementById('sessional_%d').value = '%s';\n" %(i, stringconv(each[2], i, sec, fName, filename)))     #sessoinal final
                    file.write("calTotal(sessional_%d);\n" %i)
                    i = i + 1
        
                file.close()

            else:
                file = open(filename,"w")
                i = 0
                for each in marks:
                    file.write("document.getElementById('attendance_%d').value = '%s';\n" %(i, stringconv(each[0], i, sec, fName, filename)))      #attendance
                    file.write("document.getElementById('classtest_%d').value = '%s';\n" %(i, stringconv(each[1], i, sec, fName, filename)))       #classtest
                    file.write("document.getElementById('assignment_%d').value = '%s';\n" %(i, stringconv(each[2], i, sec, fName, filename)))      #assignment
                    if each[4] == 'abs':
                        file.write("document.getElementById('final_%d').value = '%s';\n" %(i, each[4]))                 #final absent
                    else:
                        file.write("document.getElementById('final_%d').value = '%s';\n" %(i, stringconv(each[4], i, sec, fName, filename)))       #final
                    if each[3] == 'abs':
                        file.write("document.getElementById('midterm_%d').value = '%s';\n" %(i, each[3]))               #mid absent
                    else:
                        file.write("document.getElementById('midterm_%d').value = '%s';\n" %(i, stringconv(each[3], i, sec, fName, filename)))     #mid
                    file.write("calTotal(midterm_%d);\n" %i)
                    i = i + 1
                
                file.close()

print("\n\t\t====================")
print("\t\tScripts are ready!!!")
print("\t\t====================")
#print("\n")
input("\n\t\tPress enter to exit...\n")
```
{{< /note >}}
