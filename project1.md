# JDBC connection in Netbeans 

### Used:
Netbeans 8.2
JDK 8
MySQL in MAMP (localhost)

### Creating database in MySQL
CREATE simplejdbcapplication;
USE simplejdbcapplicaton;

### SQL DDL:
create tables - 

create table books
(
   isbn varchar(20) primary key,
   title varchar(50),
   edition varchar(20),
   price float(10,2)
);

create table authors
(
   author_id int primary key,
   author_name varchar(50)
);

create table book_by_author
(
   isbn varchar(20),
   author_id int,
   foreign key(isbn)
      references books(isbn),
   foreign key(author_id)
      references authors(author_id)
);

Insert values in tables -

insert into books values('123456','Discrete Math',
   'Second',56.78);
insert into books values('234567','Integral Calculus',
   'Second',92.73);
insert into books values('345678','Differential Calculus',
   'First',57.86);
insert into books values('456789','Graph Theory',
   'Second',33.8);
insert into books values('567890','Set Theory',
   'Fourth',34.89);
insert into books values('102938','Numerical Methods',
   'Third',98.46);

insert into authors values(1,'CS Liu');
insert into authors values(2,'N Deo');
insert into authors values(3,'Rogers');
insert into authors values(4,'Saxena');
insert into authors values(5,'Sandip');
insert into authors values(6,'Srivastava');
insert into authors values(7,'Jha');

insert into book_by_author values('123456',1);
insert into book_by_author values('123456',2);
insert into book_by_author values('123456',3);
insert into book_by_author values('234567',4);
insert into book_by_author values('234567',5);
insert into book_by_author values('345678',6);
insert into book_by_author values('456789',7);
insert into book_by_author values('567890',4);
insert into book_by_author values('102938',6);


### Create Java project
1. File - New Project 
    Java - Java Application 
2. Give projet name 'simplejdbcapplication'
    Finish
3. Add MySQL jdbc driver
    Right click project - properties
    Libaries
      - Add libary..
      - Click Import button
      - MySQL JDBC driver
      - OK
     Note: The MySQL JDBC Driver is already shipped with NetBeans. 
     There is no need to install it separately. Other drivers, which may not be shipped with NetBeans, may need to be imported as a separate jar file. In such a case, use the Add JAR/Folder button to import it in the current project.
   
  Creating Application 
  package simplejdbcapplication;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class SimpleJDBCApplication {

   static final String DB_URL =
      "jdbc:mysql://localhost:3306/simplejdbcapplication";
   static final String DB_DRV =
      "com.mysql.jdbc.Driver";
   static final String DB_USER = "root";
   static final String DB_PASSWD = "root";

   public static void main(String[] args){

      Connection connection = null;
      Statement statement = null;
      ResultSet resultSet = null;

      try{
         connection=DriverManager.getConnection
            (DB_URL,DB_USER,DB_PASSWD);
         statement=connection.createStatement();
         resultSet=statement.executeQuery
            ("SELECT * FROM books");
         while(resultSet.next()){
            System.out.printf("%s\t%s\t%s\t%f\n",
            resultSet.getString(1),
            resultSet.getString(2),
            resultSet.getString(3),
            resultSet.getFloat(4));
         }

      }catch(SQLException ex){
      }finally{
         try {
            resultSet.close();
            statement.close();
            connection.close();
         } catch (SQLException ex) {
         }
      }
   }
}


Finally Run - Run Project (Ctrl + F11)
