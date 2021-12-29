<template>
  <div class="d-flex pa-4 justify-center">
    <v-card elevation="24" width="500px" class="pa-4">
      <div class="d-flex justify-center" style="font-size: 50px">Регистрация</div>
      <div class="d-flex mt-4">
        <v-text-field
          v-model="login"
          label="Логин"
          placeholder="Логин должен содержать не менее 6 символов"
          prepend-inner-icon="mdi-login"
          hide-details
          clearable
        />
      </div>
      <div class="d-flex mt-4">
        <v-text-field
          v-model="password"
          label="Пароль"
          elevation="24"
          placeholder="Пароль должен содержать не менее 6 цифр"
          prepend-inner-icon="mdi-form-textbox-password"
          hide-details
          clearable
        />
      </div>
      <div class="d-flex justify-center mt-4">
        <v-btn color="red" :disabled="!login || !password" @click="registration">
          Зарегестрироваться
        </v-btn>
      </div>
    </v-card>

    <v-snackbar
      style="display: flex; justify-content: flex-end"
      v-model="snackbarError"
      color="red"
    >
      Ошибка, введите правильный логин или пароль
    </v-snackbar>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        users: [],
        login: '',
        password: '',
        snackbarError: false
      }
    },
    methods: {
      registration() {
        if (this.password.match(/^\d{6,15}$/) && this.login.match(/^[A-Z0-9-]+.+.[A-Z]{3,16}$/i)) {
        } else {
          this.snackbarError = true
          return
        }
        if (this.users.login === this.login) {
          if (this.users.password === this.password) {
            this.$router.push('login')
          }
        } else if (this.users.login === undefined) {
          const user = {
            id: Date.now(),
            login: this.login,
            password: this.password
          }
          console.log(user)
          this.users.push(user)
          this.localStorageBd()
          this.$router.push('mainPage')
        } else {
          alert('У вас уже есть аккаунт')
        }
      },
      localStorageBd() {
        localStorage.setItem('users', JSON.stringify(this.users))
      }
    },
    mounted() {
      this.users = JSON.parse(localStorage.getItem('users')) ?? []
      if (localStorage.getItem('users')) {
        try {
          this.$router.push('mainPage')
        } catch (e) {
          localStorage.removeItem('users')
        }
      }
    }
  }
</script>
