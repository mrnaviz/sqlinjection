# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/6cd56b49-88a5-49f6-b271-24cc7f38c8bc)

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:

SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database.

Identify IP address using ifconfig in Metasploitable2

![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/9c9d5db0-03f7-4e56-b8c4-1456f29677cf)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.



![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/10011f47-07e6-4a1e-ad06-eea910e38eba)


Select Multidae from the menu listed as shown above. You will get the page as displayed below:


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/8b7cf9d2-b98a-4c47-b2eb-585471813ef8)

Click on the menu Login/Register and register for an account


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/85d01efd-c442-4b66-bbb3-2af14c5f9f2c)

Click on the link “Please register here”


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/5058ced6-6c66-444f-a802-f0b996e482e9)


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/bcf54128-5054-4b55-ba0f-182aed407faf)

Click on “Create Account” to display the following page:


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/c8864ce1-9b68-45cc-89f6-94ac5bce8b9c)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/cf8f4c46-3281-4a08-bfc0-73f7de717241)

Click “Login”. The logged in page will show as below:


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/71ba205e-40d4-406f-966a-31b07106d9f8)

## Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/41b6685d-2ef3-490d-be89-e3dd5ec24b85)

Click the login button and you will see it enter into the administrator page.


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/a5e2c5bd-c24c-4e18-b143-ae30a9881cf6)

## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/4b59d481-2a8d-40ae-94cc-da38ef27df0f)


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/8fe11a73-527b-454d-95fb-8e783167942d)


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/8bd5d40c-d731-463e-a95a-546eba1faec3)


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/bc6c821b-4cce-4cc9-a073-6b33ea8a99b9)



![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/ba7e362e-4036-4940-8b7b-e88980e82146)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/d064ad48-c0ef-488b-8ecf-16f9798abf2c)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/1bc4a4f4-6e33-4f4a-b251-ba0ed70f79db)

After adding the order by 6 into the existing url , the following error statement will be obtained:


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/72e6d4cd-eeae-48b4-9d52-57e587c70101)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/0d8836ed-0007-4e1c-9f95-56556ff17869)

As it is having 5 columns the query worked fine and it provides the correct result


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/ec70d66f-56c0-4e83-b8f7-44d2ee0535e9)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union)


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/b82fd7cc-2952-4be6-85f2-c88b2a1d6e74)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/35232704-922a-42d2-8f04-08aab1810ea5)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/07374d0c-de05-4446-ae7f-2c0996624bfb)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/4080d1f4-dc63-4760-b821-d8e152172ee5)

The url once executed will retrieve table names from the “owasp 10” database.

## Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/4c6cf630-1685-4937-b2ca-5c42a4b956ec)


The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/9a1642ee-718f-4e6a-ac26-107b39d23b85)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/13bb1c48-64c7-4633-bc26-7392f08b6fb3)

## Reading and writing files on the web-server

We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/mrnaviz/sqlinjection/assets/123350791/ad40825f-ce89-447d-b5cb-0f49fc8f49b2)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
