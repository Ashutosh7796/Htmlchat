<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Management Form</title>
    <style>
        /* Global Styles */
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f3f4f6;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .container {
            background: #ffffff;
            padding: 20px 30px;
            border-radius: 10px;
            box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 500px;
        }

        h2 {
            text-align: center;
            color: #333333;
            margin-bottom: 20px;
        }

        .form-group {
            display: flex;
            flex-direction: column;
            margin-bottom: 15px;
        }

        label {
            margin-bottom: 5px;
            font-weight: bold;
            color: #555555;
        }

        input {
            padding: 10px;
            border: 1px solid #dddddd;
            border-radius: 5px;
            font-size: 14px;
            outline: none;
        }

        input:focus {
            border-color: #007bff;
            box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
        }

        .btn-container {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
            margin-top: 10px;
        }

        .btn {
            padding: 10px 15px;
            color: #ffffff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
            flex: 1 1 calc(50% - 10px);
            text-align: center;
        }

        .btn-primary { background: #007bff; }
        .btn-warning { background: #ffc107; }
        .btn-success { background: #28a745; }
        .btn-danger { background: #dc3545; }

        .btn:hover {
            opacity: 0.9;
        }

        .output {
            margin-top: 20px;
            padding: 15px;
            border: 1px solid #e0e0e0;
            background: #f8f9fa;
            border-radius: 5px;
            font-family: monospace;
            white-space: pre-wrap;
            color: #333333;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>User Management</h2>
        <form id="userForm">
            <!-- Name -->
            <div class="form-group">
                <label for="name">Name</label>
                <input type="text" id="name" placeholder="Enter your name">
            </div>

            <!-- Address -->
            <div class="form-group">
                <label for="address">Address</label>
                <input type="text" id="address" placeholder="Enter your address">
            </div>

            <!-- Mobile Number -->
            <div class="form-group">
                <label for="mobileNumber">Mobile Number</label>
                <input type="text" id="mobileNumber" placeholder="Enter your mobile number">
            </div>

            <!-- Email -->
            <div class="form-group">
                <label for="email">Email</label>
                <input type="email" id="email" placeholder="Enter your email">
            </div>

            <!-- Buttons -->
            <div class="btn-container">
                <button type="button" class="btn btn-primary" onclick="registerUser()">Register</button>
                <button type="button" class="btn btn-warning" onclick="patchUser()">Update (Patch)</button>
                <button type="button" class="btn btn-success" onclick="getUser()">Get User</button>
                <button type="button" class="btn btn-danger" onclick="getAllUsers()">Get All Users</button>
            </div>
        </form>

        <!-- Output Section -->
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
            .catch(err => displayOutput({ error: "Error registering user", details: err }));
        }

        function patchUser() {
            const user = getUserInput();
            const id = prompt("Enter the ID of the user to update:");
            if (!id) return displayOutput({ error: "User ID is required" });

            fetch(`${apiBaseUrl}/${id}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(user),
            })
            .then(response => response.json())
            .then(data => displayOutput(data))
            .catch(err => displayOutput({ error: "Error updating user", details: err }));
        }

        function getUser() {
            const id = prompt("Enter the ID of the user to fetch:");
            if (!id) return displayOutput({ error: "User ID is required" });

            fetch(`${apiBaseUrl}/${id}`)
            .then(response => response.json())
            .then(data => displayOutput(data))
            .catch(err => displayOutput({ error: "Error fetching user", details: err }));
        }

        function getAllUsers() {
            fetch(apiBaseUrl)
            .then(response => response.json())
            .then(data => displayOutput(data))
            .catch(err => displayOutput({ error: "Error fetching users", details: err }));
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
