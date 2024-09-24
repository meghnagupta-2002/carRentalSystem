*Car Rental System*

For making database for Car Rental System we have to first create database using CREATE command.
Query:- create database carRentalSystem; 
 

Next, we will connect to the database
Query:- use carRentalSystem;
 

Now, it’s time to create tables in the database
1) Vehicle Table
Query:- 
create table vehicle (vehicleID int primary key, make varchar(15), model   varchar(15),
 year int, dailyRate decimal(5,2), status int, passengerCapacity int,  engineCapacity int);
 





2) Customer Table
Query:- 
create table customer (customerID int primary key, firstname varchar(10),
 lastname varchar(10), email varchar(50), phoneNumber varchar(15));
 

3) Lease Table
Query:- 
create table lease (leaseID int primary key, vehicleID int, customerID int, startDate date, endDate date, leaseType varchar(10), foreign key (vehicleID) references vehicle(vehicleID),foreign key (customerID) references customer(customerID));
 




4) Payment Table
Query:- 
create table payment (paymentID int primary key, leaseID int, paymentDate date,
 amount float, foreign key (leaseID) references lease(leaseID));
 

After creating tables, we will now insert given data 
1) Vehicle Table
Query:-
insert into vehicle values
(1,'Toyota','Camry',2022,50.00,1,4,1450), 
(2,'Honda','Civic',2023,45.00,1,7,1500), 
(3,'Ford','Focus',2022,48.00,0,4,1400), 
(4,'Nissan','Altima',2023,52.00,1,7,1200),
(5,'Chevrolet','Malibu',2022,47.00,1,4,1800),
(6,'Hyundai','Sonata',2023,49.00,0,7,1400),
(7,'BMW','3 Series',2023,60.00,1,4,2499),
(8,'Mercedes','C-Class',2022,58.00,1,4,2599),
(9,'Audi','A4',2022,55.00,0,4,2500),
(10,'Lexus','ES',2023,54.00,1,4,2500);
 

2) Customer Table
Query:-
insert into customer values
(1,'John','Doe','johndoe@example.com','555-555-5555'),
(2,'Jane','Smith','janesmith@example.com','555-123-4567'),
(3,'Robert','Johnsn','robert@example.com','555-789-1234'),
(4,'Sarah','Brown','sarah@example.com','555-456-7890'),
(5,'David','Lee','david@example.com','555-987-6543'),
(6,'Laura','Hill','laura@example.com','555-234-5678'),
(7,'Michael','David','michael@example.com','555-876-5432'),
(8,'Emma','Wilson','emma@example.com','555-432-1098'),
(9,'William','Taylor','william@example.com','555-321-6547'),
(10,'Olivie','Adams','olivia@example.com','555-765-4321'); 
 

3) Lease Table
Query:-
insert into lease values
(1,1,1,'2023-01-01','2023-01-05','Daily'),
(2,2,2,'2023-02-15','2023-02-28','Monthly'),
(3,3,3,'2023-03-10','2023-03-15','Daily'),
(4,4,4,'2023-04-20','2023-04-30','Monthly'),
(5,5,5,'2023-05-05','2023-05-10','Daily'),
(6,4,3,'2023-06-15','2023-06-30','Monthly'),
(7,7,7,'2023-07-01','2023-07-10','Daily'),
(8,8,8,'2023-08-12','2023-08-15','Monthly'),
(9,3,3,'2023-09-07','2023-09-10','Daily'),
(10,10,10,'2023-10-10','2023-10-31','Monthly');
 

4) Payment Table
Query:- 
insert into payment values
(1,1,'2023-01-03',200.00),
(2,2,'2023-02-20',1000.00),
(3,3,'2023-03-12',75.00),
(4,4,'2023-04-25',900.00),
(5,5,'2023-05-07',60.00),
(6,6,'2023-06-18',1200.00),
(7,7,'2023-07-03',40.00),
(8,8,'2023-08-14',1100.00),
(9,9,'2023-09-09',80.00),
(10,10,'2023-10-25',1500.00);
 

