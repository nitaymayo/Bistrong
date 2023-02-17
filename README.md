# BISTrong - Pilot course sport social web
"Bistrong" is a comprehensive web application designed for tracking and training soldiers in the Israeli Air Force pilot course. The app provides detailed information about various workout drills and practices, allowing soldiers to stay on top of their physical and mental training. With Bistrong, soldiers can stay in touch with their coaches, get instant feedback, and track their progress over time.

*The app currently works only with mysql server
<h2>Sign In Screenshot</h2>
<img src="Signin_screenshot.png" alt="sign in page screenshot">

<h2>Home Page Screenshot</h2>
<img src="Homepage_screenshot.png" alt="Home page screenshot">


### Setup and run instructions:

#### Prerequisites:
Open the project's dir in the terminal and run the following commands:
1. pip install virtualenv
1. virtualenv venv
1. pip install python-dotenv
1. pip install mysql-connector-python
1. download and run mysql server, after the server start please run the sql DUMP file
<br/>

#### Configurations:
Open the project's dir in the terminal and:
1. Run the command: **python -c "import os; print(os.urandom(16))"**
1. Copy the output string
1. Edit the project's **.env** file found inside the root folder
1. Overwrite the **SECRET_KEY** value with the string you copied
<br/>
 
#### Run:
 Run the project with your IDE's configuration, or from the terminal by using the **flask run** command
 
 ##
 
### Usage:

#### Connecting and querying a database:
1. Edit **.env** file with your database details

**METHOD 1 - Query the database by using the implemented DBManager module**
1. A module named **DBManager** exists for querying the database.
1. On `.py` files you'd like to connect and query the database, use the following import:
    ```python
    from utilities.db.db_manager import dbManager
    ```
1. Then use one of the methods ```commit(query, args=())```, ```fetch(query, args=())```, ```execute(query, args=())``` to query the database according to the explanation below:
    <ul>
        <li>
            <code>commit(query, args=())</code>
            <br/>
            Use this method for <strong>INSERT</strong>, <strong>UPDATE</strong> and <strong>DELETE</strong> queries.
            <br/>
            Returns number of affected rows.
        </li>
        <li>
            <code>fetch(query, args=())</code>
            <br/>
            Use this method for <strong>SELECT</strong> queries.
            <br/>
            Returns:
            <br/>
            - The query result as a list of named tuples where fields are accessible with dot notation.
            Read more about it <a href="https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor.html">here.</a>
            <br/>
            - False if query fails.
        </li>
        <li>
            <code>execute(query, args=())</code>
            <br/>
            Use this method for <strong>CREATE...</strong>, <strong>ALTER...</strong> and <strong>DROP...</strong> queries.
            <br/>
            Returns:
            <br/>
            - True if query is successful.
            <br/>
            - False if query fails.
        </li>
    </ul>

   Example:
   ```python
   from utilities.db.db_manager import dbManager
   query_result = dbManager.fetch('SELECT * FROM example_table')
   if query_result:
       for record in query_result:
           print(record.id) #prints the record's ID 
   ```
<br/>

**METHOD 2 - DIY**
1. On `.py` files you'd like to connect to the database, use the following imports:
    ```python
    from settings import DB
    import mysql.connector
    ```
1. Connect to the database with **DB as the argument
    ```python
   db_connection = mysql.connector.connect(**DB)
   ```
1. **Tip**: If you'd like the queried results to behave like JavaScript objects then instantiate cursor as follows:
    ```python
   db_cursor = db_connection.cursor(named_tuple=True)
   ``` 
   Once you do that, you'll be able access the properties with dot notation.
   
   Read more about it [here](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor.html)
   
   Example:
   ```python
   from settings import DB
   import mysql.connector
   db_connection = mysql.connector.connect(**DB)
   db_cursor = db_connection.cursor(named_tuple=True)
   db_cursor.execute('SELECT * FROM example_table')
   records = db_cursor.fetchall()
   for record in records:
       print(record.id) #prints the record's ID 
   ```
