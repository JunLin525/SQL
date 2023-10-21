SQL 2023

===


### Insert
```
insert into employees
Values(2,"crg","Syu",50,"1994-08-08"),
	  (3,"hip","hop",60,"1993-05-25");

select * from employees;
```

![](https://hackmd.io/_uploads/Sy6HA_Jza.png)


不想全部格子都insert可以用下列方式，一定要寫範圍，不然若是不寫有空，就會error。
```
insert into employees (employee_id, first_name,last_name,hourly_pay)
Values(4,"Brg","Lyu",40);

select * from employees;
```
![](https://hackmd.io/_uploads/ryLyJtyzT.png)



### Select
![](https://hackmd.io/_uploads/S1q1BFkzT.png)



一些限制可以使用where。
```
select first_name, last_name
from employees
where hourly_pay>=30;
```

![](https://hackmd.io/_uploads/B1ifrtJza.png)



找到NULL的地方
![](https://hackmd.io/_uploads/By23wYyG6.png)

```
select *
from employees
where hire_date is Null
;

```

### Update
```
Update employees
Set hourly_pay=10.58,
hire_date = "1997-08-30"
where employee_id =4;
select * from employees;
```
![](https://hackmd.io/_uploads/By2utF1f6.png)

### - 若不加上where的話就會出大事 所有會一起變化 但若是想改變同一行所有的數據，可以這樣做沒錯
```
Update employees
Set hourly_pay=10.25;
select * from employees;
```
![](https://hackmd.io/_uploads/SJ-k9tkMp.png)

### Delete
Delete也是一樣，若是刪除之前確定寫上了where，不然整個表都會消失。

```
Delete from employees
where employee_id = 4;
select * from employees;
```
![](https://hackmd.io/_uploads/rJk3qK1z6.png)
4就確實被刪除了。



### Commit
```
SETautocommit = off寫出之後
```

commit可以定期存檔，你做錯東西的時候可以rollback。


### Current time
```
insert into test 
value(current_date(), current_time(), NOW());

select * from test;
```
![](https://hackmd.io/_uploads/B12CSqkMT.png)


想要寫昨天的話，可以用-1


```
insert into test 
value(current_date()-1, current_time(), NOW());

select * from test;
```
![](https://hackmd.io/_uploads/HkNZL5JG6.png)

想要拿掉整個Table
```
Drop table test;
```


### Unique

創建一個新的table
```
Create table products(  
product_id INT,     
product_name Varchar(25) unique,     
price decimal(4, 2) 
);
```

若想把其中一個欄位變成Unique(代表只能有一個名字，不重複)
```
alter table products
add constraint
unique(product_name);
```

接下來加入資料
```
insert into products 
values (100,"hamburger",3.99),    (101,"frice",1.89),    
(102,"soda",0.8),       
(103, "ice cream", 1.49)
;
```


### Not Null
```
alter table products 
modify price decimal(4,2) not null;
```

可以設定某個格子不能變成NULL


```
insert into products
values(104, "cookies" , Null);
select * from products
```
![](https://hackmd.io/_uploads/BJ3kK9JGp.png)

設定過後你若想寫NULL就會出現Error


### check 
另外一個限制可以確定數值的設定。

比如我想建立一個表有最低薪資的限制可以這樣做。

```
create table employees(
	employee_id  int,
    first_name varchar(50),
    last_name varchar(50),
    hourly_pay decimal (5,2),
    hire_date Date,
    constraint chk_hourly_pay check (hourly_pay >= 10.00)
);
```

確保每小時的工資都會高於十元。


```
alter table employees 
add constraint
check_hourly_pay check(hourly_pay>10.00);
```
更改本來有的數據可以這樣做。

若我要把數據加入的話。
```
Insert into employees
value(6, "sheldon", "princeton", 5.00, "2023-10-22");
```

因為是五元薪資，就會出現錯誤。我Violate constraint。

```
alter table employees
drop check check_hourly_pay;
```
若要移除特定的hourly pay資訊限制，打上訴即可。
找到那個限制的名字，然後刪除。

### default

若一個格子沒有設定數值，我自動把default的數值給進去。
新建表單如下
```
Create Table products(
    product_id INT,
    product_name VARCHAR(25),
    price DECIMAL(4,2) DEFAULT=0
    );
```
若更改已經有的表單，可以如下作業:
```
alter table products
alter price set default 0;
```


此時在輸入你要建立的資料
```
insert into products (product_id, product_name)
values (104,"straw"),
	   (105,"napkin"),
       (106,"fork"),
       (107,"spoon");
select * from products;
```
結果出現如下
![](https://hackmd.io/_uploads/SkiFJjJGp.png)


### Primary Key

若想建立一個表單
```
create table transactions(
	transaction_id int primary key,
    amount decimal (5,2)
);

select * from transactions;
```
若想變表單內部變成Primary_key可以這樣做

```
alter table transactions 
add constraint 
primary key (transaction_id);
```

但一個table只能有一個primary key ，若超過一個也會出現error。

Primay key不能重複，也不能是Null。
![](https://hackmd.io/_uploads/HJDlXjkGa.png)

![](https://hackmd.io/_uploads/ryj-mo1Gp.png)


### increment

```
create table transactions( 
transaction_id int primary key 
auto_increment,     
amount decimal(5,2) 
);
```
這樣可以建立一個自動增加的primary key，新增的時候就不用手動設定

若之前的已經設定要刪除重新弄，可以這樣做。
```
Delete from transactions;
select * from transactions;
```

若想從二千開始，可以這樣設定
```
Alter table transactions
Auto_Increment = 1000;
```

這樣下來就會跟著二千來跟著跑動設定值
```
insert into transactions (amount)
values( 3.96),(9.55),(4.88);
select * from transactions;
```
![](https://hackmd.io/_uploads/HJwg5skzT.png)




### Foreinkey Key

```
create table transactions(
transaction_id INT primary Key auto_increment,
amount Decimal(5,2),
customer_id INT,
foreign key(customer_id) references customers(customer_id)
);

select * from transactions;
```

這樣可以增加一個foreignkey是在customer這個表上，然後數值是Customer_ID作為Foreign key。
foreign key(customer_id) references customers(customer_id)
我這個表的foreign key是Customer_id，然後是參考Customer表Customer_id的這個值。

想要移除也非常簡單
```
Alter table transactions
drop foreignkey transactions_ibfk_1;

```

若想要自己命名Foreign key的名稱不要給系統抓，可以這樣做
```
alter table transactions 
add constraint fk_customer_id
foreign key(customer_id) references customers(customer_id);
```
這樣fk_customer_id這個foreignkey就會出現
![](https://hackmd.io/_uploads/S1jNTjkzp.png)


若想刪除一個表格，但其中的資料是另外一個資料的Foreignkey，就會出現錯誤。


Left Join 
![](https://hackmd.io/_uploads/Skm0p61M6.png)

Right Join 
![](https://hackmd.io/_uploads/BJBeCpkza.png)


### Function
整個SQL中有很多Function，只列出最基本的在下方。

- Count
```
select count(amount)
from transactions;
```
總共有多少ROW可以被列出，然後也可以用where來限制條件。
- MAX
```
select max(amount)
from transactions;

```
- Min
```
select min(amount)
from transactions;

```

- AVG 平均值
![](https://hackmd.io/_uploads/rJPTlCyMp.png)

```
select AVG(amount) as Average
from transactions;

```
AS 後面可以改變圖表名稱

- SUM 總和
```
select SUM(amount) as sum
from transactions;

```

![](https://hackmd.io/_uploads/SkvmZCkf6.png)

- concat 結合
```
select concat(first_name,"__", last_name)
from employees;
```


- 運算邏輯 and or not 
```
alter table employees
add column job varchar(25) after hourly_pay;
select * from employees;

```
![](https://hackmd.io/_uploads/Sku3ZAyzT.png)

在hourly之後插入job格子可以這樣寫。
```
alter table employees 
add column job varchar(25) after hourly_pay;
```
![](https://hackmd.io/_uploads/ByTwX0yM6.png)


若我要搜查相關的資料，在1994年5月25日前近來工作且是廚師的人，用AND把兩個資訊一起連接起來查詢。
```
select * 
from employees 
where hire_date < "1994-05-25" and job = "cook";
```
![](https://hackmd.io/_uploads/H1_O8Rkz6.png)

若我想找的是非經理的一般員工
我會這樣做
```
select * 
from employees 
where not job ="manager";
```
![](https://hackmd.io/_uploads/SJaHvRkz6.png)

若是要不是經理又不是看門人的
```
select * 
from employees 
where not job ="manager" and not job = "janitor";
```
![](https://hackmd.io/_uploads/HyXsvR1M6.png)


- between
```
select * 
from employees 
where hire_date between "1994-05-05" And "1997-08-08";

```
![](https://hackmd.io/_uploads/rklWcd0yf6.png)

- in 
字串沒有大小跟區間，就用in，有這三者的就秀出。
```
select * 
from employees 
where job In ('cook','cashier','janitor');

```
![](https://hackmd.io/_uploads/rJxJt0JGa.png)

### wild cards
主要指的是_跟%的使用

所有firstname 開頭是s的。且=要改成like
```
select * 
from employees 
where first_name Like "s%";

```

![](https://hackmd.io/_uploads/HJxMqrgfp.png)

```
select * 
from employees 
where hire_date like "19%";

```
![](https://hackmd.io/_uploads/H1rU9BeGa.png)


找到用g結尾的first name
```
select * 
from employees 
where first_name like "%g";

```
![](https://hackmd.io/_uploads/r1Xs9HeGp.png)


而_則是可以填入任何內容的空白
```
select * 
from employees 
where job like "_ook";

```

![](https://hackmd.io/_uploads/rkFA5BlG6.png)


找到所有八月入職的
```
select * 
from employees 
where hire_date like "____-08-__";

```
![](https://hackmd.io/_uploads/ryJSireMp.png)


第二個字是a的職位
```
select * 
from employees 
where job like "_a%";

```
![](https://hackmd.io/_uploads/H1gG3rgGa.png)


### order by
```
select * 
from employees 
order by last_name desc;

```

desc主要是反向排列
![](https://hackmd.io/_uploads/HJOO3BeM6.png)

asc是順向，一般order by沒有打字都是順向
![](https://hackmd.io/_uploads/Hk4thHxGp.png)


日期排列可以這樣做
```
select * 
from employees 
order by hire_date;

```
![](https://hackmd.io/_uploads/H1Pa3rlfT.png)

但若同一個排法有太多相同的排在一起怎辦?可以設定兩個項目來比較，這樣就可以明確的排列的先後了。
```
select * 
from transactions
order by amount asc , customer_id desc;

```
![](https://hackmd.io/_uploads/SJE7pSgGp.png)

### limit
limit在抓取大量資料的時候可以做pagination，讓你一次抓到資料不用顯示這麼多。同時也可以跟order結合。
下面是用last name排完之後，顯示前兩筆。

```
select * 
from customers
order by last_name desc 
limit 2;
```

![](https://hackmd.io/_uploads/S1xaTBxMa.png)

談到pagination，就是前面可以加上offset來做為顯示。
前面offset為1，一次顯示兩筆資料。
```
select * 
from customers
limit 1,2;

![](https://hackmd.io/_uploads/HJXV0SeGT.png)
```
### Union
可以將不相干的兩個表結合。
若我想把顧客跟員工整合成一個名單。
```
select 
first_name, last_name from employees
union 
select first_name, last_name from customers
```

![](https://hackmd.io/_uploads/S1S7zIlGa.png)

但要注意，Union若在兩個表中都有的名字，只會出現一次，這時候你就要使用union all

```
select 
first_name, last_name from employees
union all
select first_name, last_name from customers
```

### self join
主要比較同一張表的row可以看

```
alter table customers
add referral_id INT;
select * from customers;

```
先增加一個位置，然後把customer id 加入


```
update customers 
set referral_id =2
where customer_id = 4;
select * from customers;
```

主要還是把自己的兩個表分別命名，然後在特定條件下可以接起來。
```
select *
from customers as a
left join customers as b
ON a.referral_id = b.customer_id;
```
![](https://hackmd.io/_uploads/H18u48lG6.png)


若要顯示可以這樣寫
```
select a.customer_id, a.first_name, a.last_name, b.first_name, b.last_name
from customers as a
left join customers as b
ON a.referral_id = b.customer_id;
```
![](https://hackmd.io/_uploads/ByiWrIxza.png)


也但都是firstname lastname太難判斷，寫成下面這樣concat另外命名。
```
select a.customer_id, a.first_name, a.last_name, 
concat(b.first_name," ", b.last_name) as "referred_by"
from customers as a
left join customers as b
ON a.referral_id = b.customer_id;
```

![](https://hackmd.io/_uploads/SkI8SLgf6.png)


若我想把員工跟supervisor放一起的話
先寫這下面(整理之前)
```
select *
from employees as a 
left join employees as b
on a.supervisor_id = b.employee_id;
```

![](https://hackmd.io/_uploads/BytZqLlMp.png)

整理後如下

```
select concat(a.first_name," ", a.last_name) as employee, 
concat(b.first_name," ", b.last_name) as supervisor 
from employees as a 
left join employees as b
on a.supervisor_id = b.employee_id;
```
![](https://hackmd.io/_uploads/HyVg98lzp.png)

### View
透過現有表格組合成一個新的view
```
create view employee_attendace as
select first_name, last_name
from employees;

```
![](https://hackmd.io/_uploads/S1P-qJ-G6.png)

若本來的table有所改變，View也會跟著改變，因為他是拿本來table的component的。

### Index
主要是拿來增加搜查速度的，大資料庫可以設定比較好搜查的資料方式增強資料搜查速度。
這樣搜查last_name當資料的時候就會比本來快上很多。

```
create index last_name_idx
on customers(last_name);
```

![](https://hackmd.io/_uploads/SJBuilZza.png)

也可以同時設定兩個欄位增進Index
```
create index last_name_first_name_idx
On customers(last_name, first_name);
```
秀index
```
show indexes from customers;
```
![](https://hackmd.io/_uploads/HJ1l3lWfp.png)

想拉到不要的就如下圖這樣做。
![](https://hackmd.io/_uploads/H1Wb2g-Mp.png)


### subquery
可以在query裡面在放另外一個query結合成一個表。
```
select first_name, last_name, hourly_pay, 
(select avg(hourly_pay)  from employees)  as avergae_hourly_pay
from employees;
```

![](https://hackmd.io/_uploads/HkCNrWbzT.png)

或是寫一個找到大於平均薪資員工的表
```
select first_name, last_name, hourly_pay
from employees
where hourly_pay >(select avg(hourly_pay)  from employees) ;
```

![](https://hackmd.io/_uploads/ryt_8W-fT.png)


或是找到那些曾經點過餐的Customer
```
select customer_id 
from transactions 
where customer_id is not null;
```
![](https://hackmd.io/_uploads/SJpZwW-f6.png)

若不想出現重複的，就加上distinct
```
select distinct customer_id 
from transactions 
where customer_id is not null;
```

![](https://hackmd.io/_uploads/SJJND-bMT.png)


```
select first_name, last_name
from customers
where customer_id in
(select distinct customer_id 
from transactions 
where customer_id is not null);
```
![](https://hackmd.io/_uploads/SkxbCZZza.png)
其實這個寫起來結果就是如下
![](https://hackmd.io/_uploads/HylMRWbG6.png)

做的時候可以先完成subquery在完成大的。


- group by
今天我想把兩個column合併起來，所以使用了groupby。

我有一個transaction table寫了每個交易哪天產生，我想把每天總額算出來。

```
select sum(amount) as sum,order_date as orderdate
from transactions
group by order_date;
```
![](https://hackmd.io/_uploads/H1aXMf-fa.png)

想找到每天最大筆的金額
```
select max(amount) as max,order_date as orderdate
from transactions
group by order_date;
```

![](https://hackmd.io/_uploads/H1n8MMWG6.png)

想找最小的交易
```
select min(amount) as min,order_date as orderdate
from transactions
group by order_date;
```

![](https://hackmd.io/_uploads/BkadzMZf6.png)

每天平均交易金額
```
select avg(amount) as avg,order_date as orderdate
from transactions
group by order_date;
```
![](https://hackmd.io/_uploads/r1z5MzbM6.png)

每天的交易筆數
```
select count(amount) as cnt,order_date as orderdate
from transactions
group by order_date;
```
![](https://hackmd.io/_uploads/HJehMzbzT.png)

想算個別客戶花的錢
null的會放在一起。
```
select sum(amount) as sum, customer_id 
from transactions
group by customer_id;
```
![](https://hackmd.io/_uploads/rkAeQMZMT.png)

```
select max(amount) as max, customer_id 
from transactions
group by customer_id;
```
![](https://hackmd.io/_uploads/HyQQQMWzT.png)

min
```
select min(amount) as min, customer_id 
from transactions
group by customer_id;
```
![](https://hackmd.io/_uploads/Hy3VXzZG6.png)

average
```
select avg(amount) as average, customer_id 
from transactions
group by customer_id;
```
![](https://hackmd.io/_uploads/rkOL7MWfp.png)

來的次數
```
select count(amount) as times, customer_id 
from transactions
group by customer_id;

```

![](https://hackmd.io/_uploads/Skg29XMbG6.png)


但要記得，若group出來之後想要同時加上限制條件，要使用having而不是where

```
select count(amount) as times, customer_id 
from transactions
group by customer_id
having count(amount)>1;
```
![](https://hackmd.io/_uploads/SkNxNGZz6.png)

若不知名的客人出來太多次
可以這樣調整
```
select count(amount) as times, customer_id 
from transactions
group by customer_id
having count(amount)>1 and customer_id is not null;


```

### roll up 
rollup可以做在groupby之後，這樣可以直接把所有統計出來的東西加總在下，在最後面加上with rollup

```
select sum(amount),order_date
from transactions 
group by order_date with rollup;
```

![](https://hackmd.io/_uploads/Sy-YBGZfa.png)

想看交易紀錄次數
也可以這樣做
```
select count(transaction_id),order_date
from transactions 
group by order_date with rollup;
```

![](https://hackmd.io/_uploads/BJ_3HGbGT.png)



```
select count(transaction_id) as '# of orders' ,customer_id
from transactions 
group by customer_id with rollup;
```

![](https://hackmd.io/_uploads/HJ5BIfZMT.png)

員工薪資總和

```
select sum(hourly_pay) as 'sum of payment', employee_id 
from employees 
group by employee_id with rollup
```
![](https://hackmd.io/_uploads/SJhs8fZGa.png)


### On Delete
- On delete set null
```
Create table transactions(
	transaction_id INT primary key,
    amount decimal (5,2),
    customer_id INT,
    order_date date,
    foreign key(customer_id) references cutomers(customer_id)
    on Delete set null
);
```

這是一種讓customer id 被刪除時候，我的這個foreign key可以自動變成NULL的方式不會被影響。

但若已經建立了就只能移除本來的foreignkey然後新建新的

![](https://hackmd.io/_uploads/SkvnTzZM6.png)


再新建一個新的
```
alter table transactions
add constraint fk_customer_idd
foreign key(customer_id) references customers(customer_id)
on delete set null;
```

- On delete cascade





```
Alter table transactions 
add constraint fk_transactions_iid
foreign key (customer_id) references customers(customer_id)
on delete cascade;
```
這樣設定可以讓foreignkey被刪掉的時候
相關的檔案一起消失。

### store procedure
若一個東西常常要使用跟出現，我們會把他直接存起來，然後再call出來。

為了設定我們可能暫時把 ; 換掉$$，因為這樣兩個;會造成編譯錯誤


```
delimiter $$
create procedure get_customer()
begin 
	select * from customers;
end $$
Delimiter ;

```
接下來把procedure叫出就有以下畫面
![](https://hackmd.io/_uploads/r1r_vm-za.png)

若要刪除procedure可以這樣做
![](https://hackmd.io/_uploads/Bk0_Y7bM6.png)


或是寫一個可以找到特定資料的，裡面含有參數
```

delimiter $$
create procedure find_customer(IN id INT)

BEGIN
	select * 
    from customers
    where customer_id =id;
END $$
delimiter ;

```
![](https://hackmd.io/_uploads/rkJr9mbM6.png)

![](https://hackmd.io/_uploads/S1Pr57bfa.png)

若一次想要給兩個參數也可以這樣設定


```
Delimiter $$
create procedure find_customer(in f_name varchar(50), 
							   in l_name varchar(50))
                               
Begin
	select * 
    from customers
    where first_name = f_name and last_name = l_name;
End $$

delimiter ;
```
![](https://hackmd.io/_uploads/BkUujmbGa.png)

使用store procedure好處如下
![](https://hackmd.io/_uploads/ryFniQZG6.png)


### trigger
有些事情發生之後，就會觸發。
當增加刪除發生的時候可以查看表單，看錯誤或式audit table。


### Trigger

```
create trigger before_hourly_pay
before update on employees
for each row
set New.salary = (New.hourly_pay * 2080);

```

這樣更改員工薪資之後他們都會自動計算。


下面這個可以讓我每次新增員工的時候自動計算薪資
```
create trigger before_hourly_pay_insert
before INSERT ON employees
for each row
set new.salary=(new.hourly_pay *2080);
```

新增員工後
```
INSERT INTO EMPLOYEES
VALUES(6,"SHELDON", "PLANKTON", 10, null, "JANITOR", "2023-01-07", 1);
SELECT * FROM EMPLOYEES;
```

![](https://hackmd.io/_uploads/HyqczEWGp.png)


另外一個表格計算所有開支

```
update expenses
set expense_total =(select sum(salary) from employees)
where expense_name = 'salaries';
select * from expenses;
```
![](https://hackmd.io/_uploads/BkOJEVbGa.png)

設定trigger
```
create trigger after_salary_delete
after delete on employees 
for each row 
update expenses
set expense_total =expenses_total - old.salary
where expense_name='salaries';
```

若員工薪資調整時，自動計算

```
create trigger after_salary_update
after update on employees
for each row
update expenses
set expense_total= expense_total+(New.salary - OLD.salary)
where expense_name = 'salaries';
select * from expenses;
```
