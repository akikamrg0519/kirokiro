<template>
  <div class="app">
    <LoginForm v-if="!loggedIn" @login="handleLogin" />
    <div v-else class="welcome">
      <h2>ようこそ、{{ username }} さん！</h2>
      <button @click="handleLogout">ログアウト</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import LoginForm from './components/LoginForm.vue'

const loggedIn = ref(false)
const username = ref('')

function handleLogin(user) {
  loggedIn.value = true
  username.value = user
}

function handleLogout() {
  loggedIn.value = false
  username.value = ''
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Segoe UI', sans-serif;
  background: #f0f2f5;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.app {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.welcome {
  text-align: center;
}

.welcome h2 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
  color: #333;
}

.welcome button {
  padding: 0.5rem 1.5rem;
  background: #4f46e5;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.9rem;
}

.welcome button:hover {
  background: #4338ca;
}
</style>
