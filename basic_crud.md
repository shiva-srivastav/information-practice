# Database Fundamentals for Class 12 Board Exam

## 1. Relational Data Model 📚

### What is a Relational Database?
A relational database stores data in tables (called relations) where:
- Each table has rows (called tuples) and columns (called attributes)
- Each row represents a unique record
- Each column represents a specific piece of data

### Real-World Example: School Database
Let's consider a school database with these main tables:
1. Student table (for student details)
2. Class table (for class information)
3. Subject table (for subject details)
4. Marks table (for student marks)

## 2. Understanding Keys 🔑

### Primary Key
- Uniquely identifies each record in a table
- Cannot have duplicate values
- Cannot be NULL
- Usually one column, but can be multiple columns combined

**Example:**
- In Student table: Roll_No is the primary key
- In Subject table: Subject_Code is the primary key

### Candidate Key
- All possible columns that could be used as primary key
- Must have unique values

**Example in Student Table:**
- Roll_No (chosen as primary key)
- Admission_No (could be primary key)
- Aadhar_No (could be primary key)

### Foreign Key
- Links two tables together
- References primary key of another table
- Maintains referential integrity

**Example:**
- In Marks table: Roll_No is a foreign key referencing Student table

## 3. MySQL Data Types 📊

### Numeric Data Types
1. **INT** 
   - Whole numbers without decimals
   - Range: -2147483648 to 2147483647
   - Example use: Roll numbers, Age
   ```sql
   Roll_No INT,
   Age INT
   ```

2. **DECIMAL(size, d)**
   - Exact decimal numbers
   - 'size' is total number of digits
   - 'd' is number of digits after decimal
   - Example use: Marks, Percentage
   ```sql
   Percentage DECIMAL(5,2)  -- Allows 999.99
   Marks DECIMAL(4,1)      -- Allows 100.0
   ```

3. **FLOAT/DOUBLE**
   - Approximate decimal numbers
   - Used when decimal precision isn't critical
   - Example use: Scientific calculations
   ```sql
   Height FLOAT,
   Weight DOUBLE
   ```

### String Data Types
1. **VARCHAR(size)**
   - Variable-length string
   - Maximum size: 65535 characters
   - Example use: Names, Addresses
   ```sql
   Name VARCHAR(50),
   Address VARCHAR(100)
   ```

2. **CHAR(size)**
   - Fixed-length string
   - Always uses specified size
   - Example use: Gender, Section
   ```sql
   Section CHAR(1),      -- Always 1 character
   Gender CHAR(1)        -- 'M' or 'F'
   ```

3. **TEXT**
   - Long text strings
   - Maximum size: 65535 characters
   - Example use: Comments, Descriptions
   ```sql
   Description TEXT
   ```

### Date and Time Data Types
1. **DATE**
   - Format: 'YYYY-MM-DD'
   - Example use: Birth dates, Admission dates
   ```sql
   DOB DATE,
   Admission_Date DATE
   ```

2. **TIME**
   - Format: 'HH:MM:SS'
   - Example use: Class timings
   ```sql
   Class_Time TIME
   ```

3. **DATETIME**
   - Combines date and time
   - Format: 'YYYY-MM-DD HH:MM:SS'
   ```sql
   Assignment_Deadline DATETIME
   ```

## 4. Basic SQL Commands with Examples and Explanations 💻

### CREATE TABLE
Creates a new table with specified columns and constraints.

```sql
-- Creating Student Table
CREATE TABLE Student (
    Roll_No INT PRIMARY KEY,
    Name VARCHAR(50) NOT NULL,
    Class VARCHAR(10),
    Section CHAR(1),
    Contact_No VARCHAR(15)
);

-- Creating Marks Table
CREATE TABLE Marks (
    Roll_No INT,
    Subject_Code VARCHAR(10),
    Marks INT,
    FOREIGN KEY (Roll_No) REFERENCES Student(Roll_No)
);
```

### INSERT Command
Adds new records to a table.

```sql
-- Adding a single student
INSERT INTO Student 
VALUES (101, 'Ananya Sharma', '12', 'A', '9876543210');

-- Adding multiple students
INSERT INTO Student VALUES
(102, 'Rahul Kumar', '12', 'A', '9876543211'),
(103, 'Priya Singh', '12', 'B', '9876543212'),
(104, 'Amit Verma', '12', 'A', '9876543213');

-- Adding marks for Ananya
INSERT INTO Marks VALUES
(101, 'IP001', 95),    -- Informatics Practices
(101, 'MATH001', 92),  -- Mathematics
(101, 'PHY001', 88);   -- Physics
```

### SELECT Command with Clauses
The SELECT command is used to retrieve data from tables. Let's understand each component:

#### Basic SELECT Structure:
```sql
SELECT column_names
FROM table_name
[WHERE condition]
[ORDER BY column_name [ASC|DESC]]
[GROUP BY column_name]
[HAVING condition];
```

