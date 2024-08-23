<template>
  <div class="login-page">
    <div class="card">
      <div class="card-fundo">
        <!-- Formulário de Login -->
      <form v-if="!isSignup" @submit.prevent="login">
        <div class="title">Login</div>
        <input placeholder="Username" type="text" v-model="username" />
        <br />
        <input placeholder="Password" type="password" v-model="password" />
        <br />
        <button class="login-btn" type="submit">Login</button>
      </form>
      <!-- Formulário de Sign Up -->
      <form v-if="isSignup" @submit.prevent="signup">
        <div class="title">Sign Up</div>
        <input placeholder="Username" type="text" v-model="username" />
        <br />
        <input placeholder="Password" type="password" v-model="password" />
        <br />
        <input placeholder="Email" type="text" v-model="email" />
        <br />
        <input placeholder="First name" type="text" v-model="first_name" />
        <br />
        <input placeholder="Last name" type="text" v-model="last_name" />
        <br />
        <button class="sing-btn" type="submit">Sign Up</button>
      </form>
            <!-- Botão para alternar entre Login e Sign Up -->
      <button class="sing-btn2" @click="toggleForm">{{ isSignup ? 'Go to Login' : 'Go to Sign Up' }}</button>
      </div>
    </div>
  </div>
</template>

<script>
import { loginRest, signupRest } from "./api";

export default {
  data() {
    return {
      username: "",
      password: "",
      email: "",
      first_name: "",
      last_name: "",
      isSignup: false, // Adiciona uma variável para controlar o estado do formulário
    };
  },
  methods: {
    login() {
      loginRest(this.username, this.password)
        .then((response) =>
          this.$emit("onAuth", { ...response.data, secret: this.password })
        )
        .catch((error) => console.log("Login error", error));
    },
    signup() {
      signupRest(
        this.username,
        this.password,
        this.email,
        this.first_name,
        this.last_name
      )
        .then((response) =>
          this.$emit("onAuth", { ...response.data, secret: this.password })
        )
        .catch((error) => console.log("Sign up error", error));
    },
    toggleForm() {
      this.isSignup = !this.isSignup;
    },
  },
};
</script>
