# UNIT3
# Diary App
## Criteria A: Planning
### Context of the problem
Many people like to keep diaries to document important events of their lives. However, paper diaries are old-school and take a lot of storage space [1].This project is a Diary App. It allows the user to keep a personal digital diary on their phone through an app. The user keeps their entries secure by creating an account and accessing their personal portal which allows them to enter and read diaries according to the date. <br>

[1] Adminassistance. “What Type Of Diary Is Better To Use?” Admin Assistance, 16 Sept. 2020, www.adminassistance.co.uk/diaries-online-vs-paperback.  <br>

To support this idea, I received a request from my roomate asking about an app to record her diary entries since her physical journal is taking too much space in her small dorm room (see appendix). <br>

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
1. The product should allow the customer to store their account info and diary entries. <br>
2. The product contains a login feature. <br>
3. The product contains a register feature. <br>
4. The product should have a pink theme. <br>
## Criteria B: Design
### Test Plan 
|                                       Test                                       |                                                           Expected Outcome                                                           | Met? |
|:--------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------:|:----:|
| Crit.1. The product should allow the customer to store their account info and diary entries.               | The product contains 2 database tables: 1 database to store user account information and 1 database to store each user's diary entries.                                                              | YES  |
| Crit.2. The product contains a login feature.               | When you open the app, you can enter your email and password in the allocated fields and you get signed in when you click the "sign in" button.                                                              | YES  |
| Crit.3. The product contains a register feature. | You can enter an email and password in the allocated fields and your account gets added to the users database when you click the "register" button.                                                       | YES  |
| Crit.4. The product should have a pink theme. | The app's theme has been set at the colour pink which is visible in the background and buttons.                                                       | YES  |
### Initial Sketches
I started by making some initial sketches of each screen of the app. It shows the components and its location on each screen. These sketches will be later used as a refrence when coding the app's screens. <br>
![alt text](sketchesapp.jpg) <br>
**Fig. 1:** Initial Paper Sketches of the App <br>
<br>
The app contains 2 database tables: the users table to store the users' accounts information and the diary table to store the diary entries of each user. These are the Entity Relationships (ER) diagrams for the 2 database tables of the app. <br>
![alt text](erapp.jpg) <br>
**Fig. 2:** ER Diagrams of our 2 database tables <br>
### User Interface
This is the Home Login Screen and Register Screen of the app. <br>
![alt text](loginregisterscreen.png) <br>
**Fig. 3:** Login and Register Screens <br>
<br>
This is the Diary Entries Screen which allows the user to enter their journal entries after they have logged-in. <br>
![alt text](entryscreen.png) <br>
**Fig. 4:** Diary entries Screens <br>
<br>
The following are the 2 database tables with some data in them. The app allows us to write and read from them using the sign-in/register and diary entry functions. <br>
![alt text](databasetables.png) <br>
**Fig. 5:** 2 database tables of the app <br>
## Criteria C: Development
The following is the KivyMD code file to create the 4 needed screens of the app: 
```py
ScreenManager:
    id: scr_manager

    LoginScreen:
        name:'LoginScreen'

    RegisterScreen:
        name:'RegisterScreen'

    EntryScreen:
        name:'EntryScreen'

    DiaryScreen:
        id: diary
        name:'DiaryScreen'
```
### Login Screen
The Log-in screen is the first and default screen of the app.The following is the Python code for the login function which is run on PyCharm: <br>
```py
class LoginScreen(MDScreen):
    def try_login(self):
        email = self.ids.email_input.text

        password = self.ids.password_input.text

        conn = sqlite3.connect('app.sqlite')
        sql = f"select * from users where email='{email}' and password='{password}';"
        cur = conn.cursor()
        cur.execute(sql)
        result = cur.fetchone()
        conn.close()
        print(result)
        id, password, email = result
        print(f"login successful for user with id {id} and email {email}")
        self.parent.ids.diary.ids.test_label.text = f"{email}"
``` 
### Register Screen
If the user does not have an account, they will have to register for an account. The following is the Python code for the register function which is run on PyCharm: <br>
```py
class RegisterScreen(MDScreen):
    def try_register(self):
        email = self.ids.email_input.text
        password = self.ids.password_input.text
        password_check = self.ids.password_input.text
        if password != password_check:
            print("Passwords do not match")
        else:
            # check the user is not registered already
            conn = sqlite3.connect('app.sqlite')
            sql = f"select * from users where email='{email}' and password='{password}';"
            cur = conn.cursor()
            cur.execute(sql)
            result = cur.fetchone()

            conn = sqlite3.connect("app.sqlite")
            sql = f"insert into users(email, password) values ('{email}', '{password}');"
            cur = conn.cursor()
            cur.execute(sql)
            print("User created")
            conn.commit()
            conn.close()
``` 

## Criteria D: Functionality
The functionality of the app has been presented in class.
## Criteria E: Evaluation
As a reminder, this is the criteria for success of this app: <br>
1. The product should allow the customer to store their account info and diary entries. <br>
2. The product contains a login feature. <br>
3. The product contains a register feature. <br>
4. The product should have a pink theme. <br>

### 1. Alpha Testing:
- Crit.1. The product should allow the customer to store their account info and diary entries. --> The product contains 2 database tables: 1 database to store user account information and 1 database to store each user's diary entries. --> PASSED <br>
- Crit.2. The product contains a login feature. --> When you open the app, you can enter your email and password in the allocated fields and you get signed in when you click the "sign in" button. --> PASSED <br>
- Crit.3. The product contains a register feature. --> You can enter an email and password in the allocated fields and your account gets added to the users database when you click the "register" button. --> PASSED <br>
- Crit.4.The product should have a pink theme. --> The app's theme has been set at the colour pink which is visible in the background and buttons. --> PASSED <br>
### 2. Beta Testing:
I asked my roomate, Aditi, who is the person who requested the app to use the app and test the same 4 criterias. She got the same results as in the alpha testing. I also received some feedback which I will copy paste here: "FEEDBACK"
### 3. Unit Testing:
1. Sign-in/ Register feature <br>
Test 1: putting the wrong password for an already registered username in the login field --> fails to login <br>
Test 2: register an account then put the same username and password in the login page --> success to login <br>
2. Diary feature: <br>
Test 1: entering a new diary entry then checking the diary table --> the entry appears in the diary table <br>
### 4. Personal Reflection:
The app functions pretty well. The database portion is functionning successfully. When you register, it writes your email and associated password in the users database table so that next time when you sign-in, it checks that your account information is available and links you to the diary screen. The diary screen allows you to enter a journal entry with the date associated. However, I struggled to make the entries appear in another screen. The only way to access previous entries is to look at the diary database table which is quite inconvenient and has to be improved. Overall, the success criteria has been met although the app might need some improvements. It has been generally to me a good introduction and practise of app development using KivyMD.

## Appendix
![alt text](screenshot.jpg) <br> 
**Fig. 6:** Customer requests conversation proof <br>
