<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Register</title>
    
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" rel="stylesheet">

    <style>
        body {
            background-color: #f8f9fa;
            font-family: Arial, sans-serif;
        }

        .container {
            margin-top: 50px;
            max-width: 500px;
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

        .form-group label {
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="card">
            <div class="card-header bg-dark text-white">
                <h4>Register</h4>
            </div>
            <div class="card-body">
                <!-- Registration Form -->
                <form id="registerForm">
                    <div class="form-group">
                        <label for="name">Name</label>
                        <input type="text" id="name" class="form-control" placeholder="Enter your name" required>
                    </div>
                    <div class="form-group">
                        <label for="email">Email</label>
                        <input type="email" id="email" class="form-control" placeholder="Enter your email" required>
                    </div>
                    <div class="form-group">
                        <label for="mobileNumber">Mobile Number</label>
                        <input type="text" id="mobileNumber" class="form-control" placeholder="Enter your mobile number" required>
                    </div>
                    <div class="form-group">
                        <label for="address">Address</label>
                        <input type="text" id="address" class="form-control" placeholder="Enter your address" required>
                    </div>
                    <button type="button" class="btn btn-custom btn-block" onclick="registerUser()">Register</button>
                </form>
                <div id="output" class="mt-3 text-center"></div>
            </div>
        </div>
    </div>

    <script>
        const registerApi = "http://localhost:8080/user/registerUser"; // Update to match your API endpoint
        const profilePageUrl = "profile-page.html"; // Redirect URL for the profile page

        function registerUser() {
            const user = {
                name: document.getElementById("name").value.trim(),
                email: document.getElementById("email").value.trim(),
                mobileNumber: document.getElementById("mobileNumber").value.trim(),
                address: document.getElementById("address").value.trim(),
            };

            fetch(registerApi, {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(user),
            })
            .then(response => {
                if (!response.ok) throw new Error("Registration failed");
                return response.json();
            })
            .then(data => {
                // Assuming the backend returns success or user details
                document.getElementById("output").textContent = "Registration successful!";
                setTimeout(() => {
                    window.location.href = profilePageUrl;
                }, 1000);
            })
            .catch(err => {
                document.getElementById("output").textContent = `Error: ${err.message}`;
            });
        }
    </script>
</body>
</html>
