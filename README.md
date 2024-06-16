<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Secure Authentication System</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 50px;
        }
        .auth-form {
            max-width: 300px;
            margin: auto;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
        }
        input {
            width: 100%;
            padding: 8px;
            box-sizing: border-box;
        }
        button {
            width: 100%;
            padding: 8px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>

<div class="auth-form">
    <h1>Login</h1>
    <div class="form-group">
        <label for="username">Username:</label>
        <input type="text" id="username" required>
    </div>
    <div class="form-group">
        <label for="password">Password:</label>
        <input type="password" id="password" required>
    </div>
    <button type="button" onclick="login()">Login</button>
</div>

<script>
    async function login() {
        const username = document.getElementById('username').value;
        const password = document.getElementById('password').value;

        const hashedPassword = await hashPassword(password);

        const response = await fetch('https://your-backend-url.onrender.com/login', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ username, password: hashedPassword })
        });

        const result = await response.json();
        if (result.success) {
            alert('Login successful!');
        } else {
            alert('Login failed: ' + result.message);
        }
    }

    async function hashPassword(password) {
        const encoder = new TextEncoder();
        const data = encoder.encode(password);
        const hashBuffer = await crypto.subtle.digest('SHA-256', data);
        const hashArray = Array.from(new Uint8Array(hashBuffer));
        return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
    }
</script>

</body>
</html>


