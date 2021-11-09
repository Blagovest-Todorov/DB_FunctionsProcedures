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
