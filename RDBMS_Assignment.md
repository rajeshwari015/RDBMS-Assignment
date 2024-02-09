# MS-AU 2020 RDBMS 1 Assignment 
 
## Topic : Data Modelling and ER Diagrams

### Task 1 : Create a conceptual and logical and physical data model using ER diagram for UBER (Ride sharing app).

## Conceptual Data Model for UBER 
![Conceptual_model](https://raw.githubusercontent.com/rajeshwari015/RDBMS-Assignment/main/Conceptual_schema.jpeg)

## Logical Data Model for UBER
![Logical_model](https://raw.githubusercontent.com/rajeshwari015/RDBMS-Assignment/main/Logical_schema.jpeg)


## Physical Data Model for UBER
![Physical_model](https://raw.githubusercontent.com/rajeshwari015/RDBMS-Assignment/main/Physical_schema.jpeg)

## Topic : SQL Statements

### Task 2: Create tables from the physical data model from above task.(Create appropriate constraints and keys

```sql
create database my_uber
use my_uber


CREATE TABLE Passenger (
    PID INT PRIMARY KEY identity(1,1),
    PName VARCHAR(20) not null,
    Email VARCHAR(50) unique,
    DOB DATE,
    Address VARCHAR(100),
    Wallet decimal(10,2) ,
    CreatedAt DATETIME default getdate(),
);

CREATE TABLE VehType (
    TypeID INT PRIMARY KEY Identity,
    VType VARCHAR(20),
    NoOfPassengers INT 
);

CREATE TABLE Vehicle (
    ID INT PRIMARY KEY identity(1,1),
    TypeID INT,
    RegistrationNumber VARCHAR(25) UNIQUE,
    CONSTRAINT FK_Type FOREIGN KEY (TypeID) REFERENCES VehType(TypeID)
);

CREATE TABLE Driver (
    DriverID INT PRIMARY KEY identity(1,1),
    VehicleID INT UNIQUE references Vehicle(ID),
    DriverName VARCHAR(20) NOT NULL,
    Email VARCHAR(50) UNIQUE,
    DOB DATE,
    Address VARCHAR(100),
    Licence VARCHAR(15) UNIQUE,
    CurrentLocation GEOGRAPHY NOT NULL,
    CreatedAt DATETIME DEFAULT GETDATE()
);

CREATE TABLE Rating (
    ID INT PRIMARY KEY,
    DriverID INT,
    Rating INT CHECK (Rating >= 1 AND Rating <= 5),
    Feedback VARCHAR(MAX),
    CONSTRAINT FK_Driver FOREIGN KEY (DriverID) REFERENCES Driver(DriverID)
);

CREATE TABLE Mode(
ID BINARY PRIMARY KEY ,
Mtype Varchar(6)
);

CREATE TABLE Payment (
    ID INT PRIMARY KEY identity(1,1),
    Mode BINARY,--(Mode IN ('Cash', 'Wallet'))
    Amount DECIMAL(10, 2),
    PaidAt DATETIME default GETDATE(),
	CONSTRAINT Fk_Mode FOREIGN KEY (Mode) REFERENCES Mode(ID)
    
);


create table status(
stid int primary key,
stype varchar(20),
);

create table Reason(
ID int identity primary key,
reason varchar(50)
);

create table  Trips(
TID INT PRIMARY KEY identity(1,1), 
STID int references status(stid),
DriverID INT references Driver(DriverID),
PID INT references Passenger(PID),
PaymentId INT references Payment(ID),
Rating_ID int references Rating(ID),
Reason_ID int references Reason(ID),
waiting_time time,
destination GEOGRAPHY,
pickup_loc GEOGRAPHY,
CreatedAt DATETIME DEFAULT GETDATE()
);



create table Request(
id int primary key identity(1,1),
stid int references status(stid),
pid int references passenger(pid),
pickuploc GEOGRAPHY,
destination GEOGRAPHY,
wating_time time
);
```
### Task 3: Populate all the tables created with sample data

```sql
INSERT INTO Passenger (PName, Email, DOB, Address, Wallet)
VALUES 
    ('Aarav Kumar', 'aarav@example.com', '1990-05-15', '123 Main St, Mumbai', 1000),
    ('Vidya Sharma', 'vidya@example.com', '1985-08-22', '456 Elm St, Delhi', 800),
    ('Rohan Patel', 'rohan@example.com', '1992-11-10', '789 Oak St, Bangalore', 1200),
    ('Priya Gupta', 'priya@example.com', '1988-03-25', '101 Pine St, Kolkata', 600),
    ('Arjun Singh', 'arjun@example.com', '1995-07-19', '555 Cedar St, Chennai', 1500),
    ('Neha Verma', 'neha@example.com', '1993-09-30', '777 Maple St, Hyderabad', 900),
    ('Rajesh Shah', 'rajesh@example.com', '1987-12-08', '888 Birch St, Pune', 1100),
    ('Ananya Reddy', 'ananya@example.com', '1991-04-12', '222 Spruce St, Ahmedabad', 1300),
    ('Kabir Kumar', 'kabir@example.com', '1998-06-28', '333 Willow St, Jaipur', 700),
    ('Shreya Singh', 'shreya@example.com', '1989-02-14', '444 Walnut St, Lucknow', 1000);

	
	INSERT INTO Driver (VehicleID, DriverName, Email, DOB, Address, Licence, CurrentLocation)
VALUES 
    (1, 'Amit Mishra', 'amit@example.com', '1980-07-12', '123 Main St, Mumbai', 'DL123456', geography::Point(19.0760, 72.8777, 4326)),
    (2, 'Deepak Singh', 'deepak@example.com', '1983-09-18', '456 Elm St, Delhi', 'DL654321', geography::Point(28.7041, 77.1025, 4326)),
    (3, 'Pooja Sharma', 'pooja@example.com', '1982-05-03', '789 Oak St, Bangalore', 'DL987654', geography::Point(12.9716, 77.5946, 4326)),
    (4, 'Rajiv Gupta', 'rajiv@example.com', '1979-11-26', '101 Pine St, Kolkata', 'DL456789', geography::Point(22.5726, 88.3639, 4326)),
    (5, 'Nisha Patel', 'nisha@example.com', '1984-03-15', '555 Cedar St, Chennai', 'DL234567', geography::Point(13.0827, 80.2707, 4326)),
    (6, 'Rahul Verma', 'rahul@example.com', '1981-08-09', '777 Maple St, Hyderabad', 'DL345678', geography::Point(17.3850, 78.4867, 4326)),
    (7, 'Kiran Shah', 'kiran@example.com', '1978-12-24', '888 Birch St, Pune', 'DL567890', geography::Point(18.5204, 73.8567, 4326)),
    (8, 'Anjali Reddy', 'anjali@example.com', '1986-04-30', '222 Spruce St, Ahmedabad', 'DL789012', geography::Point(23.0225, 72.5714, 4326)),
    (9, 'Vikram Kumar', 'vikram@example.com', '1985-06-17', '333 Willow St, Jaipur', 'DL890123', geography::Point(26.9124, 75.7873, 4326)),
    (10, 'Sneha Singh', 'sneha@example.com', '1980-02-05', '444 Walnut St, Lucknow', 'DL901234', geography::Point(26.8467, 80.9462, 4326));


	INSERT INTO VehType (VType, NoOfPassengers)
VALUES 
    ('Cab', 4),
    ('Auto', 3),
    ('Bike', 2);
	select * from VehType
	INSERT INTO Vehicle (TypeID, RegistrationNumber)
VALUES 
    (2, 'MH01AB1234'),
    (2, 'DL02CD4567'),
    (3, 'KA03EF7890'),
    (1, 'WB04GH1234'),
    (3, 'TN05IJ5678'),
    (1, 'MH06KL9012'),
    (2, 'DL07MN3456'),
    (3, 'KA08OP6789'),
    (1, 'WB09QR1234'),
    (2, 'TN10ST5678');

INSERT INTO status (stid, stype)
VALUES 
    (1, 'Pending'),
	(2, 'Accepted'),
	(3, 'Failed'),
    (4, 'Ongoing'),
    (5, 'Completed'),
    (6, 'Canceled');
    

INSERT INTO Request (STID, PID, pickuploc, destination, wating_time)
VALUES 
    (5, 1, geography::Point(19.0760, 72.8777, 4326), geography::Point(19.0822, 72.7412, 4326), '00:05:00'),
    (3, 2, geography::Point(28.7041, 77.1025, 4326), geography::Point(28.5355, 77.3910, 4326), '00:10:00'),
    (5, 3, geography::Point(12.9716, 77.5946, 4326), geography::Point(12.9719, 77.5937, 4326), '00:08:00'),
    (3, 4, geography::Point(22.5726, 88.3639, 4326), geography::Point(22.5431, 88.3406, 4326), '00:15:00'),
    (5, 5, geography::Point(13.0827, 80.2707, 4326), geography::Point(13.0837, 80.2700, 4326), '00:20:00'),
    (5, 6, geography::Point(17.3850, 78.4867, 4326), geography::Point(17.3883, 78.4611, 4326), '00:05:00'),
    (2, 7, geography::Point(18.5204, 73.8567, 4326), geography::Point(18.5204, 73.8567, 4326), '00:10:00'),
    (4, 8, geography::Point(23.0225, 72.5714, 4326), geography::Point(23.0225, 72.5714, 4326), '00:08:00'),
    (5, 9, geography::Point(26.9124, 75.7873, 4326), geography::Point(26.9124, 75.7873, 4326), '00:15:00'),
    (5, 10, geography::Point(26.8467, 80.9462, 4326), geography::Point(26.8467, 80.9462, 4326), '00:20:00');


INSERT INTO Trips (STID, DriverID, PID, PaymentId, waiting_time, destination, pickup_loc, CreatedAt)
VALUES 
    (5, 1, 1, 1, '00:05:00', '19.0822, 72.7412', '19.0760, 72.8777', GETDATE()),
    (3, 2, 2, 2, '00:10:00', '28.5355, 77.3910', '28.7041, 77.1025', GETDATE()),
    (5, 3, 3, 3, '00:08:00', '12.9719, 77.5937', '12.9716, 77.5946', GETDATE()),
    (3, 4, 4, 4, '00:15:00', '22.5431, 88.3406', '22.5726, 88.3639', GETDATE()),
    (5, 5, 5, 5, '00:20:00', '13.0837, 80.2700', '13.0827, 80.2707', GETDATE()),
    (5, 6, 6, 6, '00:05:00', '17.3883, 78.4611', '17.3850, 78.4867', GETDATE()),
    (2, 7, 7, 7, '00:10:00', '18.5204, 73.8567', '18.5204, 73.8567', GETDATE()),
    (4, 8, 8, 8, '00:08:00', '23.0225, 72.5714', '23.0225, 72.5714', GETDATE()),
    (5, 9, 9, 9, '00:15:00', '26.9124, 75.7873', '26.9124, 75.7873', GETDATE()),
(5, 10, 10, 10, '00:20:00', '26.8467, 80.9462', '26.8467, 80.9462', GETDATE());

INSERT INTO Payment (Mode, Amount, PaidAt)
VALUES 
    (1, 50.00, GETDATE()),
    (2, 70.00, GETDATE()),
    (1, 35.00, GETDATE()),
    (2, 60.00, GETDATE()),
    (1, 45.00, GETDATE()),
    (1, 55.00, GETDATE()),
    (2, 75.00, GETDATE()),
    (1, 40.00, GETDATE()),
    (2, 65.00, GETDATE()),
    (1, 30.00, GETDATE());

	INSERT INTO Mode (ID, Mtype)
VALUES 
    (1, 'Cash'),
    (2, 'Wallet');

INSERT INTO Reason (reason)
VALUES 
    ('Driver unavailable'),
    ('Unexpected traffic'),
    ('Passenger canceled'),
    ('Technical issues'),
    ('Bad weather'),
    ('Change in plans'),
    ('Vehicle breakdown'),
    ('Accident'),
    ('Payment issue'),
    ('Other');
```

### Task 4: Write a stored routine to update the wallet balance of a user(2 incoming argument i.e. user and amount. (Print or return) updated wallet balance)

```sql
CREATE PROCEDURE UpdateWalletBal
    @UserID INT,
    @Amount DECIMAL(10, 2)
AS
BEGIN
    SET NOCOUNT ON;
    DECLARE @UpdatedBalance DECIMAL(10, 2);
    UPDATE Passenger
    SET Wallet = Wallet - @Amount
    WHERE PID = @UserID;
    SELECT @UpdatedBalance = Wallet
    FROM Passenger
    WHERE PID = @UserID;
    PRINT 'Updated wallet balance for user ' + CAST(@UserID AS VARCHAR(10)) + ' is ' + CAST(@UpdatedBalance AS VARCHAR(20));
END;

EXEC UpdateWalletBal @UserID = 1, @Amount = 250;
select * from passenger;
```

### Screenshot of Updated wallet_balnce 
![Final_output](https://raw.githubusercontent.com/rajeshwari015/RDBMS-Assignment/main/Final_output.jpeg)

### Screenshot of the initial passenger table
![Initial](https://raw.githubusercontent.com/rajeshwari015/RDBMS-Assignment/main/Initial.jpeg)

### Screenshot of updated passenger table
![updated](https://raw.githubusercontent.com/rajeshwari015/RDBMS-Assignment/main/After_update.jpeg)


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

![SS](https://raw.githubusercontent.com/rajeshwari015/RDBMS-Assignment/main/Screen_shot.PNG)

### The output format is as below.

* The factorial of 10 is 3628800
* The factorial of 5 is 120
* The factorial of 2 is 2

## MCQS

### 1.) What is a tuple equivalent to in SQL?

**Ans-**  A row in the table

### 2.) How many NULL value that Unique key can have?

**Ans-** 1

### 3.) Which Join is used to get only match tuples?

**Ans-** Inner join

### 4.) Which is the kind of Aggregate function?

**Ans-** MIN

### 5.) Which are the TRANSACTION control commands?
* a.) Commit
* b.)Savepoint
* c.) Rollback
* d.) All the above 

**Ans-** All the above

### 6.) When a program is abnormally terminated in a transaction,which of the following command occurs?

**Ans-** Rollback

### 7.) What is wrong with the following query?
### Select V_ID,P_ID,P_DESC,P_RATE rate FROM TABLE1 GROUP BY V_ID
 * a.) No Aggregate function is used 
 * b.)No where cluase is specified
 * c.)Alias is only for one column
 * d.) Nothing is wrong 

  **Ans-** No Aggregate function is used

