<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Update Profile</title>
    
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

        .form-group label {
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="card">
            <div class="card-header bg-dark text-white">
                <h4>Update Your Profile</h4>
            </div>
            <div class="card-body">
                <!-- User ID Input -->
                <div class="form-group">
                    <label for="userId">User ID</label>
                    <input type="text" id="userId" class="form-control" placeholder="Enter your User ID">
                    <button type="button" class="btn btn-primary mt-2" onclick="fetchUserDetails()">Fetch Details</button>
                </div>

                <!-- Profile Form -->
                <form id="updateProfileForm" style="display: none;">
                    <div class="form-group">
                        <label for="name">Name</label>
                        <input type="text" id="name" class="form-control" placeholder="Your Name">
                    </div>
                    <div class="form-group">
                        <label for="address">Address</label>
                        <input type="text" id="address" class="form-control" placeholder="Your Address">
                    </div>
                    <div class="form-group">
                        <label for="mobileNumber">Mobile Number</label>
                        <input type="text" id="mobileNumber" class="form-control" placeholder="Your Mobile Number">
                    </div>
                    <div class="form-group">
                        <label for="email">Email</label>
                        <input type="email" id="email" class="form-control" placeholder="Your Email">
                    </div>
                    <button type="button" class="btn btn-custom btn-block" onclick="updateProfile()">Update Profile</button>
                </form>
                <div id="output" class="mt-3"></div>
            </div>
        </div>
    </div>

    <script>
        const getUserApi = "http://localhost:8080/user/getUserById?userId=";
        const patchApi = "http://localhost:8080/user/";

        // Fetch user details and auto-populate form fields
        function fetchUserDetails() {
            const userId = document.getElementById("userId").value.trim();
            if (!userId) {
                alert("Please enter a valid User ID.");
                return;
            }

            fetch(`${getUserApi}${userId}`)
                .then(response => {
                    if (!response.ok) throw new Error("User not found.");
                    return response.json();
                })
                .then(data => {
                    document.getElementById("updateProfileForm").style.display = "block";
                    document.getElementById("name").value = data.name || "";
                    document.getElementById("address").value = data.address || "";
                    document.getElementById("mobileNumber").value = data.mobileNumber || "";
                    document.getElementById("email").value = data.email || "";
                    document.getElementById("output").textContent = "";
                })
                .catch(error => {
                    document.getElementById("output").textContent = `Error: ${error.message}`;
                    document.getElementById("updateProfileForm").style.display = "none";
                });
        }

        // Update profile details
        function updateProfile() {
            const userId = document.getElementById("userId").value.trim();
            if (!userId) {
                alert("User ID is required to update the profile.");
                return;
            }

            const userDetails = {
                name: document.getElementById("name").value,
                address: document.getElementById("address").value,
                mobileNumber: document.getElementById("mobileNumber").value,
                email: document.getElementById("email").value,
            };

            fetch(`${patchApi}${userId}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(userDetails),
            })
                .then(response => {
                    if (!response.ok) throw new Error("Failed to update profile.");
                    return response.json();
                })
                .then(data => {
                    document.getElementById("output").textContent = "Profile updated successfully!";
                })
                .catch(error => {
                    document.getElementById("output").textContent = `Error: ${error.message}`;
                });
        }
    </script>
</body>
</html>
