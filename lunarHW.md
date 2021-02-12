# Lunar New Year HW - DBs
![alt text](LunarERdiagram.jpg) <br>
**Fig. 1:**ER diagram of the database showing the table StudentInfo and its 6 attributes
```sql
CREATE TABLE IF NOT EXISTS  StudentInfo(
    id integer primary key autoincrement not null,
    name text,
    email varchar(30),
    birthdate date,
    address varchar(40),
    school text
  );

INSERT INTO StudentInfo(name, email, birthdate, address, school) VALUES('Carlos', 'carlosb@gmail.com', '2009-02-09', '23 something street','Lake School');
INSERT INTO StudentInfo(name, email, birthdate, address, school) VALUES('Marie', 'mariera@gmail.com', '1995-06-11', '9 this street','St. Louis School');
INSERT INTO StudentInfo(name, email, birthdate, address, school) VALUES('Bob', 'bobob@gmail.com', '2001-05-10', '56 blabla av.','Westhigh School')
```
