---
layout: post
title:  SQL Database performance optimization
author: Benjamin Dahl
date:   2017-10-05 12:03:10 -0700

images:

  - url: assets/img/blog/sql.jpg
    alt: SQL Database performance optimization
    caption: SQL Database performance optimization

---
# SQL Database performance optimization
### Benjamin Dahl
### Liberty University: 201630 Summer 2016 CSIS 325-D01

#### Abstract

As more of our information is stored online in massive databases, it is increasingly important that access to this information remains fast. As databases grow larger they can also grow slower. This paper seeks to explore techniques used in database optimization and how to speed up slow or poor performing database queries. Some of the techniques explored include hardware considerations, use of indexes, temporary tables, compression, and normalization.

#### Introduction

One of the most important responsibilities of a Database Administrator is to ensure that a database is performing optimally and does not suffer from performance issues. As databases grow larger in size it is likely that it will begin to suffer from slow queries and an overall performance decrease (Mor, Kashyap, & Rathy, 2012). This can become a problem as database queries slow down they affect users work and productivity. Even small databases can suffer from performance issues if queries are written poorly. This paper seeks to explore some of the techniques used by SQL database administrators to optimize database query performance and keep information moving in and out of a database quickly.

#### Hardware considerations
	
One of the first things to consider when tackling a poor performing database is whether or not the server or computer that is running your database is up for the challenge. Large databases can consume huge amounts of memory and disk space. Typically, these large databases would be running on large servers in dedicated data centers with lots of memory and storage rather than a traditional personal computer. Even in these data center situations, it is important for a DBA to monitor memory and disk usage to spot potential problems. In some situations, where database performance is suffering, it may be worth the cost and investment to upgrade the amount of server memory or even to upgrade to a new server. Servers can be very expensive so it is important to do a cost benefit analysis before buying so that you don’t end up buying more server than you actually need. Today, many applications run on cloud servers and it is very easy to simply order a larger server instance from the cloud provider when needed. These cloud servers can be made available in seconds. If your application depends on fast database queries it may be worth the extra expense of the faster cloud server. The only exception may be during the database development phase. During this phase it may be sufficient to simply work on the developer’s local computer. However, once the database is ready for production it should be moved to a production class server.

#### Use indexes

One of the very first things that a DBA should consider when analyzing a slow performing query is whether or not to use an index. An index is a data structure that improves the speed of data retrieval by utilizing a unique and sortable key that is stored in a type of binary tree data structure (Böhm, Berchtold, & Keim, 2001). Primary keys make good indexes because they are already unique and are often sortable (Apte, Ingle, & A.k., 2013). Using an index can drastically improve read times at the cost of additional overhead when writing data to the table. The binary tree data structure works by sorting data into connected nodes. These nodes form a structure that looks like a tree where values connected to the left are smaller than the parent node, and values connected to the right are larger than the parent node. When a query containing an index is processed, a search algorithm climbs the tree looking for a match to the query. Since the tree structure has already been sorted, a search algorithm can find the matching index much faster than traditional methods. Select queries on data with indexes will see the most improvement when used in a where clause. Any time that you are filtering data with a where clause, try to use indexes if possible. 
#### Avoid loops and nested loops

A loop is a programming tool that allows the programmer to run a block of code more than once. Generally, a programmer will insert a block of code inside of a loop and then tell the computer how many times to run the loop, or give it a condition as to when to stop. Loops are incredibly useful tools, but when it comes to database design they can easily be misused. Nested looped, where one loop runs inside of another loop, can be especially slow (Lieuwen, & Dewitt, 1992). When optimizing a database query try to avoid running queries inside of loops. Instead try accomplish the job using structured queries or joins (CORLĂŢAN, LAZĂR, LUCA, & PETRICICĂ, 2014). Often times new programmers will mistakenly use a nested loop where they could accomplish the same thing by using a join. Joins are already optimized by the database management system to perform queries as fast as possible. For example, one specific advantage that a join has over a nested loop, is that a join can make more efficient use of indexes.

