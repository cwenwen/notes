# Back End Basics

## What is DNS? Google provides public DNS. What are benefits for Google and for users?

The Domain Name System (DNS) is the phonebook of the Internet.  
It turns domain names into IP addresses. Every time we visit a website, our computer performs a DNS lookup.  
For instance, the domain name Google.com has an IP address of 172.217.27.142. We can access the web page through the domain name while the browser interacts through the IP address. And the DNS is an in-between bridge.  

#### [Google Public DNS](https://developers.google.com/speed/public-dns/)  

Benefits for users:  

- Speeding up the browsing experience  
- Improving the security  
- Getting the expected results with no redirection  

Benefits for Google:  

- To get traffics of websites for statistics and analytics

## What is "lock" in database? Why we need it?

Locking is essential to successful SQL Server transactions processing.  

It is designed to allow SQL Server to work seamlessly in a multi-user environment and to ensure the integrity of the data in the database, as it forces every SQL Server transaction to pass the ACID test.  

## What is ACID in Databases?

ACID test consists of 4 requirements that every transaction have to pass successfully:  

- **Atomicity**  
  It means all or nothing.  
  Transactions often contain multiple separate actions. For example, a transaction may insert data into one table, delete from another table, and update a third table. Atomicity ensures that either all of these actions occur or none at all.

- **Consistency**  
  Transactions always take the database from one consistent state to another. A transaction must create a valid state of new data, or it must roll back all data to the state that existed before the transaction was executed.

- **Isolation**  
  A transaction that is still running and did not commit all data yet, must stay isolated from all other transactions.  

- **Durability**  
  Committed transactions must be stored using method that will preserve all data in correct state and available to a user, even in case of a failure.

## What is the difference between NoSQL and SQL?

**With SQL databases**, all data has an inherent structure. A conventional database like Microsoft SQL Server, MySQL, or Oracle Database uses a schema — a formal definition of how data inserted into the database will be composed.  
For instance, a given column in a table may be restricted to integers only. As a result, the data recorded in the column will have a high degree of normalization. A SQL database’s rigid schema also makes it relatively easy to perform aggregations on the data, for instance by way of JOINs.  

**With NoSQL**, data can be stored in a schema-less or free-form fashion. Any data can be stored in any record.  

![SQL vs NoSQL](https://www.netsolutions.com/insights/wp-content/uploads/2014/07/5_things_you_must_consider_before_nosql1.jpg)