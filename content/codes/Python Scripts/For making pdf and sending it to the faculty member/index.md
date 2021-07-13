---
title: Python script for making PDF of exam routine and sending it to respective faculty member
weight: 26
menu:
  codes:
    name: For making PDF of exam routine and sending mail
    identifier: codes-python-makingPDFandsend
    parent: codes-python
    weight: 26
---
{{< note title="Python script for making PDF of exam routine and sending it to respective faculty member" >}}
This script reads exam schedule from an Excel file and creates a pdf that is sent to the respective faculty member. This script helped me a lot while performing as an exam coordinator in Sonargaon University (SU).
```python
#===================================================================#
#   Title:  PDF Maker for faculties                                 #
#   Author: Dhiman Roy, Dept. of ME (BUET)                          #
#   Date:   April 04, 2021                                          #
#   Licensed under Creative Commons                                 #
#===================================================================#

import os
os.chdir(os.path.dirname(os.path.abspath(__file__)))
import pandas as pd
from fpdf import FPDF

myFile = "routine.xlsx"
data = pd.read_excel(myFile, usecols = ["Sort By","Date","Day","Slot","ERP Section Name","Course Code","Course Title","Teacher","Designation","Class Schedule"], header = 2)

teachers = data["Teacher"].unique().tolist()
print(teachers)
def makePdf(df, teacher):
    titles = df.columns.tolist()
    #print(titles )
    dfData = df.values.tolist()
    dfData.insert(0, titles)
    #print(dfData[0])
    myPdf = FPDF()
    myPdf.add_page("L")

    myPdf.set_font("Times", size = 16)

    myPdf.cell(0, 10, txt = "Routine for Final Assessment, Spring 21", ln = 1, align = "C")
    myPdf.cell(0, 10, txt = "Department of Mechanical Engineering", ln = 1, align = "C")
    myPdf.cell(0, 10, txt = "Sonargaon University (SU)", ln = 1, align = "C")
    myPdf.set_font("Times", size = 10)   
    myPdf.cell(0, 10, txt = "", ln = 1)
    myPdf.set_font("Times", "I",size = 12)
    myPdf.cell(0, 10, txt = "Teacher: " + teacher, align = "L")
    myPdf.cell(0, 10, txt = "Designation: " + df["Designation"].tolist()[0], ln = 1, align = "R")
    i=3
       
    for dat in dfData:
        if i == 3:
            myPdf.set_font("Times", "B",size = 12)
        else:
            myPdf.set_font("Times", "",size = 10)
        myPdf.cell(25, 10, txt = dat[0], border = 1, align = "C")
        myPdf.cell(25, 10, txt = dat[1], border = 1, align = "C")
        myPdf.cell(35, 10, txt = dat[2], border = 1, align = "C")
        myPdf.cell(25, 10, txt = dat[4], border = 1, align = "C")
        myPdf.cell(120, 10, txt = dat[5], border = 1, align = "C")
        myPdf.cell(45, 10, txt = dat[3], border = 1, align = "C", ln = 1)
        i = i + 1
    myPdf.output(teacher + ".pdf")
    print(teacher + ".pdf is created.")
i = 1
for tea in teachers:
    dmnData = data.loc[data.Teacher == tea].sort_values(by = "Sort By")     #filters data using teachers list

    del dmnData["Sort By"]

    dmnData["Date"] = dmnData["Date"].dt.strftime("%B %d, %Y")                      #Converts date to string

    #print(dmnData)

    makePdf(dmnData, tea)
    i = i + 1

print("\n*******\nTotal %d files are saved.\n********\n" %(i-1))

input("Hit Enter to exit...")
```

<br />
The following script can send the generated pdf files to the respective faculty member.

```python
#===================================================================#
#   Title:  Email Sender                                            #
#   Author: Dhiman Roy, Dept. of ME (BUET)                          #
#   Date:   April 05, 2021                                          #
#   Licensed under Creative Commons                                 #
#===================================================================#

import os
os.chdir(os.path.dirname(os.path.abspath(__file__)))
import pandas as pd
import smtplib
from email.message import EmailMessage
from email.header import Header
from email.utils import formataddr
import glob
files = glob.glob("*.pdf") #reads pdf files only
#print(files)

log = open("log.txt","w")
log.close()
senderName = "Exam Coordination Team, Dept. of ME, SU"
senderEmail = "emailaddress@gmail.com"
pw = input("Password: ")

with open("index.html") as htmlFile:
    htmlCode = htmlFile.read()
    htmlCode = htmlCode.split("Faculties")
    

def sendEmail(i, tea, mailList):
    msg = EmailMessage()
    msg["Subject"] = "Revised Routine for Final Assessment of Spring 2021"
    msg["From"] = formataddr((str(Header(senderName , 'utf-8')), senderEmail))
    for mail in mailList:
        if tea in mail[0]:
            mailAdd = mail[1]
            msg["To"] = mailAdd
            #print(mailAdd)
    '''
    with open("messageBody.txt") as textFile:
        text = textFile.read()
        msg.set_content(text)
    '''
    newHtmlCode = htmlCode[0] + tea + htmlCode[1]
    msg.add_alternative(f"""\n\n{newHtmlCode}""", subtype = "html")

    with open(tea + "-revised.pdf","rb") as attachment: #pdf files are needed to be ready
        fileData = attachment.read()
        fileName = attachment.name
        msg.add_attachment(fileData, maintype = "application", subtype = ".pdf", filename = fileName)
    '''
    try:
        with open(tea + ".pdf","rb") as attachment:
            fileData = attachment.read()
            fileName = attachment.name
            msg.add_attachment(fileData, maintype = "application", subtype = ".pdf", filename = fileName)
    except IOError:
        print("%s.pdf is not found!!!" %tea)
    '''
    with smtplib.SMTP_SSL("smtp.gmail.com", 465) as server:
        server.login(senderEmail,pw)
        server.send_message(msg)
        print("%02d. Successfully sent %s.pdf to %s" %(i, tea, mailAdd))
        log.writelines("%02d. Successfully sent %s.pdf to %s\n" %(i, tea, mailAdd))

################## Edit this
classRoutine = "ClassRoutine.xlsx" #get mail ids from
##################

routine_data = pd.read_excel(classRoutine, usecols = ["Teacher","Email"], header = 6)
allData = routine_data.values.tolist()

################## edit this
myFile = "routine.xlsx" #gets exam routine from
##################

data = pd.read_excel(myFile, usecols = ["Teacher"], header = 2)
teachers = data["Teacher"].unique().tolist()
#print(teachers)
listOfMails=[]
teachermails = []
try:
    for teacher in allData:
        if teacher[0] in teachers:
            if teacher[0] not in teachermails:
                teachermails.append(teacher[0])
                listOfMails.append([teacher[0], teacher[1]])
            
except IOError:
    print("listOfMails not complete.")

#print(teachermails)
#print(allData)
#print(listOfMails)
up_teachers = ["Mr. X", "Dr. Y"]

print("Sending emails. Please wait.")

i = 1
for everyone in up_teachers:
    log = open("log.txt","a")
    try:
        sendEmail(i, everyone, listOfMails)
    except IOError:
        print("%02d. sendEmail did not run for %s" %(i, everyone))
        log.writelines("%02d. sendEmail did not run for %s\n" %(i, everyone))
    i = i + 1
    log.close()
input("Hit Return to exit...")
```

{{< /note >}}
