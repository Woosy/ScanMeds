<template>
  <div class="container">
    <div class="row">
      <div class="split left">
        <div class="centered">
          <div class="col">
            <img
              src="@/assets/doctor.jpg"
              alt="Femme travaillant sur un ordinateur"
              class="img img-fluid"
            >
          </div>
        </div>
      </div>

      <div class="split right roboto">
        <div class="centered">
          <h1 class="title">
            {{ form === 'login' ? 'Connexion' : 'Inscription' }}
          </h1>

          <hr>

          <div
            v-for="(error, index) in errors"
            :key="index"
            class="alert alert-danger alert-dismissible fade show"
            role="alert"
          >
            {{ error }}
            <button
              type="button"
              class="close"
              data-dismiss="alert"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          
          <!-- ============================= -->
          <!-- == LOGIN FORM =============== -->
          <!-- ============================= -->
          <form
            v-if="form === 'login'"
            class="form-group"
            novalidate="true"
            @submit.prevent="login"
          >
            <div class="input-group mb-3">
              <div class="input-group-prepend">
                <span
                  class="input-group-text form-input"
                  style="height: 30px"
                ><i class="fas fa-envelope" /></span>
              </div>
              <input
                v-model="email"
                type="email"
                required="true"
                class="form-control form-input"
                placeholder="Adresse mail"
              >
            </div>

            <div class="input-group mb-3">
              <div class="input-group-prepend">
                <span
                  class="input-group-text form-input"
                  style="height: 30px"
                ><i class="fas fa-lock" /></span>
              </div>
              <input
                v-model="password"
                type="password"
                required="true"
                class="form-control form-input"
                placeholder="Mot de passe"
              >
            </div>

            <button
              type="submit"
              class="btn btn-primary"
              :disabled="!checkForm"
            >
              <i class="fas fa-sign-in-alt fa-spacer" />
              Se connecter
            </button>

            <hr>

            <div class="text-center">
              <span class="small link">Mot de passe oublié ?</span>
              <span
                class="small link"
                @click="form = 'register'"
              >Créer un compte !</span>
            </div>
          </form>


          <!-- ============================= -->
          <!-- == REGISTER FORM ============ -->
          <!-- ============================= -->
          <form
            v-if="form === 'register'"
            class="form-group"
            @submit.prevent="register"
          >
            <div class="input-group mb-3">
              <div class="input-group-prepend">
                <span
                  class="input-group-text form-input"
                  style="height: 30px"
                ><i class="fas fa-user" /></span>
              </div>
              <input
                v-model="username"
                :class="{ 'is-invalid': !isValidUsername, 'is-valid': isValidUsername }"
                type="text"
                required="true"
                class="form-control form-input"
                placeholder="Nom d'utilisateur"
                @change="isUsernameTaken"
              >
            </div>

            <div class="input-group mb-3">
              <div class="input-group-prepend">
                <span
                  class="input-group-text form-input"
                  style="height: 30px"
                ><i class="fas fa-envelope" /></span>
              </div>
              <input
                v-model="email"
                :class="{ 'is-invalid': !isValidEmail, 'is-valid': isValidEmail }"
                type="email"
                required="true"
                class="form-control form-input"
                placeholder="Adresse mail"
                @change="isEmailTaken"
              >
            </div>

            <div class="input-group mb-3">
              <div class="input-group-prepend">
                <span
                  class="input-group-text form-input"
                  style="height: 30px"
                ><i class="fas fa-lock" /></span>
              </div>
              <input
                v-model="password"
                :class="{ 'is-invalid': !isValidPassword, 'is-valid': isValidPassword }"
                type="password"
                required="true"
                class="form-control form-input"
                placeholder="Mot de passe"
              >
            </div>

            <div class="input-group mb-3">
              <div class="input-group-prepend">
                <span
                  class="input-group-text form-input"
                  style="height: 30px"
                ><i class="fas fa-lock" /></span>
              </div>
              <input
                v-model="password2"
                :class="{ 'is-invalid': !isValidPassword2, 'is-valid': isValidPassword2 }"
                type="password"
                required="true"
                class="form-control form-input"
                placeholder="Confirmation mot de passe"
              >
            </div>

            <button
              type="submit"
              class="btn btn-primary"
              :disabled="!checkForm"
            >
              <i class="fas fa-user-plus fa-spacer" />
              S'inscrire
            </button>

            <hr>

            <div class="text-center">
              <span
                class="small link"
                @click="form = 'login'"
              >Vous avez déjà un compte ?</span>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import router from '../router'

