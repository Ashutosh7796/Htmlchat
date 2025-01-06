<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chat Room</title>

    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        /* Custom Styles */
        body {
            background-color: #343a40;
            color: white;
        }
        .typing-indicator {
            font-style: italic;
            color: #ffcc00;
        }
        .card {
            background-color: #495057;
        }
    </style>
</head>
<body>

<!-- Login Section -->
<div id="name-form" class="bg-primary d-flex align-items-center vh-100">
    <div class="container">
        <div class="row">
            <div class="col-md-4 offset-md-4">
                <div class="input-group">
                    <input type="text" placeholder="Enter your name" id="name-value" autofocus class="form-control">
                    <button class="btn btn-dark" id="login">Enter</button>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- Chat Room Section -->
<div id="chat-room" class="d-none">
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-6 offset-md-3">
                <div class="card">
                    <div class="card-header">
                        <h3 id="name-title"></h3>
                    </div>
                    <div class="card-body">
                        <!-- Typing Indicator -->
                        <p id="typing-indicator" class="typing-indicator"></p>

                        <!-- Message Input -->
                        <div class="input-group mb-3">
                            <input type="text" placeholder="Enter your message" id="message" class="form-control">
                            <button class="btn btn-dark" id="send-btn">Send</button>
                            <button class="btn btn-dark" id="logout">Logout</button>
                        </div>

                        <!-- Messages Table -->
                        <div class="table-responsive">
                            <table id="message-container-table" class="table table-bordered table-dark"></table>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- JS Libraries -->
<script src="https://cdn.jsdelivr.net/npm/sockjs-client/dist/sockjs.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/stompjs/lib/stomp.min.js"></script>

<!-- Script -->
<script>
    // WebSocket and STOMP client setup
    let stompClient = null;
    let username = null;
    let typingTimeout;

    // Elements
    const nameForm = document.getElementById("name-form");
    const chatRoom = document.getElementById("chat-room");
    const nameValue = document.getElementById("name-value");
    const nameTitle = document.getElementById("name-title");
    const messageInput = document.getElementById("message");
    const messageTable = document.getElementById("message-container-table");
    const typingIndicator = document.getElementById("typing-indicator");

    // Connect to WebSocket
    function connect() {
        const socket = new SockJS("/chat");
        stompClient = Stomp.over(socket);
        stompClient.connect({}, () => {
            console.log("Connected to WebSocket");

            // Subscribe to topic for messages
            stompClient.subscribe("/topic/return-to", (message) => {
                displayMessage(JSON.parse(message.body));
            });

            // Subscribe to topic for typing indicator
            stompClient.subscribe("/topic/typing", (message) => {
                typingIndicator.textContent = message.body;
                setTimeout(() => (typingIndicator.textContent = ""), 1000);
            });
        });
    }

    // Send message
    document.getElementById("send-btn").addEventListener("click", () => {
        const messageContent = messageInput.value;
        if (messageContent && stompClient) {
            const message = { content: messageContent, sender: username };
            stompClient.send("/app/message", {}, JSON.stringify(message));
            messageInput.value = "";
        }
    });

    // Typing indicator
    messageInput.addEventListener("input", () => {
        if (stompClient && username) {
            clearTimeout(typingTimeout);
            stompClient.send("/app/typing", {}, username + " is typing...");
            typingTimeout = setTimeout(() => stompClient.send("/app/typing", {}, ""), 1000);
        }
    });

    // Display message
    function displayMessage(message) {
        const row = messageTable.insertRow();
        const cell = row.insertCell(0);
        cell.textContent = `${message.sender}: ${message.content}`;
    }

    // Login
    document.getElementById("login").addEventListener("click", () => {
        username = nameValue.value.trim();
        if (username) {
            nameTitle.textContent = `Welcome, ${username}`;
            nameForm.classList.add("d-none");
            chatRoom.classList.remove("d-none");
            connect();
        }
    });

    // Logout
    document.getElementById("logout").addEventListener("click", () => {
        if (stompClient) stompClient.disconnect();
        nameForm.classList.remove("d-none");
        chatRoom.classList.add("d-none");
        username = null;
        messageTable.innerHTML = "";
    });
</script>
</body>
</html>

