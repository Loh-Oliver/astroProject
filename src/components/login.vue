<template>
  <div>
    <header>
      <div class="login_header">Swag Labs</div>
    </header>
    <main>
      <div class="login_container">
        <form @submit.prevent="login">
          <div class="form_group username_group">
            <input
              type="text"
              id="username"
              v-model="data.username"
              :style="{ borderColor: inputBorderColor }"
              placeholder="Username"
              autocomplete="username"
            />
            <img
              v-if="inputBorderColor === 'red'"
              src="src/assets/redcross.svg"
              alt="Red Border"
            />
          </div>
          <div class="form group password_group">
            <input
              type="password"
              id="password"
              v-model="data.password"
              :style="{ borderColor: inputBorderColor }"
              placeholder="Password"
              autocomplete="current-password"
            />
            <img
              v-if="inputBorderColor === 'red'"
              src="src/assets/redcross.svg"
              alt="Red Border"
            />
          </div>
          <p class="error_msg" v-if="data.errorMessage">
            {{ data.errorMessage }}
            <button class="close_btn" @click="clearErrorMessage">x</button>
          </p>

          <button type="submit" class="login_btn">Login</button>
        </form>
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
import { reactive } from "vue";

interface Data {
  username: string;
  password: string;
  errorMessage: string;
}

const data: Data = reactive({
  username: "",
  password: "",
  errorMessage: "",
});

let inputBorderColor = "";

const login = () => {
  if (data.username === "standard_user" && data.password === "secret_sauce") {
    // Redirect to home page if credentials match
    window.location.href = "/shop";
  } else if (
    data.username !== "standard_user" &&
    data.password === "secret_sauce"
  ) {
    data.errorMessage = "Epic sadface: Username is incorrect";
    inputBorderColor = "red";
  } else if (
    data.username === "standard_user" &&
    data.password !== "secret_sauce"
  ) {
    data.errorMessage = "Epic sadface: Password is incorrect";
    inputBorderColor = "red";
  } else {
    data.errorMessage =
      "Epic sadface: Username and password do not match any user in this service";
    inputBorderColor = "red";
  }
};

const clearErrorMessage = () => {
  data.errorMessage = "";
  inputBorderColor = "";
};
</script>

<style scoped>
h4 {
  font-size: 18px;
}
.login_container {
  max-width: 50%;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #fff;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  color: #eefcf6;
  padding-top: 50px;
  height: 100%;
}

input[type="text"],
input[type="password"] {
  border: none;
  border-bottom: 1px solid #ccc;
  padding: 10px 0px;
  width: 50%;
  margin: 0 25%;
}

.login_btn {
  width: 50%;
  padding: 10px;
  background-color: #3ddc91;
  color: #132322;
  border: none;
  height: 50px;
  font-size: 17px;
  border-radius: 5px;
  cursor: pointer;
  margin: 0 25%;
}

.login_btn:hover {
  background-color: #45a049;
}
.login_credentials {
  display: flex;
  gap: 20%;
  max-width: 50%;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #000;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  color: #fff;
}

.login_usernames span {
  display: block;
  margin-bottom: -10px;
}

header {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  margin-top: 20px;
  margin-bottom: 60px;
  font-family: "DM Mono", sans-serif;
}

.form_group {
  margin-bottom: 15px;
  display: flex;
}
.username_group img {
  color: #e2231a;
  font-size: 18px;
  position: fixed;
  right: 38%;
  top: 167px;
  height: 25px;
  width: 25px;
}

.password_group img {
  color: #e2231a;
  font-size: 18px;
  position: absolute;
  right: 38%;
  top: 215px;
  height: 25px;
  width: 25px;
}

.error_msg {
  width: 50%;
  background-color: red;
  color: white;
  height: 50px;
  margin: 10px 25%;
}

.close_btn {
  position: fixed;
  top: 255px;
  right: 37%;
  padding: 5px 10px;
  font-size: 15px;
  background-color: transparent;
  color: white;
  border: none;
  cursor: pointer;
}

@media screen and (max-width: 700px) {
  .login_credentials {
    display: block;
  }
}
</style>
