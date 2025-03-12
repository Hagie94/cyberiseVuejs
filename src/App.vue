<template>
  <div class="login-container" v-if="!loggedIn">
    <div class="logo-container">
      <h2>Connexion</h2>
    </div>
    <form @submit.prevent="login">
      <div class="form-group">
        <label for="email">Email</label>
        <input type="email" id="email" v-model="email" required />
      </div>
      <div class="form-group">
        <label for="password">Password</label>
        <input type="password" id="password" v-model="password" required />
      </div>
      <button type="submit">Se connecter</button>
    </form>
  </div>
  <router-view v-else />
</template>

<script>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

export default {
  setup() {
    const email = ref('');
    const password = ref('');
    const loggedIn = ref(false);
    const router = useRouter();

    const login = () => {
      if (email.value === "admin@cyberise" && password.value === "adminway") {
        loggedIn.value = true;
        router.push('/dashboard');
      } else {
        alert('Email ou mot de passe incorrect');
      }
    };

    return {
      email,
      password,
      loggedIn,
      login
    };
  }
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  border: none; /* Remove all borders */
}

body {
  background-color: #121212;
  color: #ffffff;
  font-family: Arial, sans-serif;
}

.login-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: #121212;
  color: #ffffff;
}

.logo-container {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

h2 {
  margin: 0;
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
  padding: 10px;
  border: none;
  border-radius: 5px;
  background-color: #333333;
  color: #ffffff;
}

button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #227a61;
  color: #ffffff;
  cursor: pointer;
}

button:hover {
  background-color: #1565c0;
}
</style>