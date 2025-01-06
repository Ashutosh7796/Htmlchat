# <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Management Form</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f9f9f9;
        }

        .container {
            width: 90%;
            max-width: 600px;
            margin: 20px auto;
            padding: 20px;
            background: #fff;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
        }

        h2 {
            text-align: center;
            color: #333;
        }

        .form-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }

        input {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 16px;
        }

        .btn {
            padding: 10px 20px;
            color: #fff;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            margin-top: 10px;
        }

        .btn-primary {
            background: #007bff;
        }

        .btn-warning {
            background: #ffc107;
        }

        .btn-success {
            background: #28a745;
        }

        .btn-danger {
            background: #dc3545;
        }

        .output {
            margin-top: 20px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            background: #f8f9fa;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>User Management</h2>
        <form id="userForm">
            <div class="form-group">
                <label for="name">Name</label>
                <input type="text" id="name" placeholder="Enter Name">
            </div>
            <div class="form-group">
                <label for="address">Address</label>
                <input type="text" id="address" placeholder="Enter Address">
            </div>
            <div class="form-group">
                <label for="mobileNumber">Mobile Number</label>
                <input type="text" id="mobileNumber" placeholder="Enter Mobile Number">
            </div>
            <div class="form-group">
                <label for="email">Email</label>
                <input type="email" id="email" placeholder="Enter Email">
            </div>
            <button type="button" class="btn btn-primary" onclick="registerUser()">Register</button>
            <button type="button" class="btn btn-warning" onclick="patchUser()">Update (Patch)</button>
            <button type="button" class="btn btn-success" onclick="getUser()">Get User</button>
            <button type="button" class="btn btn-danger" onclick="getAllUsers()">Get All Users</button>
        </form>
        <div id="output" class="output"></div>
    </div>

    <script>
        const apiBaseUrl = "http://localhost:8080/api/users"; // Replace with your API base URL

        function registerUser() {
            const user = getUserInput();
            fetch(apiBaseUrl, {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(user),
            })
            .then(response => response.json())
            .then(data => displayOutput(data))
            .catch(err => console.error("Error:", err));
        }

        function patchUser() {
            const user = getUserInput();
            fetch(`${apiBaseUrl}/${user.id}`, { // Replace with appropriate endpoint
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(user),
            })
            .then(response => response.json())
            .then(data => displayOutput(data))
            .catch(err => console.error("Error:", err));
        }

        function getUser() {
            const id = prompt("Enter User ID to fetch details:");
            fetch(`${apiBaseUrl}/${id}`)
            .then(response => response.json())
            .then(data => displayOutput(data))
            .catch(err => console.error("Error:", err));
        }

        function getAllUsers() {
            fetch(apiBaseUrl)
            .then(response => response.json())
            .then(data => displayOutput(data))
            .catch(err => console.error("Error:", err));
        }

        function getUserInput() {
            return {
                name: document.getElementById("name").value,
                address: document.getElementById("address").value,
                mobileNumber: document.getElementById("mobileNumber").value,
                email: document.getElementById("email").value,
            };
        }

        function displayOutput(data) {
            document.getElementById("output").textContent = JSON.stringify(data, null, 2);
        }
    </script>
</body>
</html>
