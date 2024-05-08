# sqlinjection
Exploiting SQL Injection vulnerability.

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. 
Identify IP address using ifconfig in Metasploitable2

![e1](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/0286bb8f-a836-45d5-9bbe-319bf723b3c6)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![e2](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/255176ee-5512-4eaf-9492-50a8cd1d60f4)


Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![e3](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/344ed47d-fe70-42f4-8488-f9d4a3ddf1b9)

Click on the menu Login/Register and register for an account


![e4](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/3b7362c7-9173-4e6f-874e-33a642a3ad5d)


Click on the link “Please register here”
![e5](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/13505237-cc8d-4862-b7a3-c796c1aa9ed2)


Click on “Create Account” to display the following page:
![e6](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/1616550a-edc5-4688-8a24-f77c47d3e68c)


The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

Click “Login”. The logged in page will show as below:
![e7](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/4473f756-1676-45af-a176-60e9808fa02b)


## Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 

Click the login button and you will see it enter into the administrator page.
![e3](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/86a8a8c6-750b-47f9-bc3d-270655524056)



## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:
![e8](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/f895aad6-fd72-4150-b839-d4af88332d7f)
![e9](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/a51e5739-7849-4a2c-a0f4-bb8f855e19ae)
![e10](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/55e5656b-cf68-4dee-b175-8a30d384a737)


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

![e12](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/301b933b-eb17-4d93-ad08-6a79e891964e)


After adding the order by 6 into the existing url , the following error statement will be obtained:

![e13](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/12b0513b-e5d9-4230-a86e-910d83a20d0d)



When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![e14](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/2c6ffca7-2ca1-4350-b10b-984de539fcad)


 As it is having 5 columns the query worked fine and it provides the correct result

![e15](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/193d6b3e-c4da-4a15-908b-a11582cf8f39)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![e16](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/46cd756e-32c0-4e89-9fb1-80f7ca9aeca0)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![e16](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/46cd756e-32c0-4e89-9fb1-80f7ca9aeca0)
 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.


![e17](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/109dfd80-7d65-43f5-8741-b3f34d6debfb)


The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.
![e18](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/fc5640f0-93fd-4854-aadc-32cc49acdc96)

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

![e20](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/5f1df8fa-81e4-46eb-a907-8032fed577ac)

The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.


The column names of the accounts is displayed below for the following url:

![e21](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/af92f89a-9f2a-44df-9106-bfb2187c0e02)


Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

![e22](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/8af9a0dd-d58e-4b2e-a411-482aaccabf18)


## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).
![e23](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/88698a38-2a83-4b63-bcdf-c660ab78fa48)
![e24](https://github.com/vasundrasriravi/sqlinjection/assets/119393983/dd19bcab-77a8-4c0a-822c-e68a8871088a)


the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).




## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
