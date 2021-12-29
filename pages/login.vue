<template>
  <div class="d-flex pa-4 justify-center">
    <v-card elevation="24" width="500px" class="pa-4">
      <div class="d-flex justify-center" style="font-size: 50px">Вход</div>
      <div class="d-flex mt-4">
        <v-text-field
          v-model="login"
          label="Логин"
          placeholder="Введите логин"
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
          placeholder="Введите пароль"
          prepend-inner-icon="mdi-form-textbox-password"
          hide-details
          clearable
        />
      </div>
      <div class="d-flex justify-center mt-4">
        <v-btn color="red" :disabled="!login || !password" @click="authPass"> Войти </v-btn>
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
      authPass() {
        const currentUser = this.users.find(user => user.id)
        if (this.password === currentUser.password && this.login === currentUser.login) {
          this.$router.push('mainPage')
        } else {
          this.snackbarError = true
        }
      }
    },
    mounted() {
      this.users = JSON.parse(localStorage.getItem('users')) ?? []
      const userData = this.users.find(user => user.login)
      console.log(userData)
      this.login = userData.login
      if (localStorage.getItem('users') === null) {
        try {
          this.$router.push('/')
        } catch (e) {
          localStorage.removeItem('users')
        }
      }
    }
  }
</script>