#### 1. Simple SELECT
```sql
-- Basic SELECT
-- Get all students (retrieves every column of every row)
SELECT * FROM Student;
/* 
- '*' means all columns
- Simple way to view entire table
- Not recommended in production code
*/

-- Get specific columns (better practice - only select what you need)
SELECT Name, Class, Section 
FROM Student;
/*
- More efficient than SELECT *
- Returns only specified columns
- Reduces data transfer
*/

#### 2. WHERE Clause
The WHERE clause filters records based on specific conditions.

```sql
-- Find Ananya's details using exact match
SELECT * FROM Student 
WHERE Name = 'Ananya Sharma';
/*
- Case sensitive matching
- Returns only rows where condition is TRUE
- Can use =, <, >, <=, >=, !=, LIKE, IN, BETWEEN
*/

-- Find all students in Section A
SELECT Name, Roll_No 
FROM Student 
WHERE Section = 'A';
/*
- Simple equality condition
- Can return multiple rows
*/

-- Complex WHERE conditions
SELECT Name, Roll_No
FROM Student
WHERE Section = 'A' 
    AND Class = '12'
    AND Roll_No > 100;
/*
- Multiple conditions using AND
- All conditions must be TRUE
- Can use OR for alternative conditions
*/

#### 3. ORDER BY Clause
ORDER BY sorts the results in ascending (ASC) or descending (DESC) order.

```sql
-- Get marks in descending order
SELECT Student.Name, Marks.Subject_Code, Marks.Marks
FROM Student, Marks
WHERE Student.Roll_No = Marks.Roll_No
ORDER BY Marks.Marks DESC;
/*
- DESC for highest to lowest
- ASC is default (can be omitted)
- Can sort by multiple columns
*/

-- Multiple column sorting
SELECT Name, Class, Section
FROM Student
ORDER BY Class ASC, Section DESC;
/*
- First sorts by Class ascending
- Then within each Class, sorts by Section descending
*/
```

### UPDATE Command with Explanations
The UPDATE command modifies existing records in a table. Always use WHERE clause unless you want to update all records.

```sql
-- Update single column for one student
UPDATE Student 
SET Contact_No = '9876543299'
WHERE Name = 'Ananya Sharma';
/*
- SET specifies new value
- WHERE identifies specific record
- Always test WHERE condition with SELECT first
*/

-- Update multiple columns
UPDATE Student 
SET Contact_No = '9876543299',
    Section = 'B'
WHERE Name = 'Ananya Sharma';
/*
- Can update multiple columns in one statement
- Comma-separated SET values
- All updates happen simultaneously
*/

-- Update with calculations
UPDATE Marks 
SET Marks = Marks + 2
WHERE Subject_Code = 'IP001';
/*
- Can use current value in calculation
- Affects all rows matching WHERE condition
- Common for grace marks or corrections
*/
```

### DELETE Command
Removes records from a table.

```sql
-- Delete a specific student's record
DELETE FROM Student 
WHERE Roll_No = 104;

-- Delete all marks below 50 (failing marks)
DELETE FROM Marks 
WHERE Marks < 50;
```

## 4. Practice Scenarios 📝

### Scenario 1: Adding New Student
You need to add a new student named "Aarav Singh" who:
- Has Roll No 105
- Is in Class 12-B
- Has contact number 9876543214

```sql
INSERT INTO Student 
VALUES (105, 'Aarav Singh', '12', 'B', '9876543214');
```

### Scenario 2: Finding Class Toppers
Find students who scored above 90 in any subject:

```sql
SELECT Student.Name, Marks.Subject_Code, Marks.Marks
FROM Student, Marks
WHERE Student.Roll_No = Marks.Roll_No
    AND Marks.Marks > 90
ORDER BY Marks.Marks DESC;
```

## 5. Important Points to Remember ⭐

1. **Primary Key Rules:**
   - Must be unique
   - Cannot be NULL
   - Should not change over time

2. **Foreign Key Rules:**
   - Can be NULL
   - Must match a primary key value in referenced table
   - Can appear multiple times

3. **Common Mistakes to Avoid:**
   - Forgetting to use WHERE in UPDATE/DELETE
   - Not maintaining referential integrity
   - Using incorrect data types

4. **Best Practices:**
   - Always use meaningful table and column names
   - Comment your SQL code
   - Use proper indentation
   - Test queries with small data sets first

## 6. Exam Tips 📌

1. **For Theory Questions:**
   - Explain concepts with simple examples
   - Draw tables to show relationships
   - Use proper technical terms

2. **For Practical Questions:**
   - Read the question carefully
   - Plan your table structure
   - Write queries step by step
   - Test with sample data

3. **Common Exam Questions:**
   - Create table with constraints
   - Insert multiple records
   - Update specific records
   - Delete based on conditions
   - Select with multiple conditions
   - Join multiple tables
