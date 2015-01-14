MSSQL-Snippets
==============
CheatSheet

UDF Determinism
===============

```
SELECT OBJECTPROPERTY(OBJECT_ID('dbo.myUdf'), 'IsDeterministic');
```

DECLARE and SET Varibales
==============
DECLARE @Mojo int
SET @Mojo = 1
SELECT @Mojo = Column FROM Table WHERE id=1

IF / ELSE IF / ELSE Statement
==============
IF @Mojo < 1
  BEGIN
	PRINT 'Mojo Is less than 1'
  END
ELSE IF @Mojo = 1
  BEGIN
    PRINT 'Mojo Is 1'
  END
ELSE
  BEGIN
	PRINT 'Mojo greater than 1'
  END
  
CASE Statement
==============
SELECT Day = CASE 
  WHEN (DateAdded IS NULL) THEN 'Unknown'
  WHEN (DateDiff(day, DateAdded, getdate()) = 0) THEN 'Today'
  WHEN (DateDiff(day, DateAdded, getdate()) = 1) THEN 'Yesterday'
  WHEN (DateDiff(day, DateAdded, getdate()) = -1) THEN 'Tomorrow'
  ELSE DATENAME(dw , DateAdded) 
END
FROM Table

Add A Column
==============
ALTER TABLE YourTableName ADD [ColumnName] [int] NULL;

Rename a Column
==============
EXEC sp_rename 'TableName.OldColName', 'NewColName','COLUMN';

Rename a Table
==============
EXEC sp_rename 'OldTableName', 'NewTableName';

Add a Foreign Key
==============
ALTER TABLE Products WITH CHECK 
ADD CONSTRAINT [FK_Prod_Man] FOREIGN KEY(ManufacturerID)
REFERENCES Manufacturers (ID);

Add a NULL Constraint
==============
ALTER TABLE TableName ALTER COLUMN ColumnName int NOT NULL;

Set Default Value for Column
==============
ALTER TABLE TableName ADD CONSTRAINT 
DF_TableName_ColumnName DEFAULT 0 FOR ColumnName;

Create an Index
==============
CREATE INDEX IX_Index_Name ON Table(Columns)

Check Constraint
==============
ALTER TABLE TableName
ADD CONSTRAINT CK_CheckName CHECK (ColumnValue > 1)

DROP a Column
==============
ALTER TABLE TableName DROP COLUMN ColumnName;

Single Line Comments
==============
SET @mojo = 1 --THIS IS A COMMENT

Multi-Line Comments
==============
/* This is a comment
	that can span
	multiple lines
*/

Try / Catch Statements
==============
BEGIN TRY
	-- try / catch requires SQLServer 2005 
	-- run your code here
END TRY
BEGIN CATCH
	PRINT 'Error Number: ' + str(error_number()) 
	PRINT 'Line Number: ' + str(error_line())
	PRINT error_message()
	-- handle error condition
END CATCH

While Loop
==============
DECLARE @i int
SET @i = 0
WHILE (@i < 10)
BEGIN
	SET @i = @i + 1
	PRINT @i
	IF (@i >= 10)
		BREAK
	ELSE
		CONTINUE
END

CREATE a Table
==============
CREATE TABLE TheNameOfYourTable (
  ID INT NOT NULL IDENTITY(1,1),
  DateAdded DATETIME DEFAULT(getdate()) NOT NULL,
  Description VARCHAR(100) NULL,
  IsGood BIT DEFAULT(0) NOT NULL,
  TotalPrice MONEY NOT NULL,  
  CategoryID int NOT NULL REFERENCES Categories(ID),
  PRIMARY KEY (ID)
);

User Defined Function
==============
CREATE FUNCTION dbo.DoStuff(@ID int)
RETURNS int
AS
BEGIN
  DECLARE @result int
  IF @ID = 0
	BEGIN
		RETURN 0
	END
  SELECT @result = COUNT(*) 
  FROM table WHERE ID = @ID
  RETURN @result
END
GO
SELECT dbo.DoStuff(0)
