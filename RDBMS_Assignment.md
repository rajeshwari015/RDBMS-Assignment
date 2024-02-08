# MS-AU 2020 RDBMS 1 Assignment 

## Topic : Data Modelling and ER Diagrams

### Task 1 : Create a conceptual and logical and physical data model using ER diagram for UBER (Ride sharing app).

### Conceptual Data Model for UBER

### Logical Data Model for UBER
! [Logical_model] 


### Physical Data Model for UBER
! [Physical_model] https://raw.githubusercontent.com/rajeshwari015/RDBMS-Assignment/main/Physical_schema.jpeg

## Topic : SQL Statements

### Task 2: Create tables from the physical data model from above task.(Create appropriate constraints and keys
### Task 3: Populate all the tables created with sample data

### Task 4: Write a stored routine to update the wallet balance of a user(2 incoming argument i.e. user and amount. (Print or return) updated wallet balance)

### Task 5:Write the correct SQL execution order for the following SELECT query.
               
``` sql
SELECT DISTINCT column, AGG_FUNC(column_or_expression), ...  
  FROM mytable 
       JOIN another_table ON mytable.column = another_table.column 
       WHERE constraint_expression 
       GROUP BY column 
       HAVING constraint_expression 
       ORDER BY column ASC/DESC 
       LIMIT count OFFSET COUNT;
```

 * Answer - The order of execution of the above sql query is as follows -  
  * from
  * join
  * where
  * group_by
  * Having
  * select
  * order_by
  * limit offset

## Topic : Functions and Procedures 

### Task 6: Write a simple function to calculate factorial of any number. (function accepts an IN parameter and returns the factorial of that).

* The SQL code presented below demonstrates the factorial calculation program making use of recursion, leveraging functions and procedures. 
* Additionally, we utilized sqlcmd prompt to connect to SQL Management Studio, allowing users to input a value and obtain the factorial of the specified number.

``` sql
-- Drop the function if it exists in the database
IF OBJECT_ID('dbo.CalculateFactorial') IS NOT NULL
    DROP FUNCTION dbo.CalculateFactorial;
GO

-- Drop the procedure if it exists in the database
IF OBJECT_ID('dbo.CalculateFactorialProcedure') IS NOT NULL
    DROP PROCEDURE dbo.CalculateFactorialProcedure;
GO

-- Function to calculate factorial using recursion
CREATE FUNCTION dbo.CalculateFactorial (@Number INT)
RETURNS BIGINT
AS
BEGIN
    -- If input value is negative
    IF @Number < 0
    BEGIN
        RETURN -1; 
		-- Indicating error for negative input
    END;

    -- This is the base case if the value is 0 or 1
    IF @Number = 0
    BEGIN
        RETURN 1;
    END;

    -- Recursive case to calculate the factorial
    RETURN @Number * dbo.CalculateFactorial(@Number - 1);
END;
GO

-- Procedure to calculate factorial and error handling
CREATE PROCEDURE dbo.CalculateFactorialProcedure 
    @InputValue INT
AS
BEGIN
    DECLARE @Factorial BIGINT;

    -- Calculate factorial using the function
    SET @Factorial = dbo.CalculateFactorial(@InputValue);

    -- Print the result
    IF @Factorial = -1
        PRINT 'Input value cannot be negative.';
    ELSE
        PRINT 'The factorial of ' + CAST(@InputValue AS NVARCHAR(50)) + ' is ' + CAST(@Factorial AS NVARCHAR(50));
END;
GO

-- Execute the procedure with the input value
EXEC dbo.CalculateFactorialProcedure @InputValue = $(InputValue);
``` 

### Comamnd to run the above code

### sqlcmd -S BLR-HYSN763\RAJESHWARI -d Factorial -v InputValue=n -i "C:\Users\pavuluri.r_INT1573\Documents\SQL Server Management Studio\Factorial.sql"

> **Note:** Replace the value of n with the input for whcih you want to find the factorial and also with your respective server name and destination of your sql file.

### Task 7: Execute the function with multiple values as a test case

## Screenshots of output in sqlcmd 

![SS]https://raw.githubusercontent.com/rajeshwari015/RDBMS-Assignment/main/Screen_shot.PNG

### The output format is as below.

* The factorial of 10 is 3628800
* The factorial of 5 is 120
* The factorial of 2 is 2

## MCQS

### 1.) What is a tuple equivalent to in SQL?

* Ans-  A row in the table

### 2.) How many NULL value that Unique key can have?

* Ans- 1

### 3.) Which Join is used to get only match tuples?

* Ans- Inner join

### 4.) Which is the king of Aggregate function?

* Ans- MIN

### 5.) Which are the TRANSACTION control commands?

* Ans- All the above

### 6.) When a program is abnormally terminated in a transaction,which of the following command occurs?

* Ans- Rollback

### 7.) What is wrong with the following query?
### Select V_ID,P_ID,P_DESC,P_RATE rate FROM TABLE1 GROUP BY V_ID

* Ans- No Aggregate function is used

