# sqlinjection
Exploiting SQL Injection vulnerability

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
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2

![pp1](https://github.com/Sharmilasha/sqlinjection/assets/94506182/b795a636-505f-471a-8f17-5d365d499915)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![ccm](https://github.com/Sharmilasha/sqlinjection/assets/94506182/2e16dd46-e25b-4124-9aca-f5689694d801)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:
![mmi](https://github.com/Sharmilasha/sqlinjection/assets/94506182/5b47f5b7-0627-4a31-b93e-5b9a9b8decc8)

Click on the link “Please register here :
![bbm](https://github.com/Sharmilasha/sqlinjection/assets/94506182/8d0ca16a-c56a-4ffa-bcf2-657d5c0096af)

Click on “Create Account” to display the following page

![ppm](https://github.com/Sharmilasha/sqlinjection/assets/94506182/429a8d4c-a6c5-4fb5-b961-f41a80e2a798)

Click “Login”. The logged in page will show as below
![hhm](https://github.com/Sharmilasha/sqlinjection/assets/94506182/076dd688-2d06-4dfe-a29f-3a5a7ee2a896)

Bypassing login field
The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![uoom](https://github.com/Sharmilasha/sqlinjection/assets/94506182/16c8e115-10bb-405a-abaa-e278ab92c96a)

Click the login button and you will see it enter into the administrator page.

![mul](https://github.com/Sharmilasha/sqlinjection/assets/94506182/9a85e19f-fb68-46bf-b1f4-a8414ba9ca9c)

Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below

![jj2](https://github.com/Sharmilasha/sqlinjection/assets/94506182/7b3c14da-5fa0-4054-b99a-d07b6893d4b3)
v![mucc](https://github.com/Sharmilasha/sqlinjection/assets/94506182/ff712456-15c0-47ad-85f5-7c50be5d3276)
![bbr](https://github.com/Sharmilasha/sqlinjection/assets/94506182/386a728a-9cf4-4fb9-ac82-de1b486c97ed)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![Screenshot 2023-11-09 101429](https://github.com/Sharmilasha/sqlinjection/assets/94506182/51dca208-907e-447a-9374-8037a2fe8841)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6. The browser url of this info page need to be modified with the url as below

![Screenshot 2023-11-09 101429](https://github.com/Sharmilasha/sqlinjection/assets/94506182/1735106e-00af-454e-941c-094ed9553075)
When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![uum](https://github.com/Sharmilasha/sqlinjection/assets/94506182/2059546c-08e5-4a6a-ae4c-c2708c908352)

The column names of the accounts is displayed below for the following url: http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=bharathi%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

![hgg](https://github.com/Sharmilasha/sqlinjection/assets/94506182/abc6acc1-cb16-4bc3-8ad8-c53425feb7b4)
Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence. Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=bharathi%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Sharmilasha/sqlinjection/assets/94506182/7c4e75b9-ba30-4108-8bc2-778078ff381f)
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=bharathi%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![ukk](https://github.com/Sharmilasha/sqlinjection/assets/94506182/9dd25c65-25aa-405f-8b96-e3c11163b46b)

![eem](https://github.com/Sharmilasha/sqlinjection/assets/94506182/ce6147b7-38ea-4d15-b659-12a07d45da23)
![ppg](https://github.com/Sharmilasha/sqlinjection/assets/94506182/5791a6af-7483-44ff-917d-47854eb05979)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).
## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
