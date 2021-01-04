# SQL-Solutions
A series of solutions to SQL challenges using a variety of databases. 

# SQL Statement Fundamentals

## SELECT

 Syntax: 

```sql
SELECT column_name FROM table;  
```

- To separate columns use a comma
- The asterisk (*) symbol returns all columns. (this symbol should only be used when you actually need to return all columns. It's not best practice to use this symbol because it can cause the query to take longer to run.

## SELECT DISTINCT

Syntax: 

```sql
SELECT DISTINCT(column) FROM table; *or* SELECT DISTINCT column FROM table; 
```

Use Case: The SELECT DISTINCT statement is used when a table contains a column that has duplicate values and you may only want a list of the unique values. 

Examples: 

```sql
SELECT DISTINCT(rental_rate) FROM film
ORDER BY 1 DESC;
```

## COUNT

Syntax: 

```sql
SELECT COUNT(column_name) FROM table; 
```

The count function will not work without parentheses.

Use case: The count function will only return the count of rows in a column. 

**Pro tip - Use the count function with the distinct function to know the count of distinct rows in a column.** 

Syntax Example: 

```sql
SELECT COUNT(DISTINCT column_name) FROM table; 
```

More Examples of Count function: 

```sql
SELECT COUNT(*) FROM payment;
```

```sql
SELECT COUNT(DISTINCT amount) FROM payment;
```


## SELECT WHERE

***SELECT WHERE is a fundamental statement that allows us to define conditions of the columns returned back.*** 

Capitalization matters. 

Syntax: 

```sql
SELECT column1, column2 
FROM table 
WHERE conditions;
```

Conditions: 

[Copy of Comparison Operators ](https://www.notion.so/4ccc971933ae41bf9a7514a93d27e4fe)

### Logical Operators

Allows us to combine multiple comparison operators. 

- AND
- OR
- NOT

Example: Someone needs a table with the results of davids choices only. 

```sql
SELECT name, choice FROM table WHERE name = 'David'
```

More Examples of SELECT WHERE 

```sql
# customers named Jared 
SELECT * FROM customer
WHERE first_name = 'Jared'
```

```sql
# films where the rental rate is more than 4
# and the replacement cost greater or equal to 19.99 and rating is R
SELECT * FROM film
WHERE rental_rate > 4 AND replacement_cost >= 19.99
AND rating = 'R';
```

## ORDER BY

The ORDER BY function is used to sort rows based on a column value in either ascending or descending order (alphabetical or numerical). 

Syntax: 

```sql
SELECT column_1, column_2
FROM table
ORDER BY column_1 ASC/DESC
```

***ORDER BY is towards the bottom of the query.***

*You can order by columns that are not selected in the query. (not recommended)*

## LIMIT

The LIMIT function goes at the very end on the query and it is used to limit the number of rows returned. 

Syntax: 

```sql
SELECT column_1, column_2
FROM table 
 WHERE conditions
ORDER BY column_1
LIMIT 5
```

Example:

```sql
# The 5 most recent payments where the amount paid was not 0.00
SELECT * FROM payment
WHERE amount != 0.00
ORDER BY payment_date DESC
LIMIT 5;
```

## BETWEEN

The between operator can be used to match a value against a range of values. 

BETWEEN & NOT can be combined

The BETWEEN operator can be used with dates but the date needs to be formatted as YYYY-MM-DD 

Syntax Example: 

```sql
#return all data from payment table where the amount spent is between 8 and 9 dollars
SELECT * FROM payment
WHERE amount BETWEEN 8 AND 9;
```

```sql
#returns all the payments between this date
SELECT * FROM payment
WHERE payment_date BETWEEN '2007-02-01' AND '2007-02-15';
```

*helpful hint: you have to be careful when using date/time and the between operation. Postgres defines time as up to the 0:00 mark.*  

## IN

used in certain cases when you want to check for multiple possible value options. For example, if a user name shows up in a list of unknown names. 

Syntax: 

```sql
#the comma in the IN function reflects or. This query will return either value1 or value2
SELECT column_name FROM table
WHERE column_name IN( 'value1', 'value2')

# IN function can be combined with NOT operator 
SELECT column_name FROM table
WHERE column_name NOT IN( 'value1', 'value2')
```

## LIKE AND ILIKE

Using pattern matching with String data

The Like operator allows us to perform pattern matching against string data with the use of wildcard characters: 

- Percent % (Matches any sequence of characters)
- Underscore _  (Matches any single character)

**LIKE is *case-sensitive* and ILIKE is *case-insensitive*** 

Syntax: 

```sql
#Returns a list of every record where the name begins with the letter J 
SELECT * FROM customer 
WHERE first_name LIKE 'J%'

#Returns a list of every record where the name begins with the letter J and the last name begins with the letter S 
SELECT * FROM customer 
WHERE first_name LIKE 'J%' AND last_name LIKE 'S%'

#Returns a list of every record where the name ends with the letter A 
SELECT * FROM customer 
WHERE first_name LIKE '%a'

# Returns a list of every record where someone has 'er' somewhere in their first name
SELECT * FROM customer 
WHERE first_name LIKE '%er%'

# Returns a list of every record where someone does not have 'her' somewhere in their first name
SELECT * FROM customer 
WHERE first_name NOT LIKE '%her%'
```

## Postgres SQL Challenges

1. Challenge: SELECT 

    Use a SELECT statement to grab the first and last name of every customer and their email address.

    ```sql
    SELECT first_name, last_name, email FROM customer;
    ```

2. Challenge SELECT DISTINCT 

    An Australian visitor isn't familiar with MPAA movie ratings (PG, PG-13, R, etc.) We want to know the type of ratings we have in our database. What ratings do we have available 

    ```sql
    SELECT DISTINCT (rating), COUNT(rating) AS "Number of Films" FROM film 
    GROUP BY 1
    ORDER BY 2 DESC;
    ```

    
3. Challenge SELECT WHERE

    A customer forgot their wallet at our store! We need to track down their email to inform them. What is the email for the customer with the name Nancy Thomas? 

    ```sql
    SELECT email FROM customer
    WHERE first_name = 'Nancy' AND last_name = 'Thomas';
    ```

    A customer wants to know what the movie "Outlaw Hanky" is about. Could you give them a description of the movie? 

    ```sql
    SELECT description FROM film
    WHERE title = 'Outlaw Hanky'
    ```

    A customer is late on their movie return, and we mailed them a letter to their address at '259 Ipoh Drive'. We should also call them on the phone to let them know. Can you get the phone number for the customer who lives at '259 Ipoh Drive'? 

    ```sql
    SELECT phone FROM address
    WHERE address = '259 Ipoh Drive';
    ```

4. Challenge ORDER BY 

    We want to reward our first 10 paying customers. What are the customer ids of the first 10 customers who created a payment? 

```sql
SELECT customer_id
FROM payment
ORDER BY payment_date ASC
LIMIT 10;
```

 A customer wants to quickly rent a video to watch over their short lunch break. What are the 5 shortest (in length of runtime) movies? 

```sql
SELECT title, length
FROM film
ORDER BY length ASC
LIMIT 5
```

Bonus Question: If the previous customer can watch any movie that is 50 minutes or less in run time, how many options does she have? 

```sql
SELECT COUNT (title)
FROM film
WHERE length <= 50
```

SECTION 2: General challenge 

1. How many payment transactions were greater than $5.00? 

```sql
SELECT COUNT(payment_id) FROM payment 
WHERE amount > 5.00;
```

2. How many actors have a first name that starts with the letter P? 

```sql
SELECT COUNT(*) FROM actor 
WHERE first_name LIKE 'P%';
```

3. How many unique districts are our customers from? 

```sql
SELECT COUNT(DISTINCT district)
FROM address;
```

4. Retrieve the list of names for those distinct district from the previous question

```sql
SELECT DISTINCT district
FROM address;
```

5. How many films have a rating of R and a replacement cost between $5 and $15? 

```sql
SELECT COUNT (*) FROM film 
WHERE rating = 'R' AND 
replacement_cost BETWEEN 5 AND 15;
```

How many films have the word Truman somewhere in the title? 

```sql
SELECT COUNT (*) FROM film 
WHERE title LIKE '%Truman%'
```
