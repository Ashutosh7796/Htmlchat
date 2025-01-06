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
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="card">
            <div class="card-header bg-dark text-white">
                <h4>Welcome to Your Profile</h4>
            </div>
            <div class="card-body">
                <p>Here are your options:</p>
                <ul class="list-group">
                    <li class="list-group-item">
                        <a href="update-profile.html" class="btn btn-link">Update Profile Details</a>
                    </li>
                    <li class="list-group-item">
                        <a href="chat-room.html" class="btn btn-link">Click to Chat</a>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</body>
</html>
