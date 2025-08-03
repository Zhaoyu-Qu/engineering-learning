# QuickStart
## macOS
1. Install MariaDB via homebrew: `brew install mariadb`
2. To start mariadb now and restart at login: `brew services start mariadb` (specific to macOS), or `service mysql start` (Linux). To nitiate a temporary session: `/opt/homebrew/opt/mariadb/bin/mariadbd-safe --datadir\=/opt/homebrew/var/mysql` or `mysql.server start`. See homebrew prompts upon successful installation.
3. Connect to MariaDB: `mariadb`
4. Create new user: `CREATE USER 'abcdef'@'localhost' IDENTIFIED BY '1qw23er45t';`
5. Create new database: `create database devinternhubdb;`
6. Grant access to new user: `GRANT ALL ON devinternhubdb.* TO 'abcdef'@'localhost';`
7. Exit: `exit`
8. Connect from new user: `mariadb -u abcdef -p1qw23er45t devinternhubdb`
9. Exit: `exit`

# Terminal Commands
- `mysql.server start`, `service mysql start` (system service Linux), `mysql.server stop`, `mysql.server status`
- `mariadb` - connect to MariaDB using only default values with the mariadb client
  - The host name is `localhost`.
  - The user name is either your Unix login name, or `ODBC` on Windows.
  - No password is sent.
  - The client will connect to the server with the default socket, but not any particular database on the server.
- `mariadb -h 166.78.144.191 -u username -ppassword database_name` - connect to MariaDB with parameters
  - `-h` specifies a host. Instead of using localhost, the IP `166.78.144.191` is used.
  - `-u` specifies a user name, in this case username.
  - `-p` specifies a password, password. Note that for passwords, unlike the other parameters, there cannot be a space between the option (`-p`) and the value (`password`). It is also not secure to use a password in this way, as other users on the system can see it as part of the command that has been run. If you include the `-p` option, but leave out the password, you will be prompted for it, which is more secure.
  - The database name is provided as the first argument after all the options, in this case `database_name`.
  - It will connect with the default tcp_ip port, 3306
- `SELECT USER(),CURRENT_USER();` - check current user

# References
- [Connect to MariaDB] (https://mariadb.com/docs/server/server-usage/connecting/mariadb-connecting-guide-1)
- [MariaDB Commands Cheatsheet] (https://gist.github.com/Jonasdero/b4c8a5e284e13aaf456f44f5dc60257e)