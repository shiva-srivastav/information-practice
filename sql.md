# SQL Functions Complete Guide for Class 12 Board Exam 📚

## Theory Overview of SQL Functions

### What are SQL Functions?
SQL functions are pre-written procedures that accept input parameters, perform specific tasks, and return results. They help in:
- Data manipulation
- Calculations
- Text processing
- Date handling
- Aggregate analysis

### Types of SQL Functions
1. Single-Row Functions (Scalar Functions)
   - Process one row at a time
   - Return one result per row
   - Examples: UPPER(), ROUND(), LENGTH()

2. Multiple-Row Functions (Aggregate Functions)
   - Process a group of rows
   - Return one result per group
   - Examples: COUNT(), SUM(), AVG()

## Setting Up Example Tables

Let's use a school database example with three tables:

```sql
-- Students table
CREATE TABLE Students (
    RollNo INT PRIMARY KEY,
    Name VARCHAR(50),
    Class VARCHAR(5),
    DateOfBirth DATE,
    Marks DECIMAL(5,2)
);

-- Subjects table
CREATE TABLE Subjects (
    SubjectCode VARCHAR(10) PRIMARY KEY,
    SubjectName VARCHAR(50)
);

-- StudentMarks table
CREATE TABLE StudentMarks (
    RollNo INT,
    SubjectCode VARCHAR(10),
    MarksObtained DECIMAL(5,2)
);

-- Sample data
INSERT INTO Students VALUES
(101, 'Ananya Sharma', '12A', '2007-05-15', 95.5),
(102, 'Rahul Kumar', '12B', '2006-12-20', 88.75),
(103, 'Priya Singh', '12A', '2007-03-10', 92.25);

INSERT INTO Subjects VALUES
('MATH01', 'Mathematics'),
('PHY01', 'Physics'),
('IP01', 'Informatics Practices');

INSERT INTO StudentMarks VALUES
(101, 'MATH01', 98),
(101, 'PHY01', 95),
(102, 'MATH01', 85),
(102, 'IP01', 92);
```

## 1. Math Functions 🔢

### Theory
Math functions perform mathematical operations on numeric data. They:
- Always return a numeric value
- Can be used in SELECT and WHERE clauses
- Can be nested within other functions
- Help in calculations and data analysis

### POWER(N,M) - Raises a number to power
- **Purpose**: Calculates N raised to power M
- **Input**: Two numbers (base and exponent)
- **Output**: Numeric value
- **Common Use**: Calculating squares, cubes

### ROUND(N,D) - Rounds a number
- **Purpose**: Rounds number to specified decimal places
- **Input**: Number and decimal places (D can be negative)
- **Output**: Rounded number
- **Note**: If D is negative, rounds left of decimal

### MOD(N,M) - Modulus operation
- **Purpose**: Returns remainder after division
- **Input**: Two numbers (dividend and divisor)
- **Output**: Remainder
- **Common Use**: Checking odd/even numbers

### TRUNCATE(N,D) - Truncates a number
- **Purpose**: Cuts off decimal places without rounding
- **Input**: Number and position to truncate
- **Output**: Truncated number
- **Note**: Different from ROUND as it doesn't round up

### Examples with All Math Functions:
```sql
-- Find square of marks
SELECT Name, Marks, POWER(Marks, 2) AS MarksSquared
FROM Students;
/*
Output:
Name            Marks   MarksSquared
Ananya Sharma   95.50   9120.25
Rahul Kumar     88.75   7876.56
Priya Singh     92.25   8510.06
*/
```

### ROUND() - Rounds numbers to specified decimals
```sql
-- Round marks to nearest integer
SELECT Name, Marks, ROUND(Marks) AS RoundedMarks
FROM Students;
/*
Output:
Name            Marks   RoundedMarks
Ananya Sharma   95.50   96
Rahul Kumar     88.75   89
Priya Singh     92.25   92
*/
```

### MOD() - Gets remainder after division
```sql
-- Check if RollNo is odd or even
SELECT RollNo, Name, MOD(RollNo, 2) AS IsOdd
FROM Students;
/*
Output:
RollNo  Name            IsOdd
101     Ananya Sharma   1    (Odd)
102     Rahul Kumar     0    (Even)
103     Priya Singh     1    (Odd)
*/
```

