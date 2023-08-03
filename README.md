# Data Engineering  + Iterview Questions

## Data Engineer Interview Questions
A collection of technical interview questions for Data Engineer position based on my day-to-day experience with real problems.

### Technical Questions.

#### 1. OLAP vs OLTP Explain it.
OLAP: Online Analitical processing, This is not the official definition (if there is one). But we could say that it is a way of processing data in such a way that it is used for analysis. It is the technology behind BI, its function is not operational in the sense of running a database to make transacion(add new user, update, delete..) but to extract value from the data.See how entities relate to each other.

#### 2. Slow changing dimension.

#### 3. Database Schemas wich one do you know. Use-cases

#### 4. Normal Form.

#### 5. CAP - Theorem

In distributed systems there are 3 main things that you need to be: be consistent, available, and partition tolerant.

Being consistent means that when you ask the computer for information, it will always give you the same answer.

Being available means that the computer is always ready to give you an answer when you ask for it.

Partition tolerance means that the computer can still work even if some parts of it stop talking to each other.

The CAP theorem says that a distributed system can only have two of these three things at once. So a computer system can only have two of these three features at a time.

For example, if you have a computer system that needs to be always available and partition tolerant, it may not be consistent. 
This means that sometimes it may give you different answers depending on which part of the system you ask.

##### Wich 2 should you choice ?

The choice of which two features to prioritize in a distributed system depends on the specific requirements of the system and the needs of the users.

For example, if you are building a system that handles financial transactions, consistency and partition tolerance might be the most important features to prioritize.
This is because you want to make sure that every transaction is recorded accurately and reliably, and that the system continues to work even if some parts of it fail.

On the other hand, if you are building a system for content delivery, availability and partition tolerance might be more important.
 This is because you want to make sure that users can always access the content they need, and that the system can continue to work even if some parts of it are 
 temporarily unavailable.

So, the choice of which two features to prioritize really depends on the specific needs and goals of the system. It's important to carefully consider these factors and make an informed decision.


#### 6. ACID properties.

#### 7. Big data, the v´s

#### 8. What is a View in Database and when does it make sense.

#### 9. SQL vs NoSQL

#### 10.Which kinds of Databases do you know ?

#### 11. Advantages and disadvantages of the Databases named above.

#### 12. Map-Reduce

### Questions based on project experience.

#### 21. A typical task of a data engineer is to migrate an ETL that is currently running on A and has to start running on B.
Where A and B are different enviroments.This may be due to different reasons, such as the choice of a new technology (due to licensing issues
or that it is more convenient in terms of scalability to run it in the new environment).You are in charge of this project. What is the first thing you would do? 

- Establishing contact with the parties involved is always the first step in a project like this, in this case it would be the person from Business who has commissioned this project.
- The second thing to do is to check where the data comes from in the original process and establish the mechanisms to get that data to the source of the new process.
  If both processes do not have the same input data, the old process and the,new process will have no way of verifying that it is correct, so the first step is to establish that both have the same input data.
- Once we have clarified the issue of data collection, we will be interested to know where we have to take the data.
- It is interesting to look at what technologies were used for the original ETL process, if it was an ETL that only used SQL code and our task is to carry the code in a Databricks environment but we can choose whether we want to use pyspark or sparksql we may be interested in replicating the code in SparkSQL, if on the other hand the original ETL used other languages 
 (either because it communicated via APIs when receiving the data) we may be interested in using python + pyspark. There is no single rule.
 
#### 22.(continuation of the first question) You are migrating the ETL process to a new technology, what strategy would you use to ensure that the outcome in the original and the final process is the same.How are you checking the results ? 

#### 23. Can you tell me about a situation that was a particular challenge?

#### 24. In your years working as a Data Engineer, can you tell me about a mistake you made in the past and how you have learned from it?


#### 25. In terms of Data warehouse what is a Change Data Capture know as CDC ?
CDC is a mechanism to keep databases that should be identical (replicas) updated, in a DWH context it may be that we have the same information at different points of our architecture for different purposes,
the mechanism to keep these databases updated is CDC

#### 26. Can you make a example of use of CDC ?

Let's imagine that we have a store that sells electrical appliances, if our business is very small we could use a database to support the OLTP actions and we could also use some analysis queries in our database,
such as knowing which appliance has been sold. more, or when money we have generated etc..

while doing this may be acceptable in a small company with a small database, a large organization with a very large database is not a good idea, it is not a good idea to use the same table for doing transactions as for Analysis, for what is chosen in these cases to replicate the database (in our example the stock table)

then we use table 1 for OLTP operations and table 2 which is the copy of table 1 for OLAP operations.

The way to synchronize the 2 tables is through CDC


Example:

Initial State:

Database 1 (Source):
diff

```
+----+------------+-------+
| ID | Product    | Stock |
+----+------------+-------+
| 1  | Laptop     | 50    |
| 2  | Smartphone | 30    |
| 3  | Tablet     | 20    |
+----+------------+-------+
```

Database 2 (Target, copy of Database 1 at the time of copying):
diff


+----+------------+-------+
| ID | Product    | Stock |
+----+------------+-------+
| 1  | Laptop     | 50    |
| 2  | Smartphone | 30    |
| 3  | Tablet     | 20    |
+----+------------+-------+
Change:
A customer purchases a Smartphone from Database 1, reducing its stock by 1.

sql
Copy code
UPDATE products SET Stock = Stock - 1 WHERE Product = 'Smartphone';
CDC Capture:
The CDC system in Database 1 detects this change and captures it in a CDC log or journal.

yaml
Copy code
Transaction ID: 12345
Table: products
Operation: UPDATE
Primary Key: ID=2
Column: Stock
Old Value: 30
New Value: 29
CDC Apply:
The CDC system reads the captured change and applies it to Database 2. It updates the corresponding record for the Smartphone product with the new stock value of 29.

Result:
After applying the change, both Database 1 and Database 2 will have the same values:

diff
Copy code
+----+------------+-------+
| ID | Product    | Stock |
+----+------------+-------+
| 1  | Laptop     | 50    |
| 2  | Smartphone | 29    |
| 3  | Tablet     | 20    |
+----+------------+-------+

summary
CDC is used to keep Database 2 updated with respect to Database 1,
enabling the data in Database 2 to be available for analysis and generating valuable insights without affecting the operational OLTP database