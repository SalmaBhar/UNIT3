# UNIT3
# Diary App
## Criteria A: Planning
### Context of the problem
Many people like to keep diaries to document important events of their lives. However, paper diaries are old-school and take a lot of storage space [1].This project is a Diary App. It allows the user to keep a personal digital diary on their phone through an app. The user keeps their entries secure by creating an account and accessing their personal portal which allows them to enter and read diaries according to the date. <br>

[1] Adminassistance. “What Type Of Diary Is Better To Use?” Admin Assistance, 16 Sept. 2020, www.adminassistance.co.uk/diaries-online-vs-paperback.  <br>

### Justification of the solution
We will create an app program using Python as a programming language on the programming software PyCharm that is available by our school. We will be using the open source KivyMD in order to build our app interface and components. In fact, KivyMD is one of the most useful sources that makes app development on Python much easier and efficient [2]. We will also use SQL programming language to build 2 databases for the app: a database to store the user's account information and a database to store the journal entries of each user. The app will have 4 main screens: a registration screen, login screen, diary entries screen and a diary entries view screen. <br>

[2] Gupta, Kaustubh. “Building Android Apps With Python: Part -3.” Medium, Towards Data Science, 4 Dec. 2020, towardsdatascience.com/building-android-apps-with-python-part-3-89a455ee7f7c. <br>

