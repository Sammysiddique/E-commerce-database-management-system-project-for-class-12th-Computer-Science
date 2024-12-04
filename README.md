# Project file of E-commerce database management system for class 12th Computer Science
[CLASS 12TH COMPUTER SCIENCE PROJECT  BY SAMEER SIDDIQUE.pdf](https://github.com/user-attachments/files/18012607/CLASS.12TH.COMPUTER.SCIENCE.PROJECT.BY.SAMEER.SIDDIQUE.pdf)
# E-commerce-database-management-system-project-for-class-12th-Computer-Science
This repository contains the source code and supporting files for a Class 12 Computer Science Project, showcasing an E-Commerce Database Management System built using Python and MySQL. The project demonstrates database management and programming skills to handle core operations of an e-commerce platform.

# Key Features:
Product Management: Add, update, view, and delete product details (name, price, stock levels). 

Customer Management: Maintain customer records with their contact information.

Order Management: Record and manage customer orders, including product details, quantities, and timestamps.

Database Integration: Uses MySQL to store, retrieve, and manipulate data.

Interactive Console Menu: Simple text-based interface to navigate between different functionalities.

# Technologies Used:
•Programming Language: Python
•Database: MySQL
•Libraries: mysql-connector-python for database connectivity

# MySQL Source code create database of E-commerce database management system 

    CREATE DATABASE ecommerce;
    
    USE ecommerce;
    
    CREATE TABLE products (
        product_id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(100),
        price DECIMAL(10, 2),
        stock INT
    );
    
    CREATE TABLE customers (
        customer_id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(100),
        contact_info VARCHAR(100)
    );
    
    CREATE TABLE orders (
        order_id INT AUTO_INCREMENT PRIMARY KEY,
        customer_id INT,
        product_id INT,
        quantity INT,
        order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        FOREIGN KEY (customer_id) REFERENCES customers(customer_id),
        FOREIGN KEY (product_id) REFERENCES products(product_id)
    );

# Python source code to build streamline logics and structure 
    import mysql.connector
    from datetime import datetime
    
    # Connect to the database
    database = mysql.connector.connect(
        host="localhost",
        user="root", #Default_username = 'root',but'kira'(in mycase) 
        password="Monster@0007", 
        database="ecommerce"
    )
    
    cursor = database.cursor()
                                                                                                                                             
    # Main Menu
    def main_menu():
        while True:
            print("\n=== E-commerce Shop Management ===")
            print("1. Manage Products")
            print("2. Manage Customers")
            print("3. Manage Orders")
            print("4. Exit")
            choice = input("Enter your choice: ")
    
            if choice == '1':
                manage_products()
            elif choice == '2':
                manage_customers()
            elif choice == '3':
                manage_orders()
            elif choice == '4':
                print("Exiting the system.")
                break
            else:
                print("Invalid choice. Please try again.")
    
    #Manage Products
    def manage_products():
        while True:
            print("\n--- Manage Products ---")
            print("1. Add Product")
            print("2. View Products")
            print("3. Update Product")
            print("4. Delete Product")
            print("5. Back to Main Menu")
            choice = input("Enter your choice: ")
    
            if choice == '1':
                add_product()
            elif choice == '2':
                view_products()
            elif choice == '3':
                update_product()
            elif choice == '4':
                delete_product()
            elif choice == '5':
                break
            else:
                print("Invalid choice. Please try again.")
    
    def add_product():
        name = input("Enter product name: ")
        price = float(input("Enter product price: "))
        stock = int(input("Enter product stock: "))
        query = "INSERT INTO products (name, price, stock) VALUES (%s, %s, %s)"
        cursor.execute(query, (name, price, stock))
        database.commit()
        print("Product added successfully.")
    
    def view_products():
        query = "SELECT * FROM products"
        cursor.execute(query)
        for row in cursor.fetchall():
            print(row)
    
    def update_product():
        product_id = int(input("Enter product ID to update: "))
        name = input("Enter new product name: ")
        price = float(input("Enter new product price: "))
        stock = int(input("Enter new product stock: "))
        query = "UPDATE products SET name=%s, price=%s, stock=%s WHERE product_id=%s"
        cursor.execute(query, (name, price, stock, product_id))
        database.commit()
        print("Product updated successfully.")
        
    def delete_product():
        product_id = int(input("Enter product ID to delete: "))
        query = "DELETE FROM products WHERE product_id=%s"
        cursor.execute(query, (product_id,))
        database.commit()
        print("Product deleted successfully.")
    
    # Manage Customers
    def manage_customers():
        while True:
            print("\n--- Manage Customers ---")
            print("1. Add Customer")
            print("2. View Customers")
            print("3. Back to Main Menu")
            choice = input("Enter your choice: ")
    
            if choice == '1':
                add_customer()
            elif choice == '2':
                view_customers()
            elif choice == '3':
                break
            else:
                print("Invalid choice. Please try again.")
    
    def add_customer():
        name = input("Enter customer name: ")
        contact_info = input("Enter customer contact info: ")
        query = "INSERT INTO customers (name, contact_info) VALUES (%s, %s)"
        cursor.execute(query, (name, contact_info))
        database.commit()
        print("Customer added successfully.")
    
    def view_customers():
        query = "SELECT * FROM customers"
        cursor.execute(query)
        for row in cursor.fetchall():
            print(row)
    
    # Manage Orders
    def manage_orders():
        while True:
            print("\n--- Manage Orders ---")
            print("1. Add Order")
            print("2. View Orders")
            print("3. Back to Main Menu")
            choice = input("Enter your choice: ")
    
            if choice == '1':
                add_order()
            elif choice == '2':
                view_orders()
            elif choice == '3':
                break
            else:
                print("Invalid choice. Please try again.")
    
    def add_order():
        customer_id = int(input("Enter customer ID: "))
        product_id = int(input("Enter product ID: "))
        quantity = int(input("Enter quantity: "))
        
        # Check product stock
        query = "SELECT stock FROM products WHERE product_id=%s"
        cursor.execute(query, (product_id,))
        stock = cursor.fetchone()[0]
        
        if stock >= quantity:
            query = "INSERT INTO orders (customer_id, product_id, quantity) VALUES (%s, %s, %s)"
            cursor.execute(query, (customer_id, product_id, quantity))
            query = "UPDATE products SET stock = stock - %s WHERE product_id = %s"
            cursor.execute(query, (quantity, product_id))
            database.commit()
            print("Order placed successfully.")
        else:
            print("Insufficient stock.")
    
    def view_orders():
        query = """
        SELECT o.order_id, c.name AS customer_name, p.name AS product_name, o.quantity, o.order_date
        FROM orders o
        JOIN customers c ON o.customer_id = c.customer_id
        JOIN products p ON o.product_id = p.product_id
        """
        cursor.execute(query)
        for row in cursor.fetchall():
            print(row)
    
    # Run the program
    main_menu()
    
    # Close the database connection
    cursor.close()
    database.close()
    
    """Code_developer of this E-commerce_Managemnet_system is Sameer_siddique 
       Class-XII/A , Roll_no. - 24 , Admn_no. - 5274
       School- DAV Public School , Kathara , Bokaro , Jharkhand - 829116 
    """
