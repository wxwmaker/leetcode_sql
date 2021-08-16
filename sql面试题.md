### [组合两个表](https://leetcode-cn.com/problems/combine-two-tables)

表 1: Person

+-------------+---------+  
| 列名 | 类型 |  
+-------------+---------+  
| PersonId | int |  
| FirstName | varchar |  
| LastName | varchar |  
+-------------+---------+  
PersonId 是上表主键  
表 2: Address

+-------------+---------+  
| 列名 | 类型 |  
+-------------+---------+  
| AddressId | int |  
| PersonId | int |  
| City | varchar |  
| State | varchar |  
+-------------+---------+  
AddressId 是上表主键

编写一个 SQL 查询，满足条件：无论 person 是否有地址信息，都需要基于上述两表提供 person 的以下信息：

*   解:
    
    ```
    # Write your MySQL query statement below
    
    
    # select FirstName, LastName, City, State
    # from 
    # (
    # (select 
    # PersonId,FirstName,LastName 
    # from 
    # Person)a  left join 
    # (select
    # PersonId,City, State
    # from 
    # Address)b
    # on a.PersonId=b.PersonId
    # );
    
    select FirstName, LastName, City, State
    from Person left join Address
    on Person.PersonId = Address.PersonId
    ;
    
    ```
    

### [第二高的薪水](https://leetcode-cn.com/problems/second-highest-salary)

编写一个 SQL 查询，获取 Employee 表中第二高的薪水（Salary） 。

+----+--------+  
| Id | Salary |  
+----+--------+  
| 1 | 100 |  
| 2 | 200 |  
| 3 | 300 |  
+----+--------+  
例如上述 Employee 表，SQL 查询应该返回 200 作为第二高的薪水。如果不存在第二高的薪水，那么查询应返回 null。

+---------------------+  
| SecondHighestSalary |  
+---------------------+  
| 200 |  
+---------------------+

*   解:
    
    ```
    # Write your MySQL query statement below
    
    # SELECT
    #     IFNULL(
    #       (SELECT DISTINCT Salary
    #        FROM Employee
    #        ORDER BY Salary DESC
    #         LIMIT 1 OFFSET 1),
    #     NULL) as SecondHighestSalary
    
    select 
    (
        select DISTINCT Salary as SecondHighestSalary 
        from Employee 
        order by Salary desc
        limit 1 offset 1
    )as SecondHighestSalary
    
    ```
    

### [第 N 高的薪水](https://leetcode-cn.com/problems/nth-highest-salary)

编写一个 SQL 查询，获取 Employee 表中第 n 高的薪水（Salary）。

+----+--------+  
| Id | Salary |  
+----+--------+  
| 1 | 100 |  
| 2 | 200 |  
| 3 | 300 |  
+----+--------+  
例如上述 Employee 表，n = 2 时，应返回第二高的薪水 200。如果不存在第 n 高的薪水，那么查询应返回 null。

+------------------------+  
| getNthHighestSalary(2) |  
+------------------------+  
| 200 |  
+------------------------+

*   解:
    
    ```
    CREATE FUNCTION getNthHighestSalary ( N INT ) RETURNS INT BEGIN
    
    DECLARE m INT;
    
    SET m = N - 1;
    
    RETURN ( # Write your MySQL query statement below.
    
    SELECT ifnull( ( SELECT DISTINCT salary FROM Employee ORDER BY salary DESC LIMIT 1 offset m ), NULL ) );
    
    END
    
    ```
    

### [分数排名](https://leetcode-cn.com/problems/rank-scores)

编写一个 SQL 查询来实现分数排名。

如果两个分数相同，则两个分数排名（Rank）相同。请注意，平分后的下一个名次应该是下一个连续的整数值。换句话说，名次之间不应该有 “间隔”。

+----+-------+  
| Id | Score |  
+----+-------+  
| 1 | 3.50 |  
| 2 | 3.65 |  
| 3 | 4.00 |  
| 4 | 3.85 |  
| 5 | 4.00 |  
| 6 | 3.65 |  
+----+-------+  
例如，根据上述给定的 Scores 表，你的查询应该返回（按分数从高到低排列）：

+-------+------+  
| Score | Rank |  
+-------+------+  
| 4.00 | 1 |  
| 4.00 | 1 |  
| 3.85 | 2 |  
| 3.65 | 3 |  
| 3.65 | 3 |  
| 3.50 | 4 |  
+-------+------+

*   解:
    
    ```
    /*
    窗口函数的区别
    rank()
    排名为相同时记为同一个排名, 并且参与总排序
    dense_rank() over (PARTITION BY xx ORDER BY xx [DESC])
    排名相同时记为同一个排名, 并且不参与总排序
    row_number() over (over (PARTITION BY xx ORDER BY xx [DESC]))
    排名相同时记为下一个排名
    窗口函数在hive sql中经常使用, 也可在 mysql 8.0 之上的版本中使用
    */
    
    select score as "Score",dense_rank() over (order by score desc) as "Rank" from scores;
    
    ```
    
