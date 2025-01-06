function fetchUserDetails() {
    const userId = document.getElementById("userId").value.trim();
    if (!userId) {
        alert("Please enter a valid User ID.");
        return;
    }

    fetch(`${getUserApi}${userId}`)
        .then(response => {
            if (!response.ok) {
                return response.text().then(err => {
                    throw new Error(`Error: ${err}`);
                });
            }
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
            document.getElementById("output").textContent = error.message;
            document.getElementById("updateProfileForm").style.display = "none";
        });
}

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
            if (!response.ok) {
                return response.text().then(err => {
                    throw new Error(`Error: ${err}`);
                });
            }
            return response.json();
        })
        .then(data => {
            document.getElementById("output").textContent = "Profile updated successfully!";
        })
        .catch(error => {
            document.getElementById("output").textContent = error.message;
        });
}