## 2. Text/String Functions 📝

### Theory
String functions manipulate text data. They:
- Work with character data (CHAR, VARCHAR)
- Are case-sensitive by default
- Return either string or numeric values
- Help in text formatting and analysis

### UPPER()/UCASE() - Uppercase conversion
- **Purpose**: Converts string to uppercase
- **Input**: Any string
- **Output**: Uppercase string
- **Note**: No effect on non-alphabetic characters

### LOWER()/LCASE() - Lowercase conversion
- **Purpose**: Converts string to lowercase
- **Input**: Any string
- **Output**: Lowercase string
- **Note**: No effect on non-alphabetic characters

### MID()/SUBSTRING() - Extract substring
- **Purpose**: Extracts part of a string
- **Syntax**: SUBSTRING(string, start, length)
- **Input**: String, starting position, length
- **Note**: Position starts from 1

### LENGTH() - String length
- **Purpose**: Returns length of string
- **Input**: Any string
- **Output**: Number (length)
- **Note**: Counts spaces and special characters

### LEFT() and RIGHT() - Extract from ends
- **Purpose**: Extract characters from start/end
- **Input**: String and number of characters
- **Output**: Extracted substring
- **Note**: Useful for fixed-length extractions

### TRIM Functions
- **LTRIM()**: Removes leading spaces
- **RTRIM()**: Removes trailing spaces
- **TRIM()**: Removes both leading and trailing spaces
- **Note**: Only removes spaces, not other characters

### CONCAT() - String concatenation
- **Purpose**: Joins two or more strings
- **Input**: Multiple strings
- **Output**: Combined string
- **Note**: NULL input results in NULL output

### INSTR() - Find substring position
- **Purpose**: Finds position of substring
- **Input**: Main string and search string
- **Output**: Position number (0 if not found)
- **Note**: Case-sensitive search

### Examples with All Text Functions:
```sql
-- Comprehensive text function examples
SELECT 
    Name,
    UPPER(Name) AS UpperCase,
    LOWER(Name) AS LowerCase,
    LENGTH(Name) AS NameLength,
    LEFT(Name, 3) AS First3,
    RIGHT(Name, 3) AS Last3,
    MID(Name, 2, 3) AS Middle3,
    CONCAT(Name, ' - Class ', Class) AS FullDetails,
    INSTR(Name, 'a') AS FirstA_Position,
    LTRIM('  ' + Name) AS TrimmedName
FROM Students;

/*
Output example for Ananya Sharma:
Name: Ananya Sharma
UpperCase: ANANYA SHARMA
LowerCase: ananya sharma
NameLength: 13
First3: Ana
Last3: rma
Middle3: nan
FullDetails: Ananya Sharma - Class 12A
FirstA_Position: 2
TrimmedName: Ananya Sharma
*/
```

## 3. Date Functions 📅

### Theory
Date functions work with date and time data. They:
- Handle various date formats
- Can extract specific parts of dates
- Help in date calculations
- Format dates for display

### NOW() - Current timestamp
- **Purpose**: Returns current date and time
- **Output**: DATETIME value
- **Note**: Includes both date and time
- **Common Use**: Logging, timestamps

### CURDATE() - Current date
- **Purpose**: Returns current date only
- **Output**: DATE value
- **Note**: No time component
- **Common Use**: Date-only operations

### Date Extraction Functions
1. **YEAR()**
   - Extracts year from date
   - Returns 4-digit year
   - Range: 1000 to 9999

2. **MONTH()**
   - Extracts month number
   - Returns 1-12
   - Used for month-based calculations

3. **DAY()**
   - Extracts day of month
   - Returns 1-31
   - Useful for day-specific queries

4. **MONTHNAME()**
   - Returns full month name
   - Example: 'January', 'February'
   - Good for reporting

5. **DAYNAME()**
   - Returns day of week name
   - Example: 'Monday', 'Tuesday'
   - Useful for scheduling

### DATE_FORMAT() - Format dates
- **Purpose**: Converts date to specific format
- **Input**: Date and format string
- **Output**: Formatted string
- **Common Use**: Report generation

