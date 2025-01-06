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
            // Populate the form fields
            document.getElementById("updateProfileForm").style.display = "block";
            document.getElementById("name").value = data.name || "";
            document.getElementById("address").value = data.address || "";
            document.getElementById("mobileNumber").value = data.mobileNumber || "";
            document.getElementById("email").value = data.email || "";

            // Display the user's name in the header
            document.getElementById("user-name-display").textContent = data.name || "User";

            // Clear error output
            document.getElementById("output").textContent = "";
        })
        .catch(error => {
            document.getElementById("output").textContent = `Error: ${error.message}`;
            document.getElementById("updateProfileForm").style.display = "none";

            // Clear the user's name if there's an error
            document.getElementById("user-name-display").textContent = "";
        });
}
