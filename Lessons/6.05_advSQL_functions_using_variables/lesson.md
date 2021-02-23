# Lesson 6.5: Stored Procedures in depth

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to learn how to create and use stored procedures and functions. We will learn how they can be useful in generating results that might be frequently required by different stakeholders in the company.

---

:exclamation: Note for instructor: If the students do not have the `bank dataset` available with MySQL Workbench or Sequel Pro, please ask the students to use `mysql_dump.sql` to get the data from `files_for_lesson_and_activities` folder.

### Learning Objectives

After this lesson, students will be able to:

- Create Stored procedures/subroutines in SQL to automate functions
- Generate reports using stored procedures and functions
- Use variables in stored procedures
- Use parameters in stored procedures

---

### Lesson 1 key concepts

> :clock10: 20 min

- More examples on stored procedures

  - Stored procedures that return a single value
  - Stored procedures that return rows of results from a query

<details>
  <summary> Click for Code Sample </summary>

- We will get more familiar with the syntax of stored procedures. We will create a stored procedure to execute a query (in a business scenario it is usually a data that the stakeholders are frequently interested in).

- The customers with status "B" are the ones where the contract was finished but the loan amount was not paid. Your manager wants to keep a track of the average money not paid by such customers. We will create a stored procedure to run this query.

```sql
delimiter //
create procedure average_loss_proc (out param1 float)
begin
  select round((sum(amount) - sum(payments))/count(*), 2) into param1
  from bank.loan
  where status = "B";
end;
//
delimiter ;

call average_loss_proc(@x);
select round(@x,2) as Average_loss_per_customer;
```

</details>

<details>
  <summary> Click for Code: Stored Procedure to return rows from query results </summary>

```sql
delimiter //
create procedure return_query_rows_proc()
begin
  select *
  from bank.loan
  where status = "B";
end;
//
delimiter;

call return_query_rows_proc();
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_6.05_activities/blob/master/6.05_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solution](https://gist.github.com/ironhack-edu/f63f6be61485ce43a114f6781021a19a).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

- Working with variables in stored procedures

  - Defining variables
  - Setting values for variables

- Check status of all the procedures

<details>
<summary> Click for Description: Variables in SQL </summary>

- As in any other programming language, variables are used to store data during the execution period of SQL statements.
- Variables can be created for different data types and can also be assigned values. The assigned values can be changed during the execution of the queries.
- If a variable is being used in a stored procedure, the scope of the variable is limited to the stored procedure. We will talk about variables in stored procedures.
- They help us in writing more dynamic queries.
- MySQL data types: [link to the image - MySQL data types](https://education-team-2020.s3-eu-west-1.amazonaws.com/data-analytics/6.5-sql_data_types.png)

</details>

<details>
<summary> Click for Code Sample: Defining Variables and Setting values </summary>

:exclamation: Note for instructor: This is the query that we used earlier to return the average loss per customer with Status "B".

```sql
delimiter //
create procedure average_loss_proc (out param1 float)
begin
  select round((sum(amount) - sum(payments))/count(*), 2) into param1
  from bank.loan
  where status = "B";
end;
//
delimiter ;

call average_loss_proc(@x);
select round(@x,2) as Average_loss_per_customer;
```

- We will try to simplify this query by using variables.

:exclamation: Note for instructor: Emphasize on the keywords - `declare`, `default` and `use`. We will also use `drop procedure` here. We have not talked about it but this syntax is the same as we have used for dropping tables.

```sql
drop procedure if exists average_loss_proc;

delimiter //
create procedure average_loss_proc ()
begin
  declare avg_loss float default 0.0;
  select round((sum(amount) - sum(payments))/count(*), 2) into avg_loss
  from bank.loan
  where status = "B";
  select avg_loss;
  set avg_loss = 0.0;
end;
//
delimiter ;

