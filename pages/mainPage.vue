<template>
  <div class="d-flex pa-4 justify-center">
    <v-card color="#BABDC2FF" width="800px" style="border-radius: 50px">
      <div class="d-flex pa-2 justify-center">
        <v-btn color="red" @click="exit">Выйти</v-btn>
      </div>
      <div class="container">
        <div class="d-flex justify-center">
          <v-avatar color="teal" size="190" class="white--text text-h5">
            <v-img :src="photo"></v-img>
          </v-avatar>
          <div>
            <v-btn @click="dialog = true" icon>
              <v-icon> mdi-camera </v-icon>
            </v-btn>
          </div>
        </div>
      </div>
      <div class="d-flex pa-10 justify-center">
        <v-card style="font-size: 30px">{{ users.login }}</v-card>
      </div>
      <div class="d-flex pa-5 justify-center" v-if="card === false" @click="card = true">
        <v-btn>Редактировать</v-btn>
      </div>

      <div class="pa-5" v-else>
        <v-card style="border-radius: 50px">
          <div class="d-flex pa-5 justify-end">
            <v-btn @click="card = false" icon>
              <v-icon color="red"> mdi-cancel </v-icon>
            </v-btn>
          </div>

          <div class="d-flex pa-5">
            <div class="pa-5" style="width: 400px">
              <v-text-field
                label="Логин"
                placeholder="Введите логин"
                prepend-inner-icon="mdi-login"
                hide-details
                clearable
                v-model="login"
              ></v-text-field>
            </div>
            <div class="pa-5" style="width: 700px">
              <v-text-field
                label="Email"
                placeholder="Введите email"
                prepend-inner-icon="mdi-email"
                hide-details
                clearable
                v-model="email"
              ></v-text-field>
            </div>
          </div>
          <div class="d-flex pa-5">
            <div class="pa-5" style="width: 700px">
              <v-text-field
                label="Телефон"
                placeholder="Введите телефон"
                prepend-inner-icon="mdi-card-account-phone"
                hide-details
                clearable
                v-model="phone"
              ></v-text-field>
            </div>
            <div class="pa-5" style="width: 400px">
              <v-text-field
                label="Пароль"
                placeholder="Введите пароль"
                prepend-inner-icon="mdi-form-textbox-password"
                hide-details
                clearable
                v-model="password"
              ></v-text-field>
            </div>
          </div>
        </v-card>

        <div class="d-flex pa-5 justify-center">
          <v-btn
            color="green"
            :disabled="!login || !password || !email || !phone"
            @click="addUserData"
            >Сохранить</v-btn
          >
        </div>
      </div>
    </v-card>
    <v-dialog v-model="dialog" max-width="500" overlay-color="#323245">
      <v-card style="padding: 10px">
        <div class="d-flex">
          <v-btn elevation="2" @click="addPhoto">
            Загрузить Фото
            <v-icon>mdi-download</v-icon>
          </v-btn>
          <v-btn href="https://postimages.org/" target="_blank" elevation="2">
            Добавить Фото
            <v-icon>mdi-download</v-icon>
          </v-btn>
        </div>
        <v-text-field
          v-model="photo"
          placeholder="Вставьте прямую ссылку"
          hide-details
          clearable
        ></v-text-field>
      </v-card>
    </v-dialog>
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
        email: '',
        phone: '',
        password: '',
        photo: '',
        card: false,
        update: 'none',
        snackbarError: false,
        dialog: false
      }
    },
    methods: {
      addUserData() {
        const currentUser = this.users.find(user => user.id)
        if (
          this.login.match(/^[A-Z0-9-]+.+.[A-Z]{3,16}$/i) &&
          this.email.match(/^[A-Z0-9._%+-]+@[A-Z0-9-]+.+.[A-Z]{2,4}$/i) &&
          this.phone.match(/^\+?(\d{1,3})?[- .]?\(?(?:\d{2,3})\)?[- .]?\d\d\d[- .]?\d\d\d\d$/) &&
          this.password.match(/^\d{6,15}$/)
        ) {
          this.users = [
            {
              id: currentUser.id,
              login: this.login,
              email: this.email,
              phone: this.phone,
              password: this.password,
              photo: this.photo
            }
          ]
          this.localStorageUsers()
        } else {
          this.snackbarError = true
          return
        }

        this.card = false
        this.dialog = false
      },
      addPhoto() {
        const currentUser = this.users.find(user => user.id)
        this.users = [
          {
            id: currentUser.id,
            login: this.login,
            email: this.email,
            phone: this.phone,
            password: this.password,
            photo: this.photo
          }
        ]
        this.localStorageUsers()
        this.dialog = false
      },
      exit() {
        this.$router.push('login')
      },
      localStorageUsers() {
        localStorage.setItem('users', JSON.stringify(this.users))
      }
    },
    mounted() {
      this.users = JSON.parse(localStorage.getItem('users')) ?? []
      const userData = this.users.find(user => user.login === user.login)
      // console.log(userData)
      this.login = userData.login
      this.email = userData.email
      this.phone = userData.phone
      this.password = userData.password
      this.photo = userData.photo

      setInterval(() => {
        this.update = localStorage.getItem('users')
        // console.log('ok')
        if (this.update === null) {
          this.$router.push('/')
        }
      }, 4000)
    }
  }
</script>

<style scoped>
  .container {
    justify-content: center;
    width: 180px;
    height: 180px;
  }
</style>
