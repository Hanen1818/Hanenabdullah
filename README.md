DROP TABLE Employee CASCADE CONSTRAINT;
CREATE TABLE Employee(
SSN CHAR(10) PRIMARY KEY,
F_Name VARCHAR2(10) NOT NULL,
L_Name VARCHAR2(10) NOT NULL,
Gender  CHAR(1) CHECK(Gender in('F','M')),
Dob     Date,
Num_Pho NUMBER(10)NOT NULL ,
Email   VARCHAR2(30) NOT NULL UNIQUE 
);
----------Driver------------------------
DROP TABLE  Driver CASCADE CONSTRAINT; 
CREATE TABLE Driver(
SSN CHAR(10) PRIMARY KEY,
F_Name VARCHAR2(10) NOT NULL,
L_Name VARCHAR2(10) NOT NULL,
Gender  CHAR(1) CHECK(Gender='M'),
Dob     DATE ,
Salary  NUMBER(5)
);
---------------BUS----------------------
DROP TABLE Bus CASCADE CONSTRAINT;
create table Bus(
b_num char(8)PRIMARY KEY,
d_ssn char(10),
max_seat number(30),
station varchar2(20),
b_reg_on varchar2(14),
b_type varchar2(14),
hour_work varchar2(3),
CONSTRAINT Bus_FK FOREIGN KEY (d_ssn) REFERENCES Driver (SSN)
);
---------------Manege----------------------
DROP TABLE Manege;
create table Manege(
E_ssn char (10),
d_ssn char (10),
CONSTRAINT Ma_FK FOREIGN KEY (E_ssn) references Employee(SSN),
CONSTRAINT Ma_FK2 FOREIGN KEY (d_ssn) REFERENCES Driver(SSN)
);
---------------------Reservtion-------------
DROP TABLE Reservation_Info CASCADE CONSTRAINT;
create table Reservation_Info(
res_id number(6) PRIMARY KEY,
departure_da Date,
return_da Date,
R_from varchar2(20),
R_to varchar2(20),
departure_tim  CHAR(4),
return_tim  CHAR(4)
);
--------------------passenger------------------
DROP table Passenger CASCADE CONSTRAINT;
Create table Passenger (
P_SSN char (10) primary key,
P_Fname Varchar(10) NOT NULL,
P_Mname Varchar(10)NOT NULL,
P_Lname Varchar(10)NOT NULL,
P_email Varchar(30) UNIQUE,
P_gender char (1) CHECK(P_gender IN('M','F')),
P_num_pho  number (9) NOT NULL UNIQUE,
p_age char(2),
b_num char(8),
CONSTRAINT pass_fk FOREIGN KEY(b_num)REFERENCES Bus(b_num)
);
----------------Ticket------------------------
DROP TABLE Ticket CASCADE CONSTRAINT ;
create table Ticket(
tic_num char(5) PRIMARY KEY,
P_ssn char(10),
res_id number(6) NOT NULL ,
tic_type varchar2(15) NOT NULL,
payment_meth varchar2(20),
price number(5)NOT NULL,
CONSTRAINT pas_FK FOREIGN key(P_ssn) REFERENCES Passenger(P_SSN),
CONSTRAINT res_FK FOREIGN key (res_id) REFERENCES Reservation_Info(res_id)
);
------------------Comfirms-----------------------
DROP TABLE Comfirms ;
Create table Comfirms (
P_SSN char (10),
res_id number (6),
CONSTRAINT com_FK FOREIGN KEY(P_ssn) REFERENCES Passenger(P_SSN),
CONSTRAINT com_FK2 FOREIGN KEY (res_id) REFERENCES Reservation_Info(res_id)
);
--------------------Organize-----------------------
DROP TABLE Organize ;
CREATE TABLE Organize(
E_SSN char (10),
res_id number (6),
CONSTRAINT Org_FK FOREIGN KEY (E_SSN)REFERENCES Employee (SSN),
CONSTRAINT Org_FK2 FOREIGN KEY (res_id) REFERENCES Reservation_Info(res_id)
);

---------------insert Employee------------------------
INSERT INTO Employee VALUES('1103565725','Ahmad','Ali','M','02-01-1984',055523432,'Ah_1@gmail.com');
INSERT INTO Employee VALUES('1102476953','Maha','Abdullah','F','03-12-1988',0509876523,'maha_ab@gmail.com');
INSERT INTO Employee VALUES('1109823776','Ali','fahad','M','11-05-1898',0504444567,'fahad_ali@hotmail.com');
---------------insert Driver --------------------------
INSERT INTO Driver VALUES('1102295877','Ahmad','Abdullah','M','03-04-1986',3000);
INSERT INTO Driver VALUES('1103344556','abdulaziz','khalid','M','12-01-1979',3000);
INSERT INTO Driver VALUES('1108876543','yaser','Ahmad','M','14-02-1980',3000);
----------------insert Bus-----------------------------
INSERT INTO Bus VALUES('2255FFF',1102295877,4,'1212B','available','Private car','10h');
INSERT INTO Bus VALUES('1345HEH',1103344556,30,'3003C','available','BUS','20h');
INSERT INTO Bus VALUES('467NNO',1108876543,30,'1818H','Not available','BUS','15h');
---------------insert manege---------------------------
INSERT INTO Manege VALUES('1103565725','1102295877');
INSERT INTO Manege VALUES('1102476953','1103344556');
INSERT INTO Manege VALUES('1109823776','1108876543');
----------------insert Reservation_Info-----------------
INSERT INTO Reservation_Info VALUES(437053,'09-12-2019 ','09-12-2019 ','GranadaCenter','aljnadreh','5PM','11PM');
INSERT INTO Reservation_Info VALUES(436304,'03-02-2019','03-02-2019','School','Zoo','9AM','11AM');
INSERT INTO Reservation_Info VALUES(437435,'02-05-2019 ','04-05-2019','Riyadh','Jeddah','9AM','5PM');
----------------insert passenger-----------------------
insert into Passenger VALUES ('1108843257','Reem','Ahmad','Ali','R_M_Ali@hotmail.com','F','053378102','21','2255FFF');
insert into Passenger VALUES ('1108847823','Deena','Mohammed','Fahad','D.2017@Gmail.com','F','054732009','9','1345HEH');
insert into Passenger VALUES ('1103256783','Ahmad','Abdallah','Almutairi','Ahm-2BA@hotmail.com','M','055321989','32','467NNO');
---------------insert Ticket--------------------------
INSERT INTO Ticket VALUES ('11123','1108843257', 437053,'vip_membership','Credit card','200');
INSERT INTO Ticket VALUES ('11224','1108847823', 436304,'membership','Cash','50');
INSERT INTO Ticket VALUES ('11453','1103256783', 437435,'vip_membership','Cash','500');
--------------insert Comfirms-------------------------
INSERT INTO Comfirms  VALUES ('1108843257',437053);
INSERT INTO Comfirms  VALUES ('1108847823',436304);
INSERT INTO Comfirms  VALUES ('1103256783',437435);
---------------insert Organize----------------------
INSERT INTO Organize VALUES('1103565725',437053);
INSERT INTO Organize VALUES('1102476953',436304);
INSERT INTO Organize VALUES('1109823776',437435);
-------------Quires-------------------------------
SELECT tic_type,COUNT(tic_num)
FROM Ticket
GROUP BY tic_type; 
SELECT *
FROM Bus
WHERE b_reg_on='available';

SELECL count(*)
FROM Driver 
WHERE Salary = 3000 ;  

SELECT departure_da 
FROM Reservtion
WHERE departure_da between '02-Jan-2018' and '02-Dec-2018';








