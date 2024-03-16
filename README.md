# Dynamic-web-application

## HTML, CSS and JAVASCRIPT code
### Login-page index.html 

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Form</title>
    
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #000; /* Changed background color to black */
            margin: 0;
            padding: 0;
        }

        .container {
            max-width: 500px;
            margin: 100px auto;
            padding: 20px;
            background-color: #333; 
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            font-weight: bold;
            margin-bottom: 5px;
            color: #fff; 
        }

        input[type="text"],
        input[type="email"],
        input[type="number"],
        input[type="date"] {
            width: calc(100% - 12px);
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            background-color: #555; 
            color: #fff; 
        }

        button[type="submit"],
        #displayButton {
            background-color: white; 
            color: black;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            margin-top: 10px;
        }

        button[type="submit"]:hover,
        #displayButton:hover {
            background-color: white; 
        }

        .error-message {
            color: red;
            margin-top: 5px;
        }
    </style>

</head>
<body>
    <div class="container">
        <form id="userForm" action="submit" method="post">
            
            <div class="form-group">
                <label for="name">Name:</label>
                <input type="text" id="name" name="name" required>
            </div>
            
            <div class="form-group">
                <label for="email">Email:</label>
                <input type="email" id="email" name="email" required>
            </div>
            
            <div class="form-group">
                <label for="age">Age:</label>
                <input type="number" id="age" name="age" required>
                <span class="error-message" id="ageError"></span>
            </div>
            
            <div class="form-group">
                <label for="dob">Date of Birth:</label>
                <input type="date" id="dob" name="dob" required>
            </div>
            
            <button type="submit">Submit</button>
            <button id="displayButton">Display User Data</button>
        </form>
        
    </div>

    <script>
        document.getElementById("userForm").addEventListener("submit", function(event) {
            const age = document.getElementById("age").value;
            if (isNaN(age) || age < 0) {
                document.getElementById("ageError").textContent = "Age must be a positive integer.";
                event.preventDefault();
            } else {
                document.getElementById("ageError").textContent = "";
            }
        });

        document.getElementById("displayButton").addEventListener("click", function() {
            window.location.href = "display";
        });
    </script>
</body>
</html>


## JEE and JDBC code:
### UserRegisterServlet.java

package com.sevlet.register;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/submit")
public class UserRegisterServlet extends HttpServlet {

    protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        req.getRequestDispatcher("/index.html").forward(req, res);
        res.sendRedirect("display");

    }

    protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        res.setContentType("text/html");
        PrintWriter out = res.getWriter();

        String name = req.getParameter("name");
        String email = req.getParameter("email");
        int age = Integer.parseInt(req.getParameter("age"));
        String dob = req.getParameter("dob");

        Connection con = null;
        PreparedStatement pstmt = null;
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            con = DriverManager.getConnection("jdbc:mysql://localhost:3306/juneb1", "root", "Sana@1234");
            String query = "INSERT INTO USER_DATA (name, email, age, dob) VALUES (?, ?, ?, ?)";
            pstmt = con.prepareStatement(query);

            pstmt.setString(1, name);
            pstmt.setString(2, email);
            pstmt.setInt(3, age);
            pstmt.setString(4, dob);

            int count = pstmt.executeUpdate();
            if (count > 0) {
                out.println("<h3>Record Stored into Database</h3>");
            } else {
                out.println("<h3>Failed to store record into Database</h3>");
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
            out.println("<h3>Error: " + e.getMessage() + "</h3>");
        } finally {
            try {
                if (pstmt != null) {
                    pstmt.close();
                }
                if (con != null) {
                    con.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        out.close();
    }
}

### DisplayServlet.java
package com.sevlet.register;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/display")
public class DisplayServlet extends HttpServlet {

    protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
        res.setContentType("text/html");
        PrintWriter out = res.getWriter();

        out.println("<!DOCTYPE html>");
        out.println("<html>");
        out.println("<head>");
        out.println("<meta charset=\"UTF-8\">");
        out.println("<title>User Data</title>");
        out.println("</head>");
        out.println("<body>");

        out.println("<h2>User Data</h2>");
        out.println("<table border=\"1\">");
        out.println("<tr>");
        out.println("<th>Name</th>");
        out.println("<th>Email</th>");
        out.println("<th>Age</th>");
        out.println("<th>Date of Birth</th>");
        out.println("</tr>");

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/juneb1", "root", "Sana@1234");
            PreparedStatement pstmt = con.prepareStatement("SELECT * FROM USER_DATA");
            ResultSet rs = pstmt.executeQuery();

            while (rs.next()) {
                String name = rs.getString("name");
                String email = rs.getString("email");
                int age = rs.getInt("age");
                String dob = rs.getString("dob");

                out.println("<tr>");
                out.println("<td>" + name + "</td>");
                out.println("<td>" + email + "</td>");
                out.println("<td>" + age + "</td>");
                out.println("<td>" + dob + "</td>");
                out.println("</tr>");
            }

            pstmt.close();
            con.close();
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
            out.println("<tr><td colspan=\"4\">Error: " + e.getMessage() + "</td></tr>");
        }

        out.println("</table>");
        out.println("</body>");
        out.println("</html>");
        out.close();
    }
}


## Display table Data:

### display.html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Data</title>
    <style>
        body {
            display: flex;
            align-items: start;
            justify-content: center;
            height: 97.5vh;
        }

        .table-info h2 {
            color: blueviolet;
            text-align: center;
        }

        .table-info table, tr, td, th {
            border: 1px solid blueviolet;
            border-collapse: collapse;
            padding: 20px;
        }
        
    </style>
</head>
<body>
    <h2>Users</h2><br>
    <table border="1">
        <thead>
            <tr>
                <th > ID </th>
                <th > Name </th>
                <th >Email</th>
                <th >Age</th>
                <th >Date</th>
            </tr>
        </thead>
        <tbody>
            <tr th:each="user : ${userDetails}">
                <td th:text="${user.id}"></td>
                <td th:text="${user.name}"></td>
                <td th:text="${user.email}"></td>
                <td th:text="${user.age}"></td>
                <td th:text="${user.dob}"></td>
            </tr>
        </tbody>
    </table>
</body>
</html>


