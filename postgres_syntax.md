### Start/stop server
pg_ctl -D /usr/local/var/postgres start
pg_ctl -D /usr/local/var/postgres stop

### Create/delete db
createdb mydb
dropdb mydb

### Access db
psql mydb

### Commands

\h for help with SQL commands. 
\? for help with psql commands.  
\g or terminate with semicolon to execute query.  
\q to quit psql.  
\d describe list of tables.  
\d tablename : describe table.  
\l list all databases.  
INSERT INTO product VALUES(DEFAULT, 'Banana', '4011');   
SELECT * FROM product;  
