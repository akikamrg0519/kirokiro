<template>
  <div class="card">
    <h1 class="title">ログイン</h1>

    <form @submit.prevent="submit">
      <div class="field">
        <label for="email">メールアドレス</label>
        <input
          id="email"
          v-model="email"
          type="email"
          placeholder="example@email.com"
          autocomplete="email"
          :class="{ error: errors.email }"
        />
        <span v-if="errors.email" class="error-msg">{{ errors.email }}</span>
      </div>

      <div class="field">
        <label for="password">パスワード</label>
        <div class="password-wrapper">
          <input
            id="password"
            v-model="password"
            :type="showPassword ? 'text' : 'password'"
            placeholder="パスワードを入力"
            autocomplete="current-password"
            :class="{ error: errors.password }"
          />
          <button type="button" class="toggle-pw" @click="showPassword = !showPassword">
            {{ showPassword ? '🙈' : '👁️' }}
          </button>
        </div>
        <span v-if="errors.password" class="error-msg">{{ errors.password }}</span>
      </div>

      <div class="options">
        <label class="remember">
          <input type="checkbox" v-model="remember" />
          ログイン状態を保持する
        </label>
        <a href="#" class="forgot">パスワードを忘れた方</a>
      </div>

      <p v-if="loginError" class="login-error">{{ loginError }}</p>

      <button type="submit" class="submit-btn" :disabled="loading">
        <span v-if="loading">ログイン中...</span>
        <span v-else>ログイン</span>
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const emit = defineEmits(['login'])

const email = ref('')
const password = ref('')
const remember = ref(false)
const showPassword = ref(false)
const loading = ref(false)
const loginError = ref('')
const errors = reactive({ email: '', password: '' })

function validate() {
  errors.email = ''
  errors.password = ''
  let valid = true

  if (!email.value) {
    errors.email = 'メールアドレスを入力してください'
    valid = false
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email.value)) {
    errors.email = '正しいメールアドレスを入力してください'
    valid = false
  }

  if (!password.value) {
    errors.password = 'パスワードを入力してください'
    valid = false
  } else if (password.value.length < 6) {
    errors.password = 'パスワードは6文字以上で入力してください'
    valid = false
  }

  return valid
}

async function submit() {
  loginError.value = ''
  if (!validate()) return

  loading.value = true

  // ダミー認証（実際のAPIに差し替えてください）
  await new Promise((r) => setTimeout(r, 1000))

  if (email.value === 'user@example.com' && password.value === 'password') {
    emit('login', email.value)
  } else {
    loginError.value = 'メールアドレスまたはパスワードが正しくありません'
  }

  loading.value = false
}
</script>

<style scoped>
.card {
  background: white;
  border-radius: 12px;
  padding: 2.5rem 2rem;
  width: 100%;
  max-width: 400px;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.08);
}

.title {
  font-size: 1.6rem;
  font-weight: 700;
  color: #1a1a2e;
  margin-bottom: 1.8rem;
  text-align: center;
}

.field {
  margin-bottom: 1.2rem;
}

.field label {
  display: block;
  font-size: 0.85rem;
  font-weight: 600;
  color: #555;
  margin-bottom: 0.4rem;
}

.field input {
  width: 100%;
  padding: 0.65rem 0.9rem;
  border: 1.5px solid #ddd;
  border-radius: 8px;
  font-size: 0.95rem;
  outline: none;
  transition: border-color 0.2s;
}

.field input:focus {
  border-color: #4f46e5;
}

.field input.error {
  border-color: #e53e3e;
}

.error-msg {
  font-size: 0.78rem;
  color: #e53e3e;
  margin-top: 0.3rem;
  display: block;
}

.password-wrapper {
  position: relative;
}

.password-wrapper input {
  padding-right: 2.8rem;
}

.toggle-pw {
  position: absolute;
  right: 0.6rem;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  padding: 0;
  line-height: 1;
}

.options {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.4rem;
  font-size: 0.83rem;
}

.remember {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  color: #555;
  cursor: pointer;
}

.forgot {
  color: #4f46e5;
  text-decoration: none;
}

.forgot:hover {
  text-decoration: underline;
}

.login-error {
  color: #e53e3e;
  font-size: 0.85rem;
  margin-bottom: 1rem;
  text-align: center;
}

.submit-btn {
  width: 100%;
  padding: 0.75rem;
  background: #4f46e5;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s;
}

.submit-btn:hover:not(:disabled) {
  background: #4338ca;
}

.submit-btn:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}
</style>
