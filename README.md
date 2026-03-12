# FSD-mid1 internal lab Exam

Maven project

Discription:
This project is developed using Java, Maven, and Hibernate ORM framework to demonstrate basic database operations using persistent objects. Hibernate is used as an Object Relational Mapping (ORM) tool that simplifies database interaction by mapping Java classes to database tables. In this project, a Department entity class is created with attributes such as ID, Name, Description, Date, and Status, where the ID is automatically generated using Hibernate annotations.

The application is connected to a MySQL database named fsadexam, where the Department entity is mapped to a table in the database. The project demonstrates basic CRUD operations, specifically Insert and Delete operations. A new department record can be inserted into the database using Hibernate session methods, and an existing department record can be deleted by providing the department ID.

A ClientDemo class is implemented to perform these operations. It establishes a connection to the database through Hibernate configuration, opens a session, begins a transaction, and performs the required database operations. Hibernate automatically converts Java objects into database records and manages the interaction between the application and the database efficiently.

This project helps students understand how Hibernate works with Maven, how entity classes are mapped to database tables, and how basic database operations can be performed using Hibernate sessions and transactions.

code:
package com.klef.fsad.exam;

import jakarta.persistence.*;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import java.util.Date;
import java.util.Scanner;

@Entity
@Table(name="department")
class Department
{
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String name;
    private String description;

    @Temporal(TemporalType.DATE)
    private Date date;

    private String status;

    // Getters and Setters

    public int getId() 
    {
        return id;
    }

    public void setId(int id) 
    {
        this.id = id;
    }

    public String getName() 
    {
        return name;
    }

    public void setName(String name) 
    {
        this.name = name;
    }

    public String getDescription() 
    {
        return description;
    }

    public void setDescription(String description) 
    {
        this.description = description;
    }

    public Date getDate() 
    {
        return date;
    }

    public void setDate(Date date) 
    {
        this.date = date;
    }

    public String getStatus() 
    {
        return status;
    }

    public void setStatus(String status) 
    {
        this.status = status;
    }
}

public class ClientDemo
{
    public static void main(String[] args)
    {
        Configuration cfg = new Configuration();
        cfg.configure("hibernate.cfg.xml");
        cfg.addAnnotatedClass(Department.class);

        SessionFactory factory = cfg.buildSessionFactory();
        Session session = factory.openSession();

        Transaction tx = null;
        Scanner sc = new Scanner(System.in);

        try
        {
            // INSERT OPERATION
            tx = session.beginTransaction();

            Department d = new Department();
            d.setName("CSE");
            d.setDescription("Computer Science Department");
            d.setDate(new Date());
            d.setStatus("Active");

            session.persist(d);

            tx.commit();

            System.out.println("Department Inserted Successfully");

            // DELETE OPERATION
            System.out.println("Enter Department ID to Delete:");
            int id = sc.nextInt();

            tx = session.beginTransaction();

            Department dept = session.get(Department.class, id);

            if(dept != null)
            {
                session.remove(dept);
                System.out.println("Department Deleted Successfully");
            }
            else
            {
                System.out.println("Department Not Found");
            }

            tx.commit();
        }
        catch(Exception e)
        {
            if(tx != null)
                tx.rollback();

            e.printStackTrace();
        }

        session.close();
        factory.close();
    }
}

Result:

The Maven Hibernate project was successfully developed to perform database operations on the Department entity. A new department record was inserted into the fsadexam database and the ID was generated automatically. The application was also able to delete a department record based on the given ID. Thus, the required insert and delete operations using Hibernate ORM were successfully executed.
