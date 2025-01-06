<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>User Management and Chat</title>
    <link href="/webjars/bootstrap/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #f8f9fa;
        }

        .container {
            margin-top: 20px;
        }

        .output {
            margin-top: 20px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
            background: #f1f1f1;
            white-space: pre-wrap;
            font-family: monospace;
        }

        #chat-room {
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2 class="text-center">User Management and Chat</h2>

        <!-- User Form -->
        <div id="user-management">
            <form id="userForm">
                <div class="form-group">
                    <label for="name">Name</label>
                    <input type="text" id="name" class="form-control" placeholder="Enter your name">
                </div>
                <div class="form-group">
                    <label for="address">Address</label>
                    <input type="text" id="address" class="form-control" placeholder="Enter your address">
                </div>
                <div class="form-group">
                    <label for="mobileNumber">Mobile Number</label>
                    <input type="text" id="mobileNumber" class="form-control" placeholder="Enter your mobile number">
                </div>
                <div class="form-group">
                    <label for="email">Email</label>
                    <input type="email" id="email" class="form-control" placeholder="Enter your email">
                </div>
                <button type="button" class="btn btn-primary" onclick="registerUser()">Register</button>
                <button type="button" class="btn btn-warning" onclick="updateUser()">Update (Patch)</button>
                <button type="button" class="btn btn-success" onclick="getUser()">Get User</button>
                <button type="button" class="btn btn-danger" onclick="checkUserForChat()">Enter Chat</button>
            </form>

            <div id="output" class="output"></div>
        </div>

        <!-- Chat Room -->
        <div id="chat-room">
            <div class="card mt-3">
                <div class="card-header">
                    <h3>Welcome to the Chat Room</h3>
                </div>
                <div class="card-body">
                    <div class="form-group">
                        <label for="message">Message</label>
                        <input type="text" id="message" class="form-control" placeholder="Enter your message">
                    </div>
                    <button type="button" class="btn btn-dark" id="send-btn">Send</button>
                    <button type="button" class="btn btn-danger" id="logout-btn" onclick="logoutChat()">Logout</button>

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

    <script>
        const registerApi = "http://localhost:8080/user/registerUser";
        const patchApi = "http://localhost:8080/user/";
        const getUserApi = "http://localhost:8080/user/getUserById?userId=";

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

        function getUser() {
            const userId = prompt("Enter the User ID to fetch:");
            if (!userId) return alert("User ID is required!");
            fetch(`${getUserApi}${userId}`)
            .then(response => response.json())
            .then(data => displayOutput(data))
            .catch(err => displayOutput({ error: "Error fetching user", details: err }));
        }

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

        function enterChat(userName) {
            document.getElementById("user-management").style.display = "none";
            document.getElementById("chat-room").style.display = "block";
            document.getElementById("name-title").textContent = `Hello, ${userName}`;
        }

        function logoutChat() {
            document.getElementById("chat-room").style.display = "none";
            document.getElementById("user-management").style.display = "block";
            document.getElementById("output").textContent = "";
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