call average_loss_proc();
```

</details>

<details>
<summary> Click for Code: Check status of procedures </summary>

```sql
show procedure status;
show function status;
```

:exclamation: Note for instructor: You can show to students how to use this set of results as a table and query from it.

```sql
show procedure status where Db = 'bank';
```

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_6.05_activities/blob/master/6.05_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [activity 2 solution](https://gist.github.com/ironhack-edu/d1e309a200432fb635e0443ce38a596e).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Using parameters with stored procedures

  - In parameter (procedure with a single argument)
  - In parameter (procedure with multiple arguments)

<details>
<summary> Click for Code Sample: Using 'IN' Parameters</summary>

- For this lesson we will use the same query as we used before to return the average loss per customer with Status "B". However, this time we want to make the query more dynamic by giving the user the option to choose from any of the 4 statuses. We will ask the user to input the choice of customer status ("A", "B", "C" or "D") that they are interested in.

This is the previous stored procedure:

```sql
drop procedure if exists average_loss_proc;

delimiter //

create procedure average_loss_proc()
begin
  declare avg_loss float default 0.0;
  select round((sum(amount) - sum(payments))/count(*), 2) into avg_loss
  from bank.loan
  where status = "B";
  select avg_loss;
  set avg_loss = 0.0;
end;
//
delimiter ;

call average_loss_proc();
```

</details>

<details>
  <summary> Click for Code Sample: Using 'IN' Parameters - UPDATED stored procedure with the option of user input </summary>

- Now, instead of retrieving the information of customers of only one status, we have the flexibility of choosing any status and look at the average loss/recovery to be done yet.

```sql
drop procedure if exists average_loss_proc;

delimiter //
create procedure average_loss_proc (in param varchar(10))
begin
  declare avg_loss float default 0.0;
  select round((sum(amount) - sum(payments))/count(*), 2) into avg_loss
  from bank.loan
  where status COLLATE utf8mb4_general_ci = param;
  select avg_loss;
end;
//
delimiter ;

call average_loss_proc("A");
```

</details>

<details>
  <summary>Click for Code: Using multiple parameters</summary>

- In this example, we will go from here: the stakeholders are interested in conducting a similar analysis but for a particular region as well. We will use two arguments here, one is status and the other is region, and return the average amount to be recovered from the customers.

- First we will write a simple query (without stored procedure) to see if we are getting the correct results:

```sql
select round((sum(amount) - sum(payments))/count(*), 2)
from (
  select a.account_id, d.A2 as district, d.A3 as region, l.amount, l.payments, l.status
  from bank.account a
  join bank.district d
  on a.district_id = d.A1
  join bank.loan l
  on l.account_id = a.account_id
  where l.status = "B" and d.A3 = 'Prague'
) sub1;
```

- We will take a look at how to use this inside a stored procedure in the next lesson.

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_6.05_activities/blob/master/6.05_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- Link to [activity 3 solution](https://gist.github.com/ironhack-edu/afeb6fb66668422c71046bcddf66e082).

</details>

---

:coffee: **BREAK**

---

### Lesson 4 key concepts

> :clock10: 20 min

- Using multiple parameters with stored procedures
- `INOUT` parameters with stored procedures (only explanation)
- Review of stored procedures (since this is a very new topic, it would be a good idea to check back on the topic again)

<details>
  <summary> Click for Code Sample: Multiple arguments with stored procedures </summary>

```sql
drop procedure if exists average_loss_status_regiom_proc;

delimiter //
create procedure average_loss_status_regiom_proc (in param1 varchar(10), in param2 varchar(100))
begin
  declare avg_loss_region float default 0.0;
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
end;
//
delimiter ;

call average_loss_status_regiom_proc("A", "Prague");
```

</details>

<details>
  <summary> Click for Description: `INOUT` parameters </summary>

- So far we looked at `IN` parameters and `OUT` parameters in stored procedures.

:exclamation: Note for instructor: You can talk about `IN` vs. `OUT` briefly.

- There's also `INOUT` parameter which is basically a combination of `IN` and `OUT` parameter. While calling the procedure, you can pass the argument. This argument can be modified inside the stored procedure and then returned to the call statement.

</details>

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-stored-procedures](https://github.com/ironhack-labs/lab-stored-procedures)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/d57b4a6c2438bb2cd1f6a219b51c7631).

</details>

---

:sandwich: **LUNCH BREAK**

---
