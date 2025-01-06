<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Profile Page</title>
    
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" rel="stylesheet">

    <style>
        body {
            background-color: #f8f9fa;
            font-family: Arial, sans-serif;
        }

        .container {
            margin-top: 50px;
            max-width: 600px;
        }

        .card {
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }

        .btn-custom {
            background-color: #007bff;
            color: white;
            font-weight: bold;
        }

        .btn-custom:hover {
            background-color: #0056b3;
        }

        .welcome {
            font-size: 1.5rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="card">
            <div class="card-header bg-dark text-white">
                <div class="d-flex justify-content-between align-items-center">
                    <span class="welcome">Welcome, <span id="user-name">Loading...</span></span>
                    <button class="btn btn-sm btn-danger" onclick="logout()">Logout</button>
                </div>
            </div>
            <div class="card-body">
                <p class="text-center">Choose an option:</p>
                <div class="d-flex justify-content-around">
                    <a href="update-profile.html" class="btn btn-custom">Update Profile</a>
                    <a href="chat-room.html" class="btn btn-custom">Chat Room</a>
                </div>
            </div>
        </div>
    </div>

    <script>
        const getUserApi = "http://localhost:8080/user/getUserById?userId="; // Update this to match your API endpoint

        // Fetch user details and display the name
        function fetchUserName() {
            // Assume the user ID is stored in localStorage after registration
            const userId = localStorage.getItem("userId");
            if (!userId) {
                document.getElementById("user-name").textContent = "Guest";
                return;
            }

            fetch(`${getUserApi}${userId}`)
                .then(response => {
                    if (!response.ok) throw new Error("User not found.");
                    return response.json();
                })
                .then(data => {
                    document.getElementById("user-name").textContent = data.name || "User";
                })
                .catch(error => {
                    document.getElementById("user-name").textContent = "Guest";
                    console.error("Error fetching user details:", error.message);
                });
        }

        // Logout functionality (optional)
        function logout() {
            localStorage.removeItem("userId"); // Clear user data
            window.location.href = "register.html"; // Redirect to the register page
        }

        // Fetch the user's name on page load
        fetchUserName();
    </script>
</body>
</html>
