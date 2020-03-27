# Open Source DBs
- Postgres: continues to grow in popularity
- MySQL (owned by oracle)
- MariaDB
Locally or in the cloud. 


# SQL 

- name: structured query lang
- relational: data distributed  many tables with diff fields. connections between tables.
    - one-to-one, one-to-many , many-to-many (extra column) 
    - can be merged together with sequel queries
- **schema on write**: all records have same schema. Every new record/row has to contain these fields  
- stored and managed in one place: update only once 
- can be worse performance. merging of data through a query

- Visual Tool posSQL, for mac: SequelPRO

### Tutorial
- [4 hour youtube](https://www.youtube.com/watch?v=HXV3zeQKqGY)



# NoSQL (e.g. Mongo DB)
- Consistency : Applies to BASE Model(Basically Available, Soft State, Eventual consistency)
- Document-oriented stores encapsulate key value pairs in JSON or JSON like documents
- mongoDB = humongous 
- no tables but collections
- no schema. where a field is missing
    - downside: not sure if the value will exist 
        - duplicates. Therefore: good for many reads and less writes. otherwise updating in diff places
    - up: flexible 
- no relations. can set it up manually. 
    - data you need in each collection. no need to query diff tables
- collections serve certain purposes
- great performance for many reads/writes (bad for constant updating: thousands per second


# Postgres 

### Resources postgres
- Install/Setup : https://www.robinwieruch.de/postgres-sql-macos-setup
- Tutorial with example database: 
    - https://www.postgresqltutorial.com
    - http://postgresguide.com/utilities/psql.html



# HANA: High Performance Analytical Appliance 

- ERP: enterprise resource planning = business process management
- in-memory db: all operations read/write  done in memory (can be done in cloud)
- web server attached to it
- data stored in columns (traditionally in rows) faster for average/aggregation on columns. e.g. avg salary



## MongoDB 

- Non-relational

- [Tutorial](https://docs.mongodb.com/manual/tutorial/query-documents/)




# Hadoop
-  Schema on read: java-based query defined the structure
-  Reading continuously updating data
-  Distributed over diff servers
