# Can't connect to local MySQL server through socket

### Question

I am getting the following error when I try to connect to mysql:

> Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)

Is there a solution for this error? What might be the reason behind it?

### Answer

Are you connecting to "`localhost`" or "`127.0.0.1`" ? 

I noticed that when you connect to "localhost" the socket connector is used, 

but when you connect to "127.0.0.1" the TCP/IP connector is used. 

You could try using "127.0.0.1" if the socket connector is not enabled/working.

# 链接

[Can't connect to local MySQL server through socket](http://stackoverflow.com/questions/4448467/cant-connect-to-local-mysql-server-through-socket-var-lib-mysql-mysql-sock)
