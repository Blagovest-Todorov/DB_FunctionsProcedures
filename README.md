# DB_FunctionsProcedures
What is function what is procedure usage
Procedures are changing the state of DB, functions not,
Procedures are sequence of selects statements.
Scalar functions return only one variable value result, 
procedures return multiple value variable  result.
Procedures are more flexible , give more field of usage.
StoredProcedure can call a functions and another SP.
function can not call another StroredProcedure.

Conclusions
Stored procedures in SQL are easier to create and functions have a more rigid structure and support less clauses and functionality. By the other hand, you can easily use the function results in T-SQL. We show how to concatenate a function with a string. Manipulating results from a stored procedure is more complex.

In a scalar function, you can return only one variable and in a stored procedure multiple variables. However, to call the output variables in a stored procedure, it is necessary to declare variables outside the procedure to invoke it.

In addition, you cannot invoke procedures within a function. By the other hand, in a procedure you can invoke functions and stored procedures.

Finally, it is important to mention some performance problems when we use functions. However, this disadvantage will be explained in a next article, Functions and stored procedures comparisons in SQL Server.

SOFTUNI Task 

CREATE OR ALTER PROC usp_GetTownsStartingWith
	    @inputString VARCHAR(50)
     AS 
	   SELECT [Name] 
	     FROM Towns
		 WHERE [Name] LIKE @inputString + '%'

     EXEC usp_GetTownsStartingWith 'b'
//////

 --Write a stored procedure usp_GetEmployeesFromTown that accepts town name as parameter and return the employees’ first and last name that live in the given town. 

	 CREATE PROC usp_GetEmployeesFromTown
	     @inputTown VARCHAR(50)
     AS 
	   SELECT 
	        e.FirstName, 
	        e.LastName 
		FROM Employees AS e
			JOIN Addresses AS a ON a.AddressID = e.AddressID
			JOIN Towns AS t ON t.TownID = a.TownID
			WHERE t.[Name] = @inputTown

   EXEC usp_GetEmployeesFromTown 'Sofia'
   
   
   ///

EXAMPLE WITH FUNCTION :

--FV=I×(〖(1+R)〗^T)
--ufn_CalculateFutureValue\


--Your task is to create a function ufn_CalculateFutureValue that accepts as parameters – sum (decimal), yearly interest rate (float) and number of years(int). It should calculate and return the future value of the initial sum rounded to the fourth digit after the decimal delimiter. Using the following formula:
--FV=I×(〖(1+R)〗^T)
--	I – Initial sum (start SUM)
--	R – Yearly interest rate
--	T – Number of years

CREATE FUNCTION ufn_CalculateFutureValue(@sum DECIMAL(18, 4), @yearlyInteresRate FLOAT, @numbYears INT)
RETURNS DECIMAL(18, 4) 
BEGIN 
    RETURN @sum * POWER ((1 + @yearlyInteresRate), @numbYears)
END 

SELECT dbo.ufn_CalculateFutureValue(1000, 0.1, 5)

///

CREATE PROC usp_DepositMoney @accountId INT, @moneyAmount DECIMAL(15, 4)
AS
BEGIN
   BEGIN TRANSACTION 
   IF(@moneyAmount <= 0)
   BEGIN
       RAISERROR('Error sum must be positive', 16, 1)
	   ROLLBACK
	   RETURN
   END
     UPDATE Accounts
	 SET Balance = Balance + @moneyAmount
	 WHERE Id = @accountId

	 IF(@@ROWCOUNT <> 1)
	 BEGIN
	    RAISERROR('invalid account', 16, 2)
		ROLLBACK;
		RETURN;
	 END 
	 COMMIT
END
////




