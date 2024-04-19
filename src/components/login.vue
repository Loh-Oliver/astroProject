<template>
    <div>
      <header>
        <div class="login_header">Swag Labs</div>
      </header>
      <main>
        <div class="login_container">
          <form @submit.prevent="login"> 
            <div class="form_group">
              <input
                type="text"
                id="username"
                v-model="data.username"
                placeholder="Username"
                autocomplete="username"
                
              />
            </div>
            <div class="form_group">
              <input
                type="password"
                id="password"
                v-model="data.password"
                placeholder="Password"
                autocomplete="current-password"
                
              />
            </div>
            <button type="submit" class="login_btn">Login</button>
          </form>
          <p class="error_msg">{{ data.errorMessage }}</p>
        </div>
  
        <div class="login_credentials">
          <div class="login_usernames">
            <h4>Accepted usernames are:</h4>
            <span>standard_user</span><br />
            <span>locked_out_user</span><br />
            <span>problem_user</span><br />
            <span>performance_glitch_user</span><br />
            <span>error_user</span><br />
            <span>visual_user</span><br />
          </div>
          <div class="login_passwords">
            <h4>Password for all users:</h4>
            <span>secret_sauce</span><br />
          </div>
        </div>
      </main>
    </div>
  </template>
  
  <script setup lang="ts">
  import { reactive } from 'vue';
  
  interface Data {
    username: string;
    password: string;
    errorMessage: string;
  }
  
  const data: Data = reactive({
    username: "", 
    password: "", 
    errorMessage: "Epic sadface: You can only access '/inventory.html' when you are logged in.",
  });
  
  
  const login = () => {
    if (data.username === "standard_user" && data.password === "secret_sauce") {
      // Redirect to another page if credentials match
      window.location.href = '/shop';
    } else if (data.username !== "standard_user" && data.password === "secret_sauce") {
      data.errorMessage = "Epic sadface: Username is incorrect";
    } else if (data.username === "standard_user" && data.password !== "secret_sauce") {
      data.errorMessage = "Epic sadface: Password is incorrect";
    } else {
      data.errorMessage = "Epic sadface: Username and password do not match any user in this service";
    }
  };
  
  </script>
  
  
  <style scoped>
  .login_container {
    max-width: 50%;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #fff;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  }
  
  .login_credentials {
    max-width: 50%;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #000;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
    color: #fff;
  }
  
  header {
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    margin-top: 20px;
    margin-bottom: 20px;
  }
  
  .form_group {
    margin-bottom: 15px;
  }
  
  label {
    font-weight: bold;
  }
  
  input[type="text"],
  input[type="password"] {
    border: none;
    border-bottom: 1px solid #ccc;
    padding: 8px;
    width: 100%;
    box-sizing: border-box; /* Ensure padding doesn't affect the width */
  }
  
  .login_btn {
    width: 100%;
    padding: 10px;
    background-color: #4caf50;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  
  .login_btn:hover {
    background-color: #45a049;
  }
  
  .error_msg {
    background-color: red;
    color: white;
    padding: 10px;
    margin-top: 5px;
  }
  </style>
  