### DATEDIFF() - Date difference
- **Purpose**: Calculates days between dates
- **Input**: Two dates
- **Output**: Number of days
- **Common Use**: Age calculations

### Examples with All Date Functions:
```

### LOWER()/LCASE() - Converts to lowercase
```sql
-- Convert subject names to lowercase
SELECT SubjectName, LOWER(SubjectName) AS LowerName
FROM Subjects;
/*
Output:
SubjectName          LowerName
Mathematics          mathematics
Physics              physics
Informatics Practices    informatics practices
*/
```

### MID()/SUBSTRING() - Extracts characters
```sql
-- Extract first 5 characters of names
SELECT Name, MID(Name, 1, 5) AS FirstFive
FROM Students;
/*
Output:
Name            FirstFive
Ananya Sharma   Anany
Rahul Kumar     Rahul
Priya Singh     Priya
*/
```

### LENGTH() - Gets string length
```sql
-- Find length of names
SELECT Name, LENGTH(Name) AS NameLength
FROM Students;
/*
Output:
Name            NameLength
Ananya Sharma   13
Rahul Kumar     11
Priya Singh     11
*/
```

### LEFT() and RIGHT() - Extract from start/end
```sql
-- Get first 3 and last 3 characters
SELECT Name, 
    LEFT(Name, 3) AS FirstThree,
    RIGHT(Name, 3) AS LastThree
FROM Students;
/*
Output:
Name            FirstThree  LastThree
Ananya Sharma   Ana         rma
Rahul Kumar     Rah         mar
Priya Singh     Pri         ngh
*/
```

## 3. Date Functions 📅

### NOW() - Gets current date and time
```sql
SELECT NOW() AS CurrentDateTime;
/*
Output:
CurrentDateTime
2024-02-24 10:30:45
*/
```

### Date Component Extraction
```sql
-- Extract various date components
SELECT DateOfBirth,
    YEAR(DateOfBirth) AS BirthYear,
    MONTH(DateOfBirth) AS BirthMonth,
    MONTHNAME(DateOfBirth) AS MonthName,
    DAYNAME(DateOfBirth) AS DayName
FROM Students;
/*
Output:
DateOfBirth  BirthYear   BirthMonth  MonthName   DayName
2007-05-15   2007        5           May         Tuesday
2006-12-20   2006        12          December    Wednesday
2007-03-10   2007        3           March       Saturday
*/
```

```sql
-- Comprehensive date function examples
SELECT 
    Name,
    DateOfBirth,
    YEAR(DateOfBirth) AS BirthYear,
    MONTH(DateOfBirth) AS BirthMonth,
    DAY(DateOfBirth) AS BirthDay,
    MONTHNAME(DateOfBirth) AS BirthMonthName,
    DAYNAME(DateOfBirth) AS BirthDayName,
    DATE_FORMAT(DateOfBirth, '%d/%m/%Y') AS FormattedDate,
    DATEDIFF(NOW(), DateOfBirth) AS DaysOld
FROM Students;

/*
Output example:
Name: Ananya Sharma
DateOfBirth: 2007-05-15
BirthYear: 2007
BirthMonth: 5
BirthDay: 15
BirthMonthName: May
BirthDayName: Tuesday
FormattedDate: 15/05/2007
DaysOld: 6127
*/
```

## 4. Aggregate Functions 📊

### Theory
Aggregate functions process multiple rows to produce a single result. They:
- Work on groups of data
- Ignore NULL values by default
- Are used with GROUP BY
- Cannot be used in WHERE clause

### COUNT() - Row counting
- **Purpose**: Counts rows or non-NULL values
- **Variations**: 
  - COUNT(*): All rows
  - COUNT(column): Non-NULL values
  - COUNT(DISTINCT column): Unique values
- **Common Use**: Finding totals

### SUM() - Total calculation
- **Purpose**: Adds numeric values
- **Input**: Numeric column
- **Output**: Total sum
- **Note**: Ignores NULL values

### AVG() - Average calculation
- **Purpose**: Calculates mean value
- **Input**: Numeric column
- **Output**: Average value
- **Note**: NULL values affect result

### MAX() and MIN() - Extreme values
- **Purpose**: Find highest/lowest values
- **Input**: Any comparable data type
- **Output**: Extreme value
- **Works with**: Numbers, dates, text

### GROUP BY Usage
- Groups rows with same values
- Used with aggregate functions
- Can group by multiple columns
- Often used with HAVING

### HAVING vs WHERE
- WHERE: Filters before grouping
- HAVING: Filters after grouping
- HAVING can use aggregate functions
- WHERE cannot use aggregate functions

### Examples with All Aggregate Functions:
```sql
-- Calculate various statistics
SELECT 
    COUNT(*) AS TotalStudents,
    AVG(Marks) AS AverageMarks,
    MAX(Marks) AS HighestMarks,
    MIN(Marks) AS LowestMarks,
    SUM(Marks) AS TotalMarks