export default {
  data () {
    return {
      errors: [],
      // form inputs
      username: null,
      email: null,
      password: null,
      password2: null,
      // validators
      emailIsUnique: false,
      usernameIsUnique: false,
      // tab management
      form: 'login'
    }
  },
  computed: {
    isValidUsername: function () {
      return this.username && this.username.length > 2 && this.usernameIsUnique
    },
    isValidEmail: function () {
      const re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
      return this.email && re.test(String(this.email).toLowerCase()) && this.emailIsUnique
    },
    isValidPassword: function () {
      return this.password && this.password.length >= 6
    },
    isValidPassword2: function () {
      return this.password2 && this.password2.length >= 6 && this.password === this.password2
    },
    // Validates the form
    checkForm: function () {
      if (this.form === 'login') {
        return this.password && this.email
      } else if (this.form === 'register') {
        return this.isValidUsername && this.isValidEmail && this.isValidPassword && this.isValidPassword2
      }
      return false
    }
  },
  mounted () {
    // if user is already connected, send him directly to the dashboard
    if (localStorage.getItem('token')) {
      router.push('/dashboard?page=dashboard')
    }
  },
  methods: {
    // Tries to login a user
    login: async function () {
      this.errors = []

      if (!this.email) return this.errors.push('Veuillez indiquer votre adresse mail')
      if (!this.password) return this.errors.push('Veuillez indiquer votre mot de passe')

      const data = {
        email: this.email,
        password: this.password
      }

      // call the login route
      await this.$http.post(`${this.$apiUrl}/auth`, data).then(res => {
        localStorage.setItem('token', res.data.accessToken)
        localStorage.setItem('user', JSON.stringify(res.data.user))
        router.push('/dashboard?page=dashboard')
      }).catch(err => {
        switch (err.response.status) {
          case 401:
          case 404: 
            this.errors.push("Nom d'utilisateur et/ou mot de passe incorrect")
            break
          default: 
            this.errors.push("Oups... Une erreur est survenue")
            break
        }
      })
    },
    register: async function () {
      this.errors = []

      if (!this.email) return this.errors.push('Veuillez indiquer votre adresse mail')
      if (!this.password) return this.errors.push('Veuillez indiquer votre mot de passe')
      if (this.password.length < 6) return this.errors.push('Votre mot de passe doit contenir au moins 6 caractères')
      if (!this.password2) return this.errors.push('Veuillez confirmer votre mot de passe')
      if (this.password !== this.password2) return this.errors.push('Les deux mots de passe ne sont pas identiques')
      this.isUsernameTaken()
      this.isEmailTaken()

      if (this.errors.length) return

      const data = {
        email: this.email,
        username: this.username,
        password: this.password,
        password2: this.password2,
      }

      // call the register route
      await this.$http.post(`${this.$apiUrl}/user`, data).then((res) => {
        // once the user is registered 
        // try to login him
        this.login()
      }).catch((err) => {
        this.errors = err.response.data.errors
      })
     
    },
    // Checks if an username is already used or not
    isUsernameTaken: async function () {
      if (!this.username) return

      const error = "Nom d'utilisateur indisponible"
      
      if (this.errors.includes(error)) {
        const index = this.errors.indexOf(error)
        this.errors.splice(index, 1)
      }
      
      await this.$http.get(`${this.$apiUrl}/user/isUsernameTaken/${this.username}`).then((res) => {
        const result = res.data.result
        if (result) this.errors.push(error)
        this.usernameIsUnique = !result
      })
    },
    // Checks if an email adresse is already used or not
    isEmailTaken: async function () {
      if (!this.email) return

      const error = 'Adresse mail indisponible'
      
      if (this.errors.includes(error)) {
        const index = this.errors.indexOf(error)
        this.errors.splice(index, 1)
      }
      
      await this.$http.get(`${this.$apiUrl}/user/isEmailTaken/${this.email}`).then((res) => {
        const result = res.data.result
        if (result) this.errors.push(error)
        this.emailIsUnique = !result
      })
    }
  }
}
</script>

<style scoped>
.alert {
  text-align: left !important;
}

.roboto {
  font-family: 'Roboto', sans-serif !important;
}

.fa-spacer {
  padding-right: 10px
}

.title {
  /* color: #639BC0; */
  font-size: 40px;
  font-weight:bold;
}

hr {
  margin: 30px
}

.link {
  color: rgb(24, 140, 185);
  display: block;
  text-decoration: none;
  position: relative;
  text-transform: uppercase;
  text-decoration: none;
  
  display: inline-block;
  padding: 5px 10px;
  position: relative;
}

.link::after{
  background: none repeat scroll 0 0 transparent;
  bottom: 0;
  content: "";
  display: block;
  height: 2px;
  left: 50%;
  position: absolute;
  background: #c6e2ed;
  color: rgb(24, 140, 185);
  transition: width 0.3s ease 0s, left 0.3s ease 0s;
  width: 0;
}

.link:hover::after{
  left: 0; 
  width: 100%; 
  cursor: pointer;
}

/* Divise l'écran en deux */
.split {
  height: 100%;
  width: 50%;
  position: fixed;
  z-index: 1;
  top: 0;
  overflow-x: hidden;
}

.left {
  left: 0;
  background-color: #C6E2ED
}

.right {
  right: 0;
  background-color: #ffffff;
}

/* Centrage vertical & horizontal */
.centered {
  top: 50%;
  left: 50%;
  position: absolute;
  text-align: center;
  transform: translate(-50%, -50%);
}

img {
  width: 100%;
  height: 100%;
}

.form-input {
  padding: 30px;
  color: #3a3a3a;
  border: 1px solid #f0f0f0;
}

.btn {
  border: none;
  height: 50px;
  width: 175px;
  transition: 0.4s;
  background-color: #558eb2;
}

.btn:hover {
  background-color: #326f95;
}
</style>
