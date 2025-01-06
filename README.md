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
            background-color: #e9ecef;
            font-family: Arial, sans-serif;
        }

        .profile-header {
            background: linear-gradient(135deg, #007bff, #6610f2);
            color: white;
            padding: 30px 20px;
            text-align: center;
            border-radius: 10px 10px 0 0;
        }

        .profile-header h2 {
            margin: 0;
            font-size: 2.5rem;
        }

        .profile-header p {
            margin: 5px 0 0;
            font-size: 1rem;
        }

        .profile-options {
            background: white;
            border: 1px solid #ddd;
            padding: 20px;
            border-radius: 0 0 10px 10px;
        }

        .profile-options .btn {
            display: block;
            width: 100%;
            margin: 10px 0;
            font-size: 1.1rem;
            font-weight: bold;
            border-radius: 5px;
        }

        .btn-update {
            background-color: #007bff;
            color: white;
        }

        .btn-update:hover {
            background-color: #0056b3;
        }

        .btn-chat {
            background-color: #28a745;
            color: white;
        }

        .btn-chat:hover {
            background-color: #1e7e34;
        }

        .btn-logout {
            background-color: #dc3545;
            color: white;
        }

        .btn-logout:hover {
            background-color: #c82333;
        }
    </style>
</head>
<body>
    <div class="container mt-5">
        <div class="profile-header">
            <h2>Welcome, [User's Name]</h2>
            <p>Manage your profile or join the chat!</p>
        </div>

        <div class="profile-options">
            <button onclick="goToUpdateProfile()" class="btn btn-update">Update Profile Details</button>
            <button onclick="goToChatRoom()" class="btn btn-chat">Click to Chat</button>
            <button onclick="logout()" class="btn btn-logout">Logout</button>
        </div>
    </div>

    <script>
        // Placeholder: Replace with actual navigation links or logic
        function goToUpdateProfile() {
            window.location.href = "update-profile.html"; // Update Profile Page
        }

        function goToChatRoom() {
            window.location.href = "chat-room.html"; // Chat Room Page
        }

        function logout() {
            alert("You have been logged out!");
            window.location.href = "index.html"; // Redirect to Home Page
        }
    </script>
</body>
</html>