FROM Students;
/*
Output:
TotalStudents  AverageMarks  HighestMarks  LowestMarks  TotalMarks
3              92.17         95.50         88.75        276.50
*/
```

### GROUP BY with HAVING
```sql
-- Average marks by class, show only if average > 90
SELECT Class, 
    COUNT(*) AS StudentCount,
    AVG(Marks) AS AverageMarks
FROM Students
GROUP BY Class
HAVING AVG(Marks) > 90;
/*
Output:
Class   StudentCount    AverageMarks
12A     2              93.875
*/
```

## 5. JOIN Operations 🔄

### INNER JOIN
```sql
-- Get student marks with names
SELECT 
    s.Name,
    sub.SubjectName,
    sm.MarksObtained
FROM Students s
INNER JOIN StudentMarks sm 
    ON s.RollNo = sm.RollNo
INNER JOIN Subjects sub 
    ON sm.SubjectCode = sub.SubjectCode;
/*
Output:
Name            SubjectName    MarksObtained
Ananya Sharma   Mathematics    98
Ananya Sharma   Physics        95
Rahul Kumar     Mathematics    85
Rahul Kumar     Inf. Practices 92
*/
```

### LEFT JOIN
```sql
-- Get all students and their marks (including those with no marks)
SELECT 
    s.Name,
    sub.SubjectName,
    COALESCE(sm.MarksObtained, 'Not Attempted') AS Marks
FROM Students s
LEFT JOIN StudentMarks sm 
    ON s.RollNo = sm.RollNo
LEFT JOIN Subjects sub 
    ON sm.SubjectCode = sub.SubjectCode;
/*
Output:
Name            SubjectName      Marks
Ananya Sharma   Mathematics      98
Ananya Sharma   Physics          95
Rahul Kumar     Mathematics      85
Rahul Kumar     Inf. Practices   92
Priya Singh     NULL            Not Attempted
*/
```

## Important Exam Tips 📝

1. **For Math Functions:**
   - Remember ROUND() takes two parameters: number and decimal places
   - POWER() is used for squares and cubes mostly
   - MOD() is useful for odd/even checks

2. **For Text Functions:**
   - UPPER/LOWER are most commonly asked
   - MID/SUBSTRING parameters: string, start position, length
   - Remember that string positions start from 1, not 0

3. **For Date Functions:**
   - NOW() gives both date and time
   - YEAR(), MONTH(), DAY() extract specific parts
   - MONTHNAME(), DAYNAME() give full names

4. **For Aggregate Functions:**
   - Always remember GROUP BY when using aggregates
   - HAVING comes after GROUP BY
   - WHERE comes before GROUP BY

5. **For JOINs:**
   - INNER JOIN shows only matching records
   - LEFT JOIN shows all records from left table
   - Always specify join conditions using ON

## Common Exam Questions 📚

1. Write queries to:
   - Find total students in each class
   - Calculate average marks by subject
   - List students with names in uppercase
   - Find youngest and oldest student
   - Get subject-wise highest marks

2. Identify errors in:
   - GROUP BY without aggregate functions
   - HAVING without GROUP BY
   - Incorrect JOIN conditions
   - Wrong function parameters

Remember:
- Practice writing queries without looking at notes
- Always check your JOIN conditions
- Test queries with small datasets first
- Pay attention to the requirement for NULL handling