#### Avoid correlated subqueries

One of the most powerful tools in SQL is correlated subqueries or nested queries. A correlated subquery is when you have a query nested within a query (Cao, & Badia, 2007). These nested queries allow a developer to run a query based on data fetched from the outer query. While correlated subqueries can be very useful it is important to avoid them if possible when attempting to optimize a slow performing database. Nested subqueries are similar to nested loops, in that the inner query is run for every row in the outer query. These nested subqueries can become unbearably slow when operating on very large databases. Often times correlated subqueries can be rewritten more efficiently as a join (Cao, et al., 2007).

#### Avoid SELECT *

SELECT * is one of the most common selector queries. The asterisk or star is shorthand for selecting all columns of a table. This works fine when your tables are rather small, but can quickly take up unnecessary time and resources on larger tables. Consider selecting only the columns that you actually need, rather than selecting everything. This will keep your memory usage low and your returned SQL objects smaller as well. This way both the database and the application querying the database have less data to process. 

#### Exists vs. count\(\*\)

One mistake that database developers often mistake is using the function count(*) to search a database to see if a record exists (Kelly, n.d.). This is a costly query because it searches every record of the table even after it has found a matching record. A better solution would be to use the exists function. The exists function searches the database for a matching record and when it finds one it stops. This can save you the time of querying through every single record in the table. Only use count(*) when you really want to count every instance of a condition in a table. 

#### Use of temporary tables

Another powerful tool in SQL is the use of temporary tables. A temporary table is a table that is created during a query and is destroyed at the end of the same query. Database management systems usually set aside memory to handle temporary tables. You can use temporary tables in situations where you would normally join two very large tables on a set of conditions. By first querying the largest database into a temporary table you can reduce the time it takes to join the temporary table with the second table (Hellerstein, 1998). This is especially useful when working with very large databases that would otherwise have to iterate through thousands or millions of rows costing precious time. Temporary tables can also be used in place of subqueries with the advantage that you can also create an index inside the temporary table (CORLĂŢAN, et al., 2014). Although creating the temporary table and then creating the index of the temporary table is a costly process, you may find that you actually save time once you’ve queried the indexed temporary table. 

#### Compression

Compression is the process of storing data into smaller bits. Decompression is the process which recovers the original information from the compressed data. Compression algorithms have been shown to improve query performance, especially through input/output costs (CORLĂŢAN, et al., 2014). If your database supports it, consider turning on compression algorithms. Combining compression with database indexes can speed up queries and keep your memory and disk space requirements low. This is especially important for databases that store multimedia such as images and videos (Böhm, Berchtold, & Keim, 2001).

#### Caching

Caching is a feature of most database management systems that allows storing of common queries into a disk cache (Kumar, 2012). This is an excellent strategy especially for queries where the result is always the same or doesn’t change often. You will often find database query caching on websites that are driven by content management systems that employ a database. Most websites on the internet are not updated very often and so information stored in the websites database such as the home page is the perfect candidate for caching. When caching is turned on, all queries are saved to disk, so that the next time the database receives the same query, it only needs to get the result saved in the cache, rather than perform the entire query on the database again. Some caching strategies can also save queries into memory or RAM which is even faster than a disk cache. Using caching with compression can yield further results. 

#### Normalization

