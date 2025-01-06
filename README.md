<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>User Management and Chat</title>
    
    <!-- Link to Bootstrap for styling -->
    <link href="/webjars/bootstrap/css/bootstrap.min.css" rel="stylesheet">

    <style>
        /* Body styles */
        body {
            background-color: #f8f9fa; /* Light background color */
            font-family: Arial, sans-serif; /* Default font */
        }

        /* Centering and spacing container */
        .container {
            margin-top: 30px;
            max-width: 600px;
        }

        /* Styling for the output box */
        .output {
            margin-top: 20px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
            background: #f1f1f1;
            white-space: pre-wrap;
            font-family: monospace; /* For JSON output readability */
        }

        /* Chat room styles */
        #chat-room {
            display: none; /* Initially hidden */
        }

        /* Form input fields */
        .form-control {
            margin-bottom: 15px;
        }

        /* Custom buttons */
        .btn {
            margin-right: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <h2 class="text-center mb-4">User Management and Chat</h2>

        <!-- User Management Section -->
        <div id="user-management">
            <form id="userForm">
                <!-- Name Input -->
                <div class="form-group">
                    <label for="name">Name</label>
                    <input type="text" id="name" class="form-control" placeholder="Enter your name">
                </div>

                <!-- Address Input -->
                <div class="form-group">
                    <label for="address">Address</label>
                    <input type="text" id="address" class="form-control" placeholder="Enter your address">
                </div>

                <!-- Mobile Number Input -->
                <div class="form-group">
                    <label for="mobileNumber">Mobile Number</label>
                    <input type="text" id="mobileNumber" class="form-control" placeholder="Enter your mobile number">
                </div>

                <!-- Email Input -->
                <div class="form-group">
                    <label for="email">Email</label>
                    <input type="email" id="email" class="form-control" placeholder="Enter your email">
                </div>

                <!-- Buttons -->
                <button type="button" class="btn btn-primary" onclick="registerUser()">Register</button>
                <button type="button" class="btn btn-warning" onclick="updateUser()">Update (Patch)</button>
                <button type="button" class="btn btn-success" onclick="getUser()">Get User</button>
                <button type="button" class="btn btn-danger" onclick="checkUserForChat()">Enter Chat</button>
            </form>

            <!-- Output Section -->
            <div id="output" class="output"></div>
        </div>

        <!-- Chat Room Section -->
        <div id="chat-room">
            <div class="card mt-4">
                <div class="card-header bg-dark text-white">
                    <h4 class="mb-0">Welcome to the Chat Room</h4>
                </div>
                <div class="card-body">
                    <!-- Message Input -->
                    <div class="form-group">
                        <label for="message">Message</label>
                        <input type="text" id="message" class="form-control" placeholder="Enter your message">
                    </div>

                    <!-- Chat Buttons -->
                    <button type="button" class="btn btn-dark" id="send-btn">Send</button>
                    <button type="button" class="btn btn-danger" id="logout-btn" onclick="logoutChat()">Logout</button>

                    <!-- Messages Display -->
                    <div class="mt-3">
                        <table id="message-container-table" class="table table-bordered">
                            <thead>
                                <tr>
                                    <th>Messages</th>
                                </tr>
                            </thead>
                            <tbody></tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- JavaScript for API Calls and Functionalities -->
    <script>
        // API Endpoints
        const registerApi = "http://localhost:8080/user/registerUser";
        const patchApi = "http://localhost:8080/user/";
        const getUserApi = "http://localhost:8080/user/getUserById?userId=";

        // Register User
        function registerUser() {
            const user = getUserInput();
            fetch(registerApi, {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(user),
            })
            .then(response => response.json())
            .then(data => displayOutput(data))
            .catch(err => displayOutput({ error: "Error registering user", details: err }));
        }

        // Update User
        function updateUser() {
            const userId = prompt("Enter the User ID to update:");
            if (!userId) return alert("User ID is required!");
            const user = getUserInput();
            fetch(`${patchApi}${userId}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(user),
            })
            .then(response => response.json())
            .then(data => displayOutput(data))
            .catch(err => displayOutput({ error: "Error updating user", details: err }));
        }

        // Get User
        function getUser() {
            const userId = prompt("Enter the User ID to fetch:");
            if (!userId) return alert("User ID is required!");
            fetch(`${getUserApi}${userId}`)
            .then(response => response.json())
            .then(data => displayOutput(data))
            .catch(err => displayOutput({ error: "Error fetching user", details: err }));
        }

        // Check User for Chat
        function checkUserForChat() {
            const userId = prompt("Enter your User ID to join the chat:");
            if (!userId) return alert("User ID is required!");
            fetch(`${getUserApi}${userId}`)
            .then(response => {
                if (!response.ok) throw new Error("User not found");
                return response.json();
            })
            .then(data => {
                if (data && data.name) {
                    enterChat(data.name);
                } else {
                    alert("User not found or invalid data!");
                }
            })
            .catch(err => displayOutput({ error: "Cannot join chat", details: err.message }));
        }

        // Enter Chat
        function enterChat(userName) {
            document.getElementById("user-management").style.display = "none";
            document.getElementById("chat-room").style.display = "block";
            document.getElementById("name-title").textContent = `Hello, ${userName}`;
        }

        // Logout from Chat
        function logoutChat() {
            document.getElementById("chat-room").style.display = "none";
            document.getElementById("user-management").style.display = "block";
            document.getElementById("output").textContent = "";
        }

        // Get User Input
        function getUserInput() {
            return {
                name: document.getElementById("name").value,
                address: document.getElementById("address").value,
                mobileNumber: document.getElementById("mobileNumber").value,
                email: document.getElementById("email").value,
            };
        }

        // Display Output
        function displayOutput(data) {
            document.getElementById("output").textContent = JSON.stringify(data, null, 2);
        }
    </script>
</body>
</html>
