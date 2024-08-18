# Library-management-System-using-JDBC-JSP-Servlet-MySql-using-Crud-operations
1. Set Up the Environment
Install Java Development Kit (JDK): Ensure you have JDK installed.
Set Up MySQL Database: Install MySQL Server and MySQL Workbench (or any other GUI tool).
Configure Apache Tomcat: Install and configure Apache Tomcat server for deploying JSP and Servlet applications.
Set Up Your IDE: Use an IDE like Eclipse or IntelliJ IDEA to manage your project.
2. Database Design
Create the Database:
sql
Copy code
CREATE DATABASE library_db;
Create Tables:
sql
Copy code
CREATE TABLE books (
    book_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(100),
    author VARCHAR(100),
    publisher VARCHAR(100),
    year INT
);

CREATE TABLE members (
    member_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100),
    phone VARCHAR(15)
);

CREATE TABLE transactions (
    transaction_id INT PRIMARY KEY AUTO_INCREMENT,
    book_id INT,
    member_id INT,
    issue_date DATE,
    return_date DATE,
    FOREIGN KEY (book_id) REFERENCES books(book_id),
    FOREIGN KEY (member_id) REFERENCES members(member_id)
);
3. JDBC Setup
Add JDBC Driver: Ensure that the MySQL JDBC driver (mysql-connector-java.jar) is added to your project's classpath.
Create Database Connection:
java
Copy code
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DatabaseConnection {
    private static final String URL = "jdbc:mysql://localhost:3306/library_db";
    private static final String USER = "root";
    private static final String PASSWORD = "your_password";

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(URL, USER, PASSWORD);
    }
}
4. Create CRUD Operations
Create the BookDAO Class:
java
Copy code
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

public class BookDAO {
    public void addBook(Book book) {
        String query = "INSERT INTO books (title, author, publisher, year) VALUES (?, ?, ?, ?)";
        try (Connection conn = DatabaseConnection.getConnection();
             PreparedStatement stmt = conn.prepareStatement(query)) {
            stmt.setString(1, book.getTitle());
            stmt.setString(2, book.getAuthor());
            stmt.setString(3, book.getPublisher());
            stmt.setInt(4, book.getYear());
            stmt.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    // Implement other CRUD methods: getBook, updateBook, deleteBook, listBooks
}
5. JSP Pages
Create JSP Pages for Each Operation:
addBook.jsp: Form to add a new book.
viewBooks.jsp: Display list of books.
editBook.jsp: Form to edit book details.
deleteBook.jsp: Confirmation page to delete a book.
6. Servlets
Create Servlets to Handle Requests:
java
Copy code
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

@WebServlet("/addBook")
public class AddBookServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String title = request.getParameter("title");
        String author = request.getParameter("author");
        String publisher = request.getParameter("publisher");
        int year = Integer.parseInt(request.getParameter("year"));

        Book book = new Book(title, author, publisher, year);
        BookDAO bookDAO = new BookDAO();
        bookDAO.addBook(book);

        response.sendRedirect("viewBooks.jsp");
    }
}
7. Deploy and Test
Deploy on Tomcat: Export your project as a WAR file and deploy it on Tomcat.
Test the Application: Test the various CRUD operations by interacting with the JSP page.

8. Enhancements
User Authentication: Add login and registration features.
Advanced Search: Implement search functionality for books and members.
Transaction Management: Implement issue and return book features.
9. Conclusion
This basic framework provides a starting point. You can extend this application by adding more features and improving the design as needed.
OUTPUT:
![bookcreate](https://github.com/user-attachments/assets/09048c5b-5175-49ee-8d72-a1ddced73db2)
![bookcreateresponse](https://github.com/user-attachments/assets/6a7f2228-243f-4e6a-8504-8a18166a5fb7)
![bdelete](https://github.com/user-attachments/assets/43750a87-2fce-45e5-a94c-f914518b45e9)
![bdeleteresp1](https://github.com/user-attachments/assets/a9582ae0-c494-467d-b373-79e9b69a507f)
![bdeleteresp2](https://github.com/user-attachments/assets/2ab10843-dca5-43ef-8c0c-b4352f836a5c)
![Bupdate](https://github.com/user-attachments/assets/e624a43e-73c7-40d3-ab7c-4af912f5fd33)
![Breadall](https://github.com/user-attachments/assets/7837dbca-c615-4bd0-b771-d4c07a63ed91)






