# SQL

**175.** [**Combining Two Tables**](https://leetcode.com/problems/combine-two-tables/)

**Solution:**

**Left join two tables, select the columns required.**

**Select** FirstName, LastName, City, **State**

**From** Person **LeftJoin** Address

**On** Address.PersonId = Person.PersonId;

**181.** [**Employees Earning More Than Their Managers**](https://leetcode.com/problems/employees-earning-more-than-their-managers/)

**Create test table**

**CREATETABLEIFNOTEXISTS** Employee (

Id INT,

Name VARCHAR(50),

Salary INT,

ManagerId INT

);

**DELETEFROM** Employee;

**INSERTINTO** Employee **VALUES**

(1, 'Joe', 70000, 3),

(2, 'Henry', 80000, 4),

(3, 'Sam', 60000, **NULL**),

(4, 'Max', 90000, **NULL**);

**Solution:**

**A typical problem of self join. Join the table with itself, for each row compare the salaries of employee and manager.**

**Select** a.Name

**From** Employee a **JOIN** Employee b

**ON** a.ManagerId = b.Id

**Where** a.Salary > b.Salary;

**183.** [**Customers Who Never Order**](https://leetcode.com/problems/customers-who-never-order/)

**Create test table**

**Createtable** Customers (

Id INT,

Name VARCHAR(50)

);

**Createtable** Orders (

Id INT,

CustomerId INT

);

**Insertinto** Customers (Id, Name) **Values** (1, 'Joe'), (2, 'Henry'), (3, 'Sam'), (4, 'Max');

**Insertinto** Orders (Id, CustomerId) **Values** (1, 3), (2, 1);

**Solution:**

**Method 1: Use Left Join, then select the rows whose CustomerId is not null.**

**SelectC**.Name **As** Customers

**From** Customers **CleftJoin** Orders O

**OnC**.Id = O.CustomerId

**Where** O.CustomerId **isnull**;

**Method 2: (not sure why the code doesn't pass): Use Join, then select the Name with Not In.**

**Note: Subquery returned more than 1 value. This is not permitted when the subquery follows =, !=, <, <= , >, >= or when the subquery is used as an expression.**

**Select** Name **As** Customers **from** Customers

**Where** Name **NotIn** (

**Select** Name **from** Customers **CJOIN** Orders O

**ONC**.Id = O.CustomerId

);

**184.** [**Department Highest Salary**](https://leetcode.com/problems/department-highest-salary/)

**Create test table**

**Createtable** Employee(

Id INT,

Name varchar(20),

Salary INT,

DepartmentId INT

);

**InsertInto** Employee (Id, Name, Salary, DepartmentId) **Values**

(1, 'Joe', 70000, 1),

(2, 'Henry', 80000, 2),

(3, 'Sam', 60000, 2),

(4, 'Max', 90000, 1);

**Createtable** Department(

Id INT,

Name varchar(20)

);

**InsertInto** Department (Id, Name) **Values**

(1, 'IT'),

(2, 'Sales');

**Solution**

**First select maxmium salary in each department with corresponding Id. Then join the result table with Employee to get the Employee name who has maximum salary, DepartmentId and maximum salary. Then join the result table with the Department table to get the Department Name based on Department id.**

**Select** D.Name **AS** Department, T.Name **AS** Employee, T.Salary **AS** Salary

**From** Department D,

(

**Select** Name, Salary, E.DepartmentId **from** Employee E **JOIN** (

**Select** DepartmentId, **Max**(Salary) **AS** MaxSalary **From** Employee

**Groupby** DepartmentId) tmp

**ON** E.DepartmentId = tmp.DepartmentId

**Where** E.Salary = tmp.MaxSalary

) **AS** T

**Where** D.Id = T.DepartmentId;

**2. Rank**

**176.** [**Second Highest Salary**](https://leetcode.com/problems/second-highest-salary/)

**Create test table**

**Createtable** Employee(

Id INT,

Salary INT

);

**Insertinto** Employee (Id, Salary) **Values**

(1, 100),

(2, 200),

(3, 300),

(4, 200);

**Solution:**

**Method 1: Since the second highest salary is the highest salary after maximum salary is removed, we add a whereclause to inquery it, within which there is a subquery to select the highest salary.**

**SelectMax**(Salary) **AS** SecondHighestSalary

**From** Employee

**Where** Salary < (**SelectMax**(Salary) **From** Employee);

**Method 2: A more general method is to generate the rank of each row according to salary, then select the row whose value in rank column is 2. The generalized ranking questions are showed later.**

**177.** [**Nth Highest Salary**](https://leetcode.com/problems/nth-highest-salary/)

**Create test table**

**The test table is the same as question 176.**

**Solution:**

**Method 1: The key is to generate the rank of rows. In MySQL, we can set a variable to help do that (which has the same output as dense\_rank()). Note there could be rows with the same rank, so we need select the distinct salary.**

**SelectDistinct** Salary **From** (

**Select** Id, Salary,

@rank := **if**(@preSalary = Salary, @rank, @rank+1) **AS** Rank,

@preSalary := Salary **AS** preSalary

**From** Employee, (**Select** @rank:=0, @preSalary:=-1) var

**Orderby** Salary **DESC**

) tmp

**Where** Rank=N;

**Method 2: In SQL server and Oracle, we can use rank() and dense\_rank() function to generate rank directly. Note if there are ties, dense\_rank() always returns consecutive integers, while rank() returns discrete ones. For the difference between these two function, see** [**here**](http://stackoverflow.com/questions/11183572/whats-the-difference-between-rank-and-dense-rank-functions-in-oracle)**.**

**Select** Id, Salary, Rank() Over (**Orderby** Salary **Desc**) **From** Employee;

**Select** Id, Salary, Dense\_Rank() Over (**Orderby** Salary **Desc**) **From** Employee;

**178.** [**Rank Scores**](https://leetcode.com/problems/rank-scores/)

**Create test table**

Create table Scores(

Id INT,

Score Float

);

Insert Into Scores (Id, Score) Values

(1, 3.5),

(2, 3.65),

(3, 4.0),

(4, 3.85),

(5, 4.0),

(6, 3.65);

**Solution:**

**The Solution is the same as question 177. First generate the rank, then select two columns Score and Rank.**

**Select** Score, Rank **From** (

**Select** Id, Score,

@rank := **if**(@preScore = Score, @rank, @rank+1) **AS** Rank,

@preScore := Score **AS** preScore

**From** Scores, (**Select** @rank:=0, @preScore:=-1) var

**Orderby** Score **DESC**

) tmp;

**180.** [**Consecutive Numbers**](https://leetcode.com/problems/consecutive-numbers/)

**Create test table**

**Createtable** Logs(

Id INT,

Num INT

);

**Insertinto** Logs (Id, Num) **Values**

(1,1),

(2,1),

(3,1),

(4,2),

(5,1),

(6,2),

(7,2);

**Solution:**

**This is similar to ranking question, in which we set a variable to count consecutive numbers. We create a new column to record the number of consecutive numbers. If the Num is the same as the previous one, then add the count by one, and reset the count to be 1 if the Num is different.**

**Note: since it doesn't need rank, in SQL server and Oracle you can't use rank() to solve this problem.**

**SelectDistinct** Num **As** ConsecutiveNums **From** (

**Select** Id, Num,

@**count** := **If**(@preNum = Num, @**count**+1, 1) **ASCount**,

@preNum := Num **AS** preNum

**From** Logs, (**Select** @preNum:=**NULL**, @**count**:=1) var

) tmp

**WhereCount**>=3;

**185.** [**Department Top Three Salaries**](https://leetcode.com/problems/department-top-three-salaries/)

**Create test table**

**Createtable** Employee(

Id INT,

Name varchar(20),

Salary INT,

DepartmentId INT

);

**InsertInto** Employee (Id, Name, Salary, DepartmentId) **Values**

(1, 'Joe', 70000, 1),

(2, 'Henry', 80000, 2),

(3, 'Sam', 60000, 2),

(4, 'Max', 90000, 1),

(5, 'Janet', 69000, 1),

(6, 'Randy', 85000, 1),

(7, 'Xiang', 70000, 1);

**Createtable** Department(

Id INT,

Name varchar(20)

);

**InsertInto** Department (Id, Name) **Values**

(1, 'IT'),

(2, 'Sales');

**Solution:**

**This is also a ranking problem. But different from previous ones, we need generate the rank for each group of each department.**

**Method 1: In MySQL, we set a variable to generate the rank within each group. If the group(departmentId) changed, then set the rank to default. Then join the generated table with Department table to get the department name.**

**Select** D.Name **AS** Department, T.Name **AS** Employee, T.Salary

**From**

(

**Select** DepartmentId, Name, Salary,

(**CASEWHEN** @id=DepartmentId **THEN** @rank:= **IF**(@preSalary = Salary, @rank, @rank+1)

**ELSE** @rank:= 1

**END**) **AS** Rank,

@preSalary:= Salary **AS** preSalary,

@id:= DepartmentId **AS** preId

**From** Employee, (**Select** @rank:=0, @preSalary:=-1, @id:=**NULL**) var

**Orderby** DepartmentId, Salary **DESC**

) T **JOIN** Department D

**ON** T.DepartmentId = D.Id

**Where** T.Rank <=3;

**Method 2: Similarly, in SQL server and Oracle, we can use Dense\_rank() function to generate the rank easily. Note according to the question, the tie should have the same rank. So Dense\_rank() is used here.**

**Select** D.Name **AS** Department, T.Name **AS** Employee, T.Salary

**From**

(

**Select** DepartmentId, Name, Salary,

Dense\_rank() Over (partition **by** DepartmentId **Orderby** Salary **DESC**) **AS** Rank

**From** Employee

) T **JOIN** Department D

**ON** T.DepartmentId = D.Id

**Where** T.Rank <=3;

**197.** [**Rising Temperature**](https://leetcode.com/problems/rising-temperature/?tab=Description)

**Create test table**

**CREATETABLE** Weather (

Id INT,

Date DATE,

Temperature INT

);

**DELETEFROM** Weather;

**INSERTINTO** Weather **VALUES**

(1, '2015-01-01', 10),

(2, '2015-01-02', 25),

(3, '2015-01-04', 20),

(5, '2015-01-03', 32),

(4, '2015-01-06', 30);

**Solution:**

**Method 1: Set dummies variables, compare each day's temperature and date with its previous day. Then select the day in which temperature is rising and date is continuous. (Don't know why this method is very slow, almost exceeded time limit).**

**Select** Id **From** (

**Select** Id, Date, Temperature,

@Higher := **If**(Temperature > @preTemp, 'Yes', 'No') **AS** Higher,

@DateContinuous := **If**(Datediff(Date, @preDate) = 1, 'Yes', 'No') **AS** DateContinuous,

@preTemp := Temperature **AS** preTemp,

@preDate := Date

**From** Weather, (**Select** @preTemp:=**NULL**, @Higher:=**NULL**, @preDate:=**NULL**) var

**Orderby** Date

) tmp

**Where** Higher = 'Yes' **AND** DateContinuous = 'Yes';

**Method 2: Join the table with itself on where the data difference is one. Then select the dates in which temperature is rising.**

**Select** W1.Id

**From** Weather W1 **JOIN** Weather W2

**ON** to\_days(W1.Date) = to\_days(W2.Date) + 1

**Where** W1.Temperature > W2.Temperature;

**3. SQL Basics**

**182.** [**Duplicate Emails**](https://leetcode.com/problems/duplicate-emails/)

**Create test table**

**Createtable** Person (

Id INT,

Email VARCHAR(100)

);

**Insertinto** Person (Id, Email) **Values**

(1, 'a@b.com'),

(2, 'c@d.com'),

(3, 'a@b.com')

**Solution:**

**Group by Email, then filter the groups whose count are greater than 1.**

**Select** Email **From** Person

**Groupby** Email

**Havingcount**(\*) > 1;

**196.** [**Delete Duplicate Emails**](https://leetcode.com/problems/delete-duplicate-emails/)

**Create test table**

**Createtable** Person (

Id INT,

Email VARCHAR(100)

);

**Insertinto** Person (Id, Email) **Values**

(1, 'john@example.com'),

(2, 'bob@example.com'),

(3, 'john@example.com')

**Solution:**

**It's very easy to retain the unique emails with the smallest ids. However, note the question require to delete the duplicate emails, so Delete clause is needed.**

/\* retain the unique emails\*/

**SelectMin**(Id) **AS** minId, Email **From** Person

**Groupby** Email

/\* delete the emails which don't appear in the unique emails\*/

**DeleteFrom** Person

**Where** Id **notin** (

**Select** minId **From** (

**Selectmin**(Id) **AS** minId, Email

**From** Person

**Groupby** Email

) **AS** tmp

);

**262.** [**Trips and Users**](https://leetcode.com/problems/trips-and-users/)

**Create test table**

**Createtable** Trips(

Id INT,

Client\_id INT,

Driver\_id INT,

City\_id INT,

Status ENUM('completed', 'cancelled\_by\_driver', 'cancelled\_by\_client'),

Request\_at DATE

);

**Insertinto** Trips **Values**

(1,1,10,1,'completed', str\_to\_date('10/1/13', '%m/%d/%Y')),

(2,2,11,1,'cancelled\_by\_driver', str\_to\_date('10/1/13', '%m/%d/%Y')),

(3,3,12,6,'completed', str\_to\_date('10/1/13', '%m/%d/%Y')),

(4,4,13,6,'cancelled\_by\_driver', str\_to\_date('10/1/13', '%m/%d/%Y')),

(5,1,10,1,'completed', str\_to\_date('10/2/13', '%m/%d/%Y')),

(6,2,11,6,'completed', str\_to\_date('10/2/13', '%m/%d/%Y')),

(7,3,12,6,'completed', str\_to\_date('10/2/13', '%m/%d/%Y')),

(8,2,12,12,'completed', str\_to\_date('10/3/13', '%m/%d/%Y')),

(9,3,10,12,'completed', str\_to\_date('10/3/13', '%m/%d/%Y')),

(10,4,13,12,'cancelled\_by\_driver', str\_to\_date('10/3/13', '%m/%d/%Y'));

**Createtable** Users(

Users\_id INT,

Banned ENUM('No', 'Yes'),

**Role** ENUM('client', 'driver', 'partner')

);

**Insertinto** Users **Values**

(1, 'No', 'client'),

(2, 'Yes', 'client'),

(3, 'No', 'client'),

(4, 'No', 'client'),

(10, 'No', 'driver'),

(11, 'No', 'driver'),

(12, 'No', 'driver'),

(13, 'No', 'driver');

**Solution:**

**The key of this question is computing the cancellation rate. To compute it, we need count the number of trips cancelled by driver, and total number of trips within each group. The group is regularized by the date. Before the group by clause, use where clause to filter the rows which meet the requirement.**

**Select** T.Request\_at **ASDay**, Round(**Sum**(**If**(Status='cancelled\_by\_driver',1,0))/**count**(\*), 2) **AS** 'Cancellation Rate'

**From** Trips T **JOIN** Users U

**ON** T.Client\_Id = U.Users\_Id

**Where** Banned='No' **AndRole**='client' **And** Request\_at **Between** '2013-10-01' **and** '2013-10-03'

**Groupby** Request\_at

**Orderby** Request\_at;