Now we will write MySQL queries for given questions.
1. Update the daily rate for a Mercedes car to 68.
Query:-
update table vehicle set dailyRate = 68.00 where make = ‘Mercedes’;
 

 2. Delete a specific customer and all associated leases and payments.
Queries:-
delete from payment where leaseID in (select leaseID from lease where customerID=7);
delete from lease where customerID=7;
delete from customer where customerID=7;
   


3. Rename the "paymentDate" column in the Payment table to "transactionDate".
Query:-
alter table payment rename column paymentDate to transactionDate;
  

4. Find a specific customer by email.
Query:- 
select concat(firstname," ", lastname) as customerName  from customer where email='sarah@example.com'; 
 

5. Get active leases for a specific customer.
Query:-
select * from lease where customerID=3 and endDate>='2023-06-30'; 

6. Find all payments made by a customer with a specific phone number.
Query:-
select p.* from payment p where leaseID in (select l.leaseID from lease l join customer c on c.customerID=l.customerID where c.phoneNumber='555-555-5555'); 
 

7. Calculate the average daily rate of all available cars.
Query:-
select avg(dailyRate) as AvgDailyRate  from vehicle where status=1;
 

8. Find the car with the highest daily rate.
Query:-
select make as carName, dailyRate  from vehicle where dailyRate= (select max(dailyRate) from vehicle);
 


9. Retrieve all cars leased by a specific customer.
Query:-
select v.make as carName, c.customerID, l.leaseID from vehicle v join customer c join lease l on c.customerID = l.customerID and v.vehicleID = l.vehicleID where c.customerID=3;
 

10. Find the details of the most recent lease.
Query:- 
select l.*, v.make, v.model, c.firstname, c.lastname from lease l join vehicle v join customer c on l.vehicleID = v.vehicleID and l.customerID = c.customerID order by l.startDate DESC limit 1;
 

11. List all payments made in the year 2023.
Query:-
select * from payment where transactionDate between '2023-01-01' and '2023-12-31';
 




12. Retrieve customers who have not made any payments.
Query:-
select c.* from customer c left join lease l on c.customerID = l.customerID left join payment p on l.leaseID = p.leaseID where paymentID is null;
 

13. Retrieve Car Details and Their Total Payments.
Query:- 
select v.vehicleID, v.make, v.model, v.year, sum(p.amount) as totalPayments  from vehicle v join lease l join payment p on v.vehicleID = l.vehicleID and l.leaseID = p.leaseID group by v.vehicleID, v.make, v.model, v.year;
 
14. Calculate Total Payments for Each Customer.
Query:- 
select c.customerID, concat(c.firstname," ",c.lastname) as customerName, sum(p.amount) from customer c left join lease l on c.customerID = l.customerID left join payment p on l.leaseID = p.leaseID group by c.customerID, c.firstname, c.lastname;
 
15. List Car Details for Each Lease.
Query:- 
select l.leaseID, v.vehicleID, v.make, v.model, v.year, l.startDate, l.endDate, l.leaseType from lease l join vehicle v on l.vehicleID = v.vehicleID;
 

16. Retrieve Details of Active Leases with Customer and Car Information.
Query:-
select l.leaseID, c.customerID, c.firstname, c.lastname,  v.vehicleID, v.make, v.model, v.year, l.startDate, l.endDate, l.leaseType from lease l join customer c join vehicle v on l.customerID = c.customerID and l.vehicleID = v.vehicleID where l.endDate>'2023-06-30';  
 

17. Find the Customer Who Has Spent the Most on Leases.
Query:-
select c.customerID, c.firstname, c.lastname, SUM(p.amount) as mostSpent from customer c join lease l join payment p on c.customerID = l.customerID and l.leaseID = p.leaseID group by c.customerID, c.firstname, c.lastname order by mostSpent desc limit 1;
 

18. List All Cars with Their Current Lease Information.
Query:- 
select v.vehicleID, v.make, v.model, v.year, l.leaseID, l.customerID, l.startDate, l.endDate, l.leaseType from vehicle v left join lease l on v.vehicleID = l.vehicleID ;
 

