
===============================================================================
                        DOLIBARR MYSQL BACKUP SCRIPT
===============================================================================

Script Name : BackupDolibarrMysql.sh
Author      : dAtUmErA
Description : Automates backup, gzip compression, logging, and email
              notifications with file attachments for a Dolibarr MySQL database.

===============================================================================
1. FEATURES
===============================================================================

* Timestamped Output    : Generates a unique timestamp (YYYYMMDD_HHMMSS) for
                          each execution to name logs, dumps, and compressed
                          archives.
* MySQL Database Backup : Executes mysqldump against the target 'dolibarr'
                          database.
* Archive Compression   : Compresses generated .sql files using gzip with
                          maximum compression level (-9).
* Multi-channel Logging : Simultaneously writes progress status and step results
                          to stdout, dedicated log files, and an email message
                          body file.
* Email Notification    : Uses mutt to send the compressed .sql.gz backup as
                          an email attachment to the administrator.

===============================================================================
2. PREREQUISITES
===============================================================================

[A] Required Applications:
    - Bash shell (/bin/bash)
    - mysqldump (MySQL/MariaDB CLI client)
    - gzip (Compression utility)
    - mutt (Command-line email client)

[B] Required Directory Structure:
    /opt/backups/
    |-- conf/
    |-- logs/
    |-- mysql_compressed/
    `-- mysql_data/

===============================================================================
3. WORKFLOW EXECUTION STEPS
===============================================================================

Step 1: Timestamp Initialization
--------------------------------
- Captures current date and time (date +%Y%m%d_%H%M%S).
- Creates:
  * /opt/backups/logs/mysql_dolibarr_backup_<timestamp>.log
  * /opt/backups/conf/mailmessage_backup_dolibarr

Step 2: Database Backup (mysqldump)
-----------------------------------
- Runs: mysqldump -u root -pXXXXXXX dolibarr
- Output file: /opt/backups/mysql_data/dolibarr_backup_<timestamp>.sql
- Checks exit status ($?).

Step 3: Compression (gzip)
--------------------------
- Compresses uncompressed SQL file.
- Output file: /opt/backups/mysql_compressed/dolibarr_compressed_<timestamp>.sql.gz
- Checks exit status ($?).

Step 4: Email Delivery (mutt)
-----------------------------
- Sends email from: Admin <admin@aeipcanaurell.org>
- Sends email to  : aeipcanaurell@gmail.com
- Includes .sql.gz attachment and body text from configuration file.
- Exits with code 1 if delivery fails.

===============================================================================
4. OUTPUT FILES CREATED PER EXECUTION
===============================================================================

Path                                                   Description
-------------------------------------------------------------------------------
/opt/backups/logs/mysql_dolibarr_backup_<timestamp>.log Execution log file.
/opt/backups/conf/mailmessage_backup_dolibarr          Email body text file.
/opt/backups/mysql_data/dolibarr_backup_<timestamp>.sql Raw SQL dump file.
/opt/backups/mysql_compressed/dolibarr_compressed_... Gzipped SQL archive.

===============================================================================
5. USAGE
===============================================================================

Grant execution permissions and execute the script directly or configure it
inside crontab:

    $ chmod +x BackupDolibarrMysql.sh
    $ ./BackupDolibarrMysql.sh

===============================================================================
