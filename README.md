Single-Table Design Pattern:
In a single-table design pattern, you can store related items in the same table to simplify querying and improve performance. For example, let's say you have an e-commerce application that stores customer orders. You could create a single table called "Orders" with the partition key as "OrderID" and the sort key as "CustomerID". This way, you can quickly retrieve all orders for a particular customer or all orders for a specific order ID.

Adjacency List Design Pattern:
In an adjacency list design pattern, you can represent hierarchical relationships between items in a table. For example, let's say you have a social media application that stores data about users and their friends. You could create a single table called "Users" with the partition key as "UserID" and the sort key as "FriendID". Each item in the table would represent a user-friend relationship, with the "UserID" attribute representing the user and the "FriendID" attribute representing the friend.

Composite Keys Design Pattern:
In a composite keys design pattern, you can use a combination of partition and sort keys to efficiently query data based on specific criteria. For example, let's say you have a music streaming application that stores data about songs, artists, and albums. You could create a single table called "Music" with the partition key as "ArtistID" and the sort key as "SongID". This way, you can quickly retrieve all songs by a particular artist or all songs on a specific album.

GSI Overloading Design Pattern:
In a GSI overloading design pattern, you can use a single Global Secondary Index to support multiple query patterns. For example, let's say you have a product catalog application that stores data about products, categories, and brands. You could create a single table called "Products" with the partition key as "ProductID" and the sort key as "CategoryID". You could then create a Global Secondary Index called "BrandCategoryIndex" with the partition key as "BrandID" and the sort key as "CategoryID". This way, you can efficiently query products by brand, category, or both.

Time-Series Data Design Pattern:
In a time-series data design pattern, you can organize data based on a timestamp to enable efficient querying and analysis. For example, let's say you have a weather monitoring application that stores data about temperature and humidity readings. You could create a single table called "WeatherData" with the partition key as "SensorID" and the sort key as "Timestamp". This way, you can quickly retrieve data for a particular sensor or all data for a specific time period.



Single-Table Design Pattern:
Let's say we have a banking system where we want to store all transaction data for customers in a single table called "Transactions". We can use the partition key as "CustomerID" and the sort key as "TransactionID". Here's an example of a query to retrieve all transactions for a specific customer:
css
Copy code
aws dynamodb query \
    --table-name Transactions \
    --key-condition-expression "CustomerID = :customer_id" \
    --expression-attribute-values '{":customer_id": {"S": "123"}}'
Adjacency List Design Pattern:
Let's say we have a hierarchy of bank accounts in our banking system, where each account has a parent account except for the root account. We can use an adjacency list design pattern to represent this hierarchy in a table called "Accounts". Each item in the table would represent an account, with attributes for the account ID, the account type, the balance, and the parent account ID. Here's an example of a query to retrieve the balance of all accounts for a specific customer:
css
Copy code
aws dynamodb query \
    --table-name Accounts \
    --key-condition-expression "CustomerID = :customer_id" \
    --expression-attribute-values '{":customer_id": {"S": "123"}}' \
    --filter-expression "attribute_not_exists(ParentAccountID)"
Composite Keys Design Pattern:
Let's say we want to query transactions based on specific criteria, such as transaction type or amount. We can use a composite keys design pattern to efficiently query data based on these criteria. We can use the partition key as "CustomerID" and the sort key as "TransactionTimestamp#TransactionType#TransactionAmount". Here's an example of a query to retrieve all debit transactions for a specific customer:
css
Copy code
aws dynamodb query \
    --table-name Transactions \
    --key-condition-expression "CustomerID = :customer_id AND begins_with(TransactionID, :debit)" \
    --expression-attribute-values '{":customer_id": {"S": "123"}, ":debit": {"S": "debit"}}'
GSI Overloading Design Pattern:
Let's say we want to efficiently query transactions by different criteria, such as transaction type or amount. We can use a GSI overloading design pattern to support multiple query patterns. We can create a Global Secondary Index called "TransactionTypeAmountIndex" with the partition key as "TransactionType" and the sort key as "TransactionAmount". Here's an example of a query to retrieve all credit transactions greater than $100 for a specific customer:
css
Copy code
aws dynamodb query \
    --table-name Transactions \
    --index-name TransactionTypeAmountIndex \
    --key-condition-expression "TransactionType = :credit AND TransactionAmount > :amount AND CustomerID = :customer_id" \
    --expression-attribute-values '{":credit": {"S": "credit"}, ":amount": {"N": "100"}, ":customer_id": {"S": "123"}}'
Time-Series Data Design Pattern:
Let's say we want to store historical transaction data for analysis and reporting purposes. We can use a time-series data design pattern to store the data in a table called "HistoricalTransactions". We can use the partition key as "CustomerID" and the sort key as "TransactionTimestamp". Here's an example of a query to retrieve all transactions for a specific customer within a certain time range:
sql
Copy code
aws dynamodb query \
    --table-name HistoricalTransactions \
    --key-condition-expression "CustomerID = :customer_id AND TransactionTimestamp BETWEEN :start_time AND :end_time" \
    --
