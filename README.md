# DADS5001_Quiz1_6420422011
How to create a simple GUI app for managing your data. The created app must have CRUD functions by Mongodb & Pysimplegui






# Overview : Simple App for update sales
เป็นโปรแกรมอย่างง่ายที่บันทึกข้อมูลยอดขายรายเดือนโดยภาพรวมการทำงานมีดังนี้

#### 1. Menu Bar แบ่งเป็น 2 แถบ คือ File และ Exit
   1.1 File แบ่งเป็น Create sales และ Delete sales
   ![Picture1](https://user-images.githubusercontent.com/115800837/204842070-7d4b7af4-f564-4cdf-b8e2-0deac81fb5ce.png)
   
   1.2 Exit จะมี Quit เป็นทางออกอีกทางนอกจาก เครื่องหมายปิด x ด้านบนด้านขวาของหน้าต่าง
   ![Picture2](https://user-images.githubusercontent.com/115800837/204842089-14c1da4f-fc2e-48f4-92d0-8a7f3e1adad0.png)


# Tools
#### 1. Mongodb Compass :a non-relational document database that provides support for JSON-like storage
สำหรับลงในเครื่อง PC ใช้งานได้ง่าย (บางคนสามารถใช้แบบ cloud ได้ การใช้งานก็ง่ายเหมือนกันค่ะ)
![image](https://user-images.githubusercontent.com/115800837/204842983-e9896f72-892c-4fcf-9434-1a3577efe64f.png)

#### 2. Pysimplegui : Python package that enables Python programmers of all levels to create GUIs. 
![image](https://user-images.githubusercontent.com/115800837/204843899-25b0c7db-c121-4a91-924a-4041478010a3.png)


# How to create & Source Code
#### 1. Pip imstall mongoengine & pysimplegui
```
pip install pysimplegui
pip install mongoengine

```


#### 2. Connecting Database (Localhost Case) 
```
from mongoengine import *

connect(host="mongodb://127.0.0.1:27017/Sales")

```


#### 3. Connecting between database and GUI App / Create CRUD Functionally (Only Create/Read/Delete Case)
```
from mongoengine import connect, Document, fields

class User(Document):
    meta = {'collection':'S2022'}
    year = fields.StringField(required=True)
    month = fields.StringField(required=True)
    sales = fields.StringField(required=True)
    cost = fields.StringField(required=True)

def create_sale(year, month, sales, cost):
    x = User() 
    x.year = year
    x.month = month
    x.sales = sales
    x.cost = cost
    x.save()

def query_contact(month):
    x = User.objects(month=month)
    return x

def delete_sales(month):
    x = query_contact(month)
    x.delete()
    
```

#### 4. Creating GUI Interface
```
sg.theme('SandyBeach')

def create_sales_gui():
    sg.theme('DarkGray')
    layout = [[sg.Text('Please enter product sales info')],
              [sg.Text('Year :', size=(15,1)),sg.Combo(['2022','2023'],key='YEAR')],
              [sg.Text('Month :', size=(15,1)),
               sg.Combo(['JANUARY','FEBUARY','MARCH','APRIL','MAY','JUNE','JULY','AUGUST','SEPTEMBER','OCTOBER','NOVEMVER','DECEMBER']
                        ,key='MONTH')],
              [sg.Text('Sales (Monthly) :', size=(15,1)), sg.Input(key='SALES',do_not_clear=False)],
              [sg.Text('Cost (Monthly) :', size=(15,1)), sg.Input(key='COST',do_not_clear=False)],
              [sg.Button('Ok'), sg.Button('Cancel')] ]
    
    window = sg.Window('Create sales', layout, element_justification='left')
    
    while True:
        event, values = window.read()
        if event == sg.WIN_CLOSED or event == 'Cancel':
            break
        if event == 'Ok':
            year = values['YEAR']
            month = values['MONTH']
            sales = values['SALES']
            cost = values['COST']
            create_sale(year, month, sales, cost)
        for key in values:
            window[key].update('')
        print(event, values)
        sg.popup('File saved')
                   
def delete_sales_gui():
    month = sg.popup_get_text('Please enter sales month')
    x = query_contact(month)
    if x:
        response = sg.popup_ok('Are you sure?')
        if response:
            delete_sales(month)
    else:
        sg.popup('User does not exist')

def main():
    menu = [['File',['Create sales','Delete sales']],
            ['Exit',['Quit']]]
    layout = [[sg.Menu(menu)],
              [sg.Image(r'C:\Users\Windows10\DADS5001\Mongodb_quiz\graph1.png')]]

# Create the Window
    window = sg.Window('Sales 2022', layout)

# Event Loop to process "events" and get the "values" of the inputs
    while True:
        event, values = window.read()
        if event == sg.WIN_CLOSED or event == 'Quit':
            break
        elif event == 'Create sales':
            create_sales_gui()
        elif event == 'Update sales':
            update_sales_gui()
        elif event == 'Delete sales':
            delete_sales_gui()
        elif event == 'Ok':
            print(event, values)
    
    window.close()
if __name__ == '__main__':
    main()
    
```

# Video Presentation
Link : https://www.canva.com/design/DAFTV9FZXt4/OXzlodzFFATZhXNbQyyV_g/edit?utm_content=DAFTV9FZXt4&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton

# Challenge
- แสดงข้อมูลที่มีรายละเอียดได้มากกว่านี้เช่น รวมข้อมูลเป็นรายวันและสรุปเป็นรายเดือนปี รวมถึงการทำ visualization ต่างๆ แต่เนื่องจากยังไม่ชำนาญในการแปลงเป็น code จึงทำให้ใช้เวลาค่อนข้างนาน จะพยายามฝึกฝนให้ชำนาญมากขึ้นเพื่อนำไปใช้ประโยชน์ได้ดีขึ้นค่ะ 
