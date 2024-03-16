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

