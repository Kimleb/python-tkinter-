from tkinter import *
import os
from datetime import date
from tkinter import Entry, Button
from tkinter import messagebox
from tkinter.ttk import Combobox
import pymysql
from PIL import Image, ImageTk
from tkinter import filedialog
import cryptography
import mysql.connector


#Functionalities
background="#06283D"
framebg="#EDEDED"
framebg="#06283D"


def connect_database():
    try:
        # Establish the database connection
        con = pymysql.connect(
            host='localhost',
            user='root',
            password='42374237',)

        # Create a cursor object to execute SQL queries
        mycursor = con.cursor()

    except pymysql.Error as e:
        messagebox.showerror('Error', 'Database connectivity issue. Please try again.')
        return


    try:
        mycursor.execute('use patients1')
        # Insert data into the table
        query = 'INSERT INTO patients1(registrationid, dateenrty,fullname,gender,phonenumber,' \
                'dateofbirth,bloodgrp,weight,bloodpressure,allergies,nextofkinname,'\
                'nextofkinphonenumber,doctorscomments) VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s)'

        mycursor.execute(query,(reg_entry.get(), Date.get(), fullname.get(), Gender.get(), phonenumber.get(),
                                dateofbirth.get(),
             Bloodgrp.get(), weight.get(), bloodpressure.get(), allergies.get(), nextofkin.get(),
             Nextofkinphonenumber.get(),doctorscomment.get('1.0')))

        con.commit()
        con.close()
        messagebox.showinfo('Success', 'Registration is successful')
        clear()
    except pymysql.Error as e:
        messagebox.showerror('Error', 'Error inserting data. Please try again.')

root=Tk()
root.title("Patient Registration System")
root.geometry('1250x700')
root.config(bg=background)




#functionality port

#Gender radio

#exit
def Exit():
    root.destroy()

save = Button(command=connect_database)

    ##### Registration No.######
def registration_ID():

    try:
        Registration.set()
    except:
        Registration.set("1/23")

####Clear#####
def clear():
    global img
    fullname.set('')
    Date.set('DD/MM/YY')
    dateofbirth.set('DD/MM/YY')
    phonenumber.set('')
    Bloodgrp.set('select Bloodgrp')
    weight.set('Select weight')
    allergies.set('')
    Nextofkinphonenumber.set('')
    nextofkin.set('')
    bloodpressure.set('')






#top frames
Label(root,text="Email: Kimlebhealthcareservices@gmail.com",
      width=10,height=3,bg="#f0687c",anchor='e').pack(side=TOP,fill=X)
Label(root,text="PATIENT REGISTRATION",
      width=10,height=2,bg="#c36464",fg='#fff',font='arial 20 bold').pack(side=TOP,fill=X)

imageicon4=PhotoImage(file="Layer 4.png")
update_button=Button(root,image=imageicon4,bg="#c36464")
update_button.place(x=110,y=64)

#search box to update
search=StringVar()
Entry(root,textvariable=search,width=15,bd=2,font="arial 20").place(x=820,y=70)
imageicon3=PhotoImage(file="search.png")
srch=Button(root,text="search",compound=LEFT,image=imageicon3,width=123,bg="#68ddfa",font="arial 13 bold")
srch.place(x=1060,y=66)

# Registration and date
Label(root,text="REGISTRATION NO:",font="arial 13",fg='White',bg=background).place(x=30,y=150)
Label(root,text=" DATE OF REGISTRATION:",font="arial 13",fg='White',bg=background).place(x=310,y=150)

Registration=StringVar()
Date = StringVar()

reg_entry= Entry(root,textvariable=Registration,width=15,font='arial 10')
reg_entry.place(x=180,y=150)

registration_ID()

#Date

today = date.today()
d1 = today.strftime("%d/%m/%y")
date_entry = Entry(root,textvariable=Date,width=15,font="arial 10")
date_entry.place(x=520,y=150)
Date.set(d1)


#patient details
obj=LabelFrame(root,text="Patient's Details",font=20,bd=2,width=900,bg="gray56",fg='White',height=250,relief=GROOVE)
obj.place(x=30,y=200)

#fullname

Label(root,text="FULL NAME:",font="arial 13",fg='White',bg=background).place(x=30,y=230)
fullname=StringVar()
fullname_entry= Entry(root,width=15,font='arial 10')
fullname_entry.place(x=140,y=230)

#Phone number