No paper on database optimization techniques would be complete without discussing normalization. Normalization is the process of reducing redundancies in a database (Mannino, 2015). Normalization is a process that goes through a number of steps called normal forms. The first normal form is known as 1NF and the second is 2NF. There are six total normal forms including 1NF, 2NF, 3NF/BCNF, 4NF, 5NF, and DKNF (Mannino, 2015). The first normal form prohibits nesting tables. In relational databases it is important to separate data into separate entities. 1NF enforces that. The next normal forms 2NF and 3NF deals with key and nonkey columns. “A table is 2NF if each nonkey column depends on whole candidate keys, not on a subset of any candidate key” (Mannino, 2015). Basically, this further enforces the separation of entities. 3NF furthers the 2NF rule by enforcing that all nonkey columns depend only on candidate keys (Mannino, 2015). These first 3 normal forms will vastly improve the performance of your database. Following 3NF is the Boyce-Codd Normal Form (BCNF). The BCNF simply states that a table is in BCNF if every determinant is a candidate key (Mannino, 2015). This is nearly the same as 3NF although it is simpler and covers an additional case. 4NF, 5NF, and DKNF have to do with m-way relationships. An m-way relationship is a many to many relationship between entities. 4NF states that a table is 4Nf if it does not contain any multivalue dependencies (Mannino, 2015). This is similar to the 1NF rule in that it prevents redundencies among multivalued dependencies. The remaining 5NF and DKNF forms are generally not considered practical and are more conceptual in nature and so we won’t discuss them here (Mannino, 2015). The bottom line is that it is important to consider normalization in order to reduce redundancies in your table structure and thus improve performance. Although normalization is a process usually undertaken during the design phase, it may be important to revisit normalization as your database grows and changes. You may discover issues that normal forms can solve and improve performance. 

#### Conclusion

As we continue into the information age gathering up massive sums of information, it is important that we continue to make that information useful. That information is only as useful as fast as we can access it. That is why large internet corporations such as Google, Microsoft, and Amazon continue to invest in large database solutions. They help innovative companies and developers create new applications that gather and analyze large amounts of data. In this paper you should have learned several tools that can help you optimize and improve the performance of your information databases. This paper has discussed server hardware considerations, the use of indexes, loops, correlated subqueries, SELECT *, exists vs count(*), temporary tables, caching, compression, and normalization. By combining these techniques with careful analysis you can keep your databases working smoothly and quickly and will keep your users happy and productive. 

#### References

Apte, T., Ingle, M., & A.k., G. (2013). Column-Store Databases: Approaches and Optimization Techniques. IJDKP International Journal of Data Mining & Knowledge Management Process, 3(5), 83-89. doi:10.5121/ijdkp.2013.3508 

Böhm, C., Berchtold, S., & Keim, D. A. (2001). Searching in high-dimensional spaces: Index structures for improving the performance of multimedia databases. CSUR ACM Comput. Surv. ACM Computing Surveys, 33(3), 322-373. doi:10.1145/502807.502809 

Cao, B., & Badia, A. (2007). SQL query optimization through nested relational algebra. ACM Transactions on Database Systems TODS ACM Trans. Database Syst., 32(3). doi:10.1145/1272743.1272748 

CORLĂŢAN, C., LAZĂR, M., LUCA, V., & PETRICICĂ, O. (2014). Query Optimization Techniques in Microsoft SQL Server. Database Systems Journal, 5(2), 33-48. 

Hellerstein, J. M. (1998). Optimization techniques for queries with expensive methods. ACM Transactions on Database Systems TODS ACM Trans. Database Syst., 23(2), 113-157. doi:10.1145/292481.277627 

Kelly, A. (n.d.). Exists Vs. Count(*) - The battle never ends... Retrieved August 15, 2016, from http://sqlblog.com/blogs/andrew_kelly/archive/2007/12/15/exists-vs-count-the-battle-never-ends.aspx 

Kumar, P. (2012, January 1). Semantic based Efficient Cache Mechanism for Database Query Optimization. International Journal of Computer Applications, 43(23). Retrieved August 15, 2016. 

Lieuwen, D. F., & Dewitt, D. J. (1992). A transformation-based approach to optimizing loops in database programming languages. ACM SIGMOD Record SIGMOD Rec., 21(2), 91-100. doi:10.1145/141484.130301 

Mannino, M. V. (2015). Database design, application development, and administration (6th ed.). Boston: McGraw-Hill Irwin. 
Mor, J., Kashyap, I., & Rathy, R. K. (2012). Analysis of Query Optimization Techniques in Databases. International Journal of Computer Applications IJCA, 47(15), 6-12. doi:10.5120/7262-0127 
