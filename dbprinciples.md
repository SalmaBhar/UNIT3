```py
-- Creating the db
CREATE TABLE IF NOT EXISTS students(
    id integer primary key autoincrement not null ,
    name text,
    fees_paid text,
    date_of_birth date,
    address varchar(150),
    course text
  )

-- inserting:
INSERT INTO students(name, address) values('Bob', 'Massachusetts ave, US')
-- deleting:
DELETE FROM students where id=2

-- selecting certain values from the table
select * from students; -- '*' means everything
select name from students info;
select name, address from students where id=3;

-- changing something from a table
update students set date_of_birth='28/07/2018' where id=3;
```
