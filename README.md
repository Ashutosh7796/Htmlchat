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

        /* Header Section */
        .profile-header {
            background: #343a40;
            color: white;
            padding: 20px;
            display: flex;
            align-items: center;
            border-radius: 10px 10px 0 0;
        }

        .profile-header img {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            margin-right: 20px;
        }

        .profile-header h2 {
            margin: 0;
            font-size: 1.8rem;
        }

        .profile-header p {
            margin: 5px 0 0;
            font-size: 1rem;
            color: #d1d1d1;
        }

        /* Navigation Bar */
        .nav-bar {
            background: #007bff;
            color: white;
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-radius: 0 0 10px 10px;
        }

        .nav-bar a {
            color: white;
            margin: 0 10px;
            text-decoration: none;
            font-weight: bold;
        }

        .nav-bar a:hover {
            text-decoration: underline;
        }

        /* Main Content */
        .profile-content {
            margin-top: 20px;
            padding: 20px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }

        .fun-section {
            margin-top: 20px;
            padding: 20px;
            background: #e9ecef;
            border-radius: 10px;
            display: flex;
            justify-content: space-between;
        }

        .fun-section div {
            flex: 1;
            text-align: center;
            margin: 0 10px;
        }

        .fun-section div img {
            width: 100px;
            height: 100px;
            margin-bottom: 10px;
        }

        .fun-section div p {
            font-weight: bold;
            font-size: 1.1rem;
        }
    </style>
</head>
<body>
    <div class="container mt-5">
        <!-- Header Section -->
        <div class="profile-header">
            <img src="https://via.placeholder.com/80" alt="User Profile">
            <div>
                <h2>Welcome, [User's Name]</h2>
                <p>Manage your profile or connect with others!</p>
            </div>
        </div>

        <!-- Navigation Bar -->
        <div class="nav-bar">
            <div>
                <a href="update-profile.html">Update Profile</a>
                <a href="chat-room.html">Chat Room</a>
            </div>
            <a href="index.html" onclick="logout()">Logout</a>
        </div>

        <!-- Profile Content -->
        <div class="profile-content">
            <h4>Profile Overview</h4>
            <p>This section can display user-specific data like recent activity, status updates, or other details. You can customize it based on your application's functionality.</p>
        </div>

        <!-- Fun Section -->
        <div class="fun-section">
            <div>
                <img src="https://via.placeholder.com/100" alt="Feature Icon">
                <p>Achievements</p>
            </div>
            <div>
                <img src="https://via.placeholder.com/100" alt="Feature Icon">
                <p>Friends</p>
            </div>
            <div>
                <img src="https://via.placeholder.com/100" alt="Feature Icon">
                <p>Activity</p>
            </div>
        </div>
    </div>

    <script>
        // Placeholder logout function
        function logout() {
            alert("You have been logged out!");
        }
    </script>
</body>
</html>