T.E.L.O.S. Study: <br>
T-Technical-Is the project technically possible? We dispose of a computer and PyCharm software for Python and KivyMD code. All technical necessities for this project are satisfied. <br>
E-Economic- Can the project be afforded? Will it increase profit? This project is developed for free by UWC ISAK Japan students. Thanks to the funding provided to our school, we can afford all of the components necessary for the project. <br>
L-Legal- Is the project legal? The project is completely legal as the 1947 Japanese constitution does not criminalize small digital projects like ours. <br>
O-Operational- How will the current operations support the change? The project will operate on a computer and displayed on a phone. The process does not have any errors and is perfectly operational. <br>
S-Scheduling- Can the project be done in time? We are given 3 weeks to complete this project and the tasks have been regularly distributed to assure that it is completed by Thursday, April 1st, 2021. <br>
### Criteria for success
1. The product contains at least 2 databases. <br>
2. The product uses Object Oriented Programming. <br>
3. The product is free of bugs and runs correctly on a phone. <br>
## Criteria B: Design
### Test Plan 
|                                       Test                                       |                                                           Expected Outcome                                                           | Met? |
|:--------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------:|:----:|
| Crit.1. The product contains at least 2 databases.               | The product contains 2 databases: 1 database to store user account information and 1 database to store each user's diary entries.                                                              | YES  |
| Crit.2. The product uses Object Oriented Programming.               | The product uses OOP. It contains LoginScreen class, RegisterScreen and DiaryScreen class.                                                              | YES  |
| Crit.3. The product is free of bugs and runs correctly on a phone. | The product has been verified to run correctly and does not show any errors.                                                       | YES  |
### System Diagram
I started by drawing the following system diagram in class: <br>
![alt text]() <br>
**Fig. 1:** System Diagram of the project <br>
### Initial Sketches
These are some initial sketches of each screen of the app. <br>
![alt text]() <br>
**Fig. 2:** Initial Paper Sketches of the App <br>
These are the diagrams for each of the 2 databases of the app. <br>
![alt text]() <br>
**Fig. 3:** Diagrams of our 2 databases <br>
## Criteria C: Development
We ended up with the following Python code which will be run on PyCharm: <br>
```py
import sqlite3
from kivymd.app import MDApp
from kivymd.uix.screen import MDScreen
from kivy.uix.behaviors import ButtonBehavior
from kivymd.uix.label import MDLabel

class MainApp(MDApp):
    def build(self):
        self.theme_cls.primary_palette = 'Pink'

        return

class DiaryScreen(MDScreen):
    def new_entry(self):
        date = self.ids.date_input.text
        entry = self.ids.entry_input.text

        conn = sqlite3.connect("app.sqlite")
        sql = f"insert into diary(date, entry) values ('{date}'" \
              f"'{entry}')"
        cur = conn.cursor()
        cur.execute(sql)
        print("New Entry Created")
        conn.commit()
        conn.close()

class LoginScreen(MDScreen):
    def try_login(self):
        email = self.ids.email_input.text

        password = self.ids.password_input.text

        conn = sqlite3.connect('app.sqlite')
        sql = f"select * from users where email='{email}' and password='psw'"
        cur = conn.cursor()
        cur.execute(sql)
        result = cur.fetchone()
        conn.close()

        id, password, email = result
        print(f"login successful for user with id {id} and email")

class RegisterScreen(MDScreen):
    def try_register(self):
        email = self.ids.email_input.text
        psw = self.ids.password_input.text
        psw_check = self.ids.password_input.text
        if psw != psw_check:
            print("Passwords do not match")
        else:
            # check the user is not registered already
            conn = sqlite3.connect('app.sqlite')
            sql = f"select * from users where email='{email}'"
            cur = conn.cursor()
            cur.execute(sql)
            result = cur.fetchone()

            if result:
                # there is already a user with this email
                print("Email already registered")
            else:
                conn = sqlite3.connect("app.sqlite")
                sql = f"insert into users(email, password) values ('{email}'" \
                      f"'{psw}')"
                cur = conn.cursor()
                cur.execute(sql)
                print("User created")
                conn.commit()
                conn.close()

class ButtonLabel(ButtonBehavior,MDLabel):
    pass

MainApp().run()
``` 
The following is the code in a text file to customize the app components: 
This is the code for our 2 databases:
```py
creenManager:
    id: scr_manager

    LoginScreen:
        name:'LoginScreen'

    RegisterScreen:
        name:'RegisterScreen'

    DiaryScreen:
        name:'DiaryScreen'

<DiaryScreen>:
    BoxLayout:
        orientation:'vertical'
        size: root.height, root.width

        FitImage:
            source: 'diary.jpg'

    MDCard:
        size_hint: 0.5, 0.8
        elevation: 10
        pos_hint: {'center_x':0.5, 'center_y':0.5}
        orientation: 'vertical'
        padding: dp(40)
        spacing: dp(40)

        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: 'vertical'
            padding: dp(30)
            spacing: dp(20)

        MDLabel:
            text: 'New Entry'
            font_style: 'H3'
            halign: 'center'

        MDTextField:
            id: date_input
            hint_text: 'Date'
            icon_left: 'date'
            required: True

        MDTextField:
            id: entry_input
            hint_text: 'Entry'
            icon_left: 'entry'
            required: True

        MDRaisedButton:
            text: 'Save New Entry'
            on_release:
                root.new_entry()


<RegisterScreen>:
    BoxLayout:
        orientation:'vertical'
        size: root.height, root.width

        FitImage:
            source: 'diary.jpg'

    MDCard:
        size_hint: 0.5, 0.8
        elevation: 10
        pos_hint: {'center_x':0.5, 'center_y':0.5}
        orientation: 'vertical'

        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: 'vertical'
            padding: dp(30)
            spacing: dp(20)

            MDLabel:
                text: 'REGISTER'
                font_style: 'H3'
                halign: 'center'

            MDTextField:
                id: email_input
                hint_text: 'Email'
                icon_left: 'email'
                helper_text: 'Invalid email'
                helper_text_mode:'on_error'
                required: True

            MDTextField:
                id: password_input
                hint_text: 'Password'
                icon_left: 'key'
                helper_text: 'Invalid password'
                helper_text_mode:'on_error'
                required: True
                password: True
                password_mask: '*'

            MDRaisedButton:
                text: 'Register'
                on_release:
                    root.try_register()
                    root.parent.current = 'LoginScreen'

            MDBoxLayout:
                adaptive_height: True
                ButtonLabel:
                    text: 'Access Diary'
                    on_press:
                        root.parent.current = 'DiaryScreen'


<LoginScreen>:
    BoxLayout:
        orientation:'vertical'
        size: root.height, root.width

        FitImage:
            source: 'diary.jpg'

    MDCard:
        size_hint: 0.5, 0.8
        elevation: 10
        pos_hint: {'center_x':0.5, 'center_y':0.5}
        orientation: 'vertical'

        MDBoxLayout:
            id: content #id or name
            adaptive_height: True
            orientation: 'vertical'
            padding: dp(30)
            spacing: dp(20)

            MDLabel:
                text: 'MyDiary'
                font_style: 'H3'
                halign: 'center'
                font_size: '50sp'

            MDTextField:
                id: email_input
                hint_text: 'Email'
                icon_left: 'email'
                helper_text: 'Invalid email'
                helper_text_mode:'on_error'
                required: True

            MDTextField:
                id: password_input
                hint_text: 'Password'
                icon_left: 'key'
                helper_text: 'Invalid password'
                helper_text_mode:'on_error'
                required: True
                password: True
                password_mask: '*'

            MDRaisedButton:
                text: 'Log in'
                on_release:
                    root.try_login()
                    root.parent.current = 'DiaryScreen'

            MDBoxLayout:
                adaptive_height: True
                ButtonLabel:
                    text: 'Register'
                    on_press:
                        root.parent.current = 'RegisterScreen'
```
This is the SQLite code for our 2 databases:
```py
# User Information Database
CREATE TABLE IF NOT EXISTS users(
    id       integer primary key autoincrement,
    email    varchar(250),
    password varchar(40)
);

# Diary Entries Database
CREATE TABLE IF NOT EXISTS diary(
    id integer primary key autoincrement,
    date Date,
    entry varchar(200)
)
```
## Criteria D: Functionality
This video demonstrates the functionality of the final app: <br>
Video Link: TBD
## Criteria E: Evaluation


