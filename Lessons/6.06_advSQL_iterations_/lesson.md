# Lesson 6.6: SQL Conditional Statements - Error Handling - Intro to VBA and Macros

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to introduce some other key concepts and techniques such as `if-else` conditional statements, talk more about case statements and how these can be used along with stored procedures to make the output more dynamic. We will also talk about some fundamental **error handling** in SQL using _handlers_. We will then introduce **macros** and how to use _VBA_ for Excel and go over how they can be useful in generating automated reports in Excel.

---

### Learning Objectives

After this lesson, students will be able to:

- Use `IF-ELSE` conditional statement
- Use a case statement
- Handle errors in SQL using _handlers_
- Explain what Macros are
- Interpret the logic of VBA (visual basic for applications)

---

### Lesson 1 key concepts

> :clock10: 20 min

- Conditional statements with stored procedures (`IF-ELSE` statement)

<details>
<summary> Click for Code: IF ELSE </summary>

- In this example, we will use the same query that we used before and try to build on that query. The objective is to make the result more interactive by displaying the result as "Red Zone", "Green Zone", or "Orange Zone", based on the company's brackets (brackets/groups of the average loss made by a region). You will see the calculation for loss in the query below.

- As the students will later notice, this is very similar to the `IF ELSE` statements used in Tableau.

```sql
drop procedure if exists average_loss_status_regiom_proc;

delimiter //
create procedure average_loss_status_regiom_proc (in param1 varchar(10), in param2 varchar(100), out param3 varchar(20))
begin
  declare avg_loss_region float default 0.0;
  declare zone varchar(20) default "";

  select round((sum(amount) - sum(payments))/count(*), 2) into avg_loss_region
  from (
    select a.account_id, d.A2 as district, d.A3 as region, l.amount, l.payments, l.status
    from bank.account a
    join bank.district d
    on a.district_id = d.A1
    join bank.loan l
    on l.account_id = a.account_id
    where l.status COLLATE utf8mb4_general_ci = param1
    and d.A3 COLLATE utf8mb4_general_ci = param2
  ) sub1;
select avg_loss_region;

  if avg_loss_region > 700000 then
    set zone = 'Red Zone';
  elseif avg_loss_region <= 70000 and credit > 40000 then
    set zone = 'Orange Zone';
  else
    set zone = 'Green Zone';
  end if;

  select zone into param3;
end;
//
delimiter;

call average_loss_status_regiom_proc("A", "Prague", @x);
select @x;
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_6.06_activities/blob/master/6.06_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solution](https://gist.github.com/ironhack-edu/dfd56b6c8a1029cf3d4db44954376176).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

- Conditional Statements with stored procedures

  - Case statements
  - Case statements vs. IF condition

<details>
<summary> Click for Code Sample </summary>

```sql
drop procedure if exists average_loss_status_regiom_proc;

delimiter //
create procedure average_loss_status_regiom_proc (in param1 varchar(10), in param2 varchar(100), out param3 varchar(20))
begin
  declare avg_loss_region float default 0.0;
  declare zone varchar(20) default "";
  select round((sum(amount) - sum(payments))/count(*), 2) into avg_loss_region
  from (
    select a.account_id, d.A2 as district, d.A3 as region, l.amount, l.payments, l.status
    from bank.account a
    join bank.district d
    on a.district_id = d.A1
    join bank.loan l
    on l.account_id = a.account_id
    where l.status COLLATE utf8mb4_general_ci = param1
    and d.A3 COLLATE utf8mb4_general_ci = param2
  ) sub1;

  select avg_loss_region;

  case
    when avg_loss_region > 50000 then
      set zone = 'PLATINUM';
    when avg_loss_region <= 50000 AND credit > 10000 then
      set zone = 'GOLD';
  else
    set zone = 'SILVER';
  end case;

  select zone into param3;
end;
//
delimiter;

call average_loss_status_regiom_proc("A", "Prague", @x);
select @x;
```

</details>

<details>
  <summary>Click for Description: Case statements vs IF else statements</summary>

- As you noticed, it seems they are very similar but there is a fundamental difference: _IF statements are usually used for logic flow while the CASE statement is usually used for return values_.

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>
  
- Link to [activity 2](https://github.com/ironhack-edu/data_6.06_activities/blob/master/6.06_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [activity 2 solution](https://gist.github.com/ironhack-edu/f2169ba3e7c3556171c3c508e90ae324).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Handling exceptions in stored procedures

  - Why we need exception handling
  - Using handlers

<details>
<summary> Click for Description: Why exception handling </summary>

- Managing exceptions is important to handle errors when they occur during the execution of stored procedures.
- Either you can continue to work in the procedure bypassing the error, or exit the code with an error message.
- You can define handlers in SQL to achieve this.
- We will see its application while trying to update some information in a table.

</details>

<details>
  <summary> Click for Code Sample: Handlers </summary>

```sql
drop procedure if exists update_account_table;

delimiter //
create procedure update_account_table (in param1 int, in param2 int, in param3 varchar(100), in param4 int, out param5 int)
begin
  declare continue handler for sqlexception
  select 'This account already exists in the database' message;
  insert into bank.account values(param1, param2, param3, param4);
  select 1 into param5;  -- we are using param 5 to check if the query is being executed or not. Later when we call the stored procedure, we will try to print the value of this variable
end;
//
delimiter;

call update_account_table(1,1,"131313", 31, @x);
select @x;
```

**Note to the instructor** Ask the students to check the `account` table in the database and see what values of `account_id` are present and which are not. Try to update the table with different values using the stored procedure and observe the results

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_6.06_activities/blob/master/6.06_activity_3.md).

(:exclamation: Note for instructor: Explain to the students how the `continue` and `exit` keywords work in each case)

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- Link to [activity 3 solution](https://gist.github.com/ironhack-edu/94ad92226361cae48b9ce81dc9f9d517).

</details>

---

:coffee: **BREAK**

---

### Lesson 4 key concepts

> :clock10: 20 min

- Stored procedures review (go over a couple of stored procedures discussed in class)
- Introduction to macros

  - What are macros
  - Intro to VBA
  - Setting up excel to work with macros
  - A walk-through developer tools interface, visual basic editor

<details>
<summary> Click for Description: Macros and VBA </summary>

- Macros are pieces of code written in Excel using VBA (visual basic for application) to automate tasks.
- VBA (visual basic for applications) is the programming language that is used to create user-defined functions/macros that enables users to automate processes in Excel.

</details>

<details>
  <summary> Click for Description: Setting up Excel to work with macros </summary>

- Ask the students to enable the "developer" tab in excel

  - For mac users - Go to Excel -> Preferences -> Ribbons and Toolbars -> Enable Developer option
  - [link to the image - Developer Tool Mac](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/6.6-enable_developer_tools_mac.png)
  - [link to the image - Developer Tool Options](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/6.6-developer_tool_options.png)

- A walk-through with the students, the interface of developer tools, different options such as record macros, visual basic, etc.: [link to the image - Visual Basic Editor Window](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/6.06-visual_basic_editor_window.png)
- Quickly talk about some of the features on the editor window.

</details>

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-sql-iterations](https://github.com/ironhack-labs/lab-sql-iterations)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/5b76d3f36c4812d17e193253a987b253).

</details>

---

:sandwich: **LUNCH BREAK**

---