Label(root,text="PHONE NUMBER:",font="arial 13",fg='White',bg=background).place(x=260,y=230)
phonenumber=StringVar()
phonenumber= Entry(root,width=15,font='arial 10')
phonenumber.place(x=420,y=230)

#Gender
Label(root,text="Gender:",font="arial 13",fg='White',bg=background).place(x=30,y=320)

Gender=StringVar()

Gender_entry= Entry(root,width=15,font='arial 10')
Gender_entry.place(x=120,y=320)

#date of birth
Label(root,text="DOB:",font="arial 13",fg='White',bg=background).place(x=30,y=280)
#date today birth
dateofbirth=StringVar()
today = date.today()
d1 = today.strftime("%d/%m/%y")
dateofbirth_entry = Entry(root,width=15,font="arial 10")
dateofbirth_entry.place(x=100,y=280)
dateofbirth.set(d1)



#blood group
Label(root,text="BLOOD GROUP:",font="arial 13",fg='White',bg=background).place(x=30,y=350)
Bloodgrp=StringVar()
Bloodgrp= Combobox(root,values=['A RhD (A+)','A RhD  (A-)','B RhD (B+)',
                                'B RhD  (B-)','O RhD (O+)','O RhD  (O-)',
                                'AB RhD (AB+)',
                                'AB RhD  (AB-)',],font="roboto 10",width=10,state="r")
Bloodgrp.place(x=200,y=350)
Bloodgrp.set("select Bloodgrp")

#weight
Label(root,text="WEIGHT:",font="arial 13",fg='White',bg=background).place(x=30,y=390)
weight=StringVar()
weight= Combobox(root,values=['0-10 kgs','10-20kgs','20-30 kgs','30-40kgs','40-50kgs','50-60kgs',
                              '60-70kgs','70-80kgs','80-90kgs','90-100kgs','100-110kgs',],font="roboto 10",width=10,state="r")
weight.place(x=120,y=390)
weight.set("select weight")



#Allergies

Label(root,text="ALLERGIES PRESENT:",font="arial 13",fg='White',bg=background).place(x=250,y=320)
allergies=StringVar()
allergies_entry= Entry(root,width=15,font='arial 10')
allergies_entry.place(x=460,y=320)

#Bloodpressure

Label(root,text="Bloodpressure:",font="arial 13",fg='White',bg=background).place(x=260,y=280)
bloodpressure=StringVar()
bloodpressure_entry= Entry(root,width=15,font='arial 10')
bloodpressure_entry.place(x=450,y=280)


#next of kin's details
obj=LabelFrame(root,text="Next of Kin's Details",font=20,bd=0,width=650,bg="gray56",fg='White',height=100,relief=GROOVE)
obj.place(x=30,y=520)



Label(root,text="Next of kin's name:",font="arial 13",fg='White',bg=background).place(x=30,y=560)
nextofkin=StringVar()
nextofkin_entry= Entry(root,width=15,font='arial 10')
nextofkin_entry.place(x=200,y=560)


Label(root,text=" Next of kin phone number:",font="arial 13",fg='White',bg=background).place(x=400,y=560)
Nextofkinphonenumber=StringVar()
phone_value = Nextofkinphonenumber.get()

Nextofkinphonenumber= Entry(root,width=15,font='arial 10')
Nextofkinphonenumber.place(x=560,y=560)

#patients image
f=Frame(root,bd=3,bg="black", width=200,height=200,relief=GROOVE)
f.place(x=1000,y=150)

upload=PhotoImage(file="upload photo.png")
lbl=Label(f,bg="black",image=upload)
lbl.place(x=0,y=0)

#doctors comment ######

Label(root,text="Doctor's comments:",font="arial 13",fg='White',bg=background).place(x=590,y=220)
doctorscomment=StringVar
doctorscomment_entry = Entry(root,width=16,bd=4)
doctorscomment_entry.place(x=760,y=220)

#Button
Button(root,text="Upload",width=19,height=2,font="arial 12 bold",bg="Lightblue").place(x=1000,y=370)

Button(root,text="Save",width=19,height=2,font="arial 12 bold",bg="Lightgreen",command=connect_database).place(x=1000,y=450)

Button(root,text="Reset",width=19,height=2,font="arial 12 bold",bg="pink",command=clear).place(x=1000,y=530)

Button(root,text="Exit",width=19,height=2,font="arial 12 bold",bg="turquoise",command=Exit).place(x=1000,y=610)
root.mainloop()
