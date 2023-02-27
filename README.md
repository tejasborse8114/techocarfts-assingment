# techocarfts-assingment



<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>User Registration Form</title>
    <style>
      input[type=text], input[type=password] {
        width: 100%;
        padding: 12px 20px;
        margin: 8px 0;
        display: inline-block;
        border: 1px solid #ccc;
        box-sizing: border-box;
      }
      button {
        background-color: #4CAF50;
        color: white;
        padding: 14px 20px;
        margin: 8px 0;
        border: none;
        cursor: pointer;
        width: 100%;
      }
      button:hover {
        opacity: 0.8;
      }
      .container {
        padding: 16px;
      }
    </style>
  </head>
  <body>
    <form>
      <div class="container">
        <label for="email"><b>Email</b></label>
        <input type="text" placeholder="Enter Email" name="email" id="email" required>

        <label for="psw"><b>Password</b></label>
        <input type="password" placeholder="Enter Password" name="psw" id="psw" required>

        <button type="submit" id="submit-btn">Register</button>
      </div>
    </form>

    <script>
      const form = document.querySelector('form');
      const emailInput = document.querySelector('#email');
      const passwordInput = document.querySelector('#psw');

      form.addEventListener('submit', event => {
        event.preventDefault();

        // Validate email and password inputs
        if (emailInput.value.trim() === '' || passwordInput.value.trim() === '') {
          alert('Email and password are required');
          return;
        }

        if (!validateEmail(emailInput.value)) {
          alert('Please enter a valid email address');
          return;
        }

        // Send data to server app
        const data = {
          email: emailInput.value.trim(),
          password: passwordInput.value.trim()
        };

        fetch('/register', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(data)
        })
        .then(response => response.json())
        .then(data => {
          if (data.success) {
            alert('Registration complete');
          } else {
            alert(data.error);
          }
        })
        .catch(error => {
          alert('An error occurred: ' + error.message);
        });
      });

      function validateEmail(email) {
        const re = /\S+@\S+\.\S+/;
        return re.test(email);
      }
    </script>
  </body>
</html>







package main

import (
	"encoding/json"
	"net/http"
)

type registrationData struct {
	Email    string `json:"email"`
	Password string `json:"password"`
}

func main() {
	http.HandleFunc("/register", registerHandler)
	http.ListenAndServe(":8080", nil)
}

func registerHandler(w http.ResponseWriter, r *http
