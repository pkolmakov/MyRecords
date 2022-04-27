Create database ZamonaWebShop

USE ZamonaWebShop you can use name in url 

DROP TABLE Book

CREATE TABLE [dbo].[Book](id int NOT NULL, 
title nvarchar(200) NOT NULL,
CONSTRAINT PK_Book_id PRIMARY KEY (id))


CREATE TABLE Writer(id int NOT NULL,
name nvarchar(300) NOT NULL,
CONSTRAINT PK_Writer_id PRIMARY KEY (id),
CONSTRAINT UK_Writer_name UNIQUE (name))

CREATE TABLE AuthorBook(bookId int NOT NULL,
writerId int NOT NULL,
CONSTRAINT FK_AuthorBook_Book_Id FOREIGN KEY (bookId)
REFERENCES Book(id),
CONSTRAINT FK_AuthorBook_Writer_id FOREIGN KEY (writerId)
REFERENCES Writer(id)
)


INSERT INTO BOOK (id,title) values(1,'Scary')
INSERT INTO BOOK (id,title) values(2,'Doom')
INSERT INTO Writer (id,name) values(1,'Pasha')
INSERT INTO Writer (id,name) values(2,'Sasha')



///Try to add foreign keys first and then only add composite primary keys

1)G

SELECT * FROM students 
join courses using (id)
select * From students

insert into students(id) values(3)
insert into students values(4,'Pasha', 4.0)

SET SHOWPLAN_ALL ON
GO


SELECT s.name,count(*)
FROM students s
join enrollment e on s.id = e.studentId
group by s.name

GO

SET SHOWPLAN_ALL OFF
GO


SELECT s.name,count(*)
FROM students s
left join enrollment e on s.id = e.studentId
group by s.name

SELECT *
FROM students s
full join enrollment e on s.id = e.studentId

