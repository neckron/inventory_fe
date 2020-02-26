<template>
  <div class="login-wrapper border border-light">

  <div class="alert alert-warning alert-dismissible fade show" role="alert" v-if="alertVisible">
    <strong>Holy guacamole!</strong>{{alertMessage}}
    <button type="button" class="close" data-dismiss="alert" aria-label="Close">
      <span aria-hidden="true">&times;</span>
    </button>
  </div>

    <form class="form-signin" @submit.prevent="login">
      <h2 class="form-signin-heading">Please sign in</h2>
      <label for="inputEmail" class="sr-only">Email address</label>
      <input v-model="loginBody.username" id="inputEmail" class="form-control" placeholder="Email address" required autofocus>
      <label for="inputPassword" class="sr-only">Password</label>
      <input v-model="loginBody.password" type="password" id="inputPassword" class="form-control" placeholder="Password" required>
      <button class="btn btn-lg btn-primary btn-block" type="submit">Sign in</button>
    </form>
  </div>
</template>
<script>
import {HTTP} from '../backend/vue-axios/axios';
export default {
  name: 'login',
  data () {
    return {
      loginBody : {
        username : "",
        password : ""
      },
      alertVisible : false,
      alertMessage : "",
      resp:[]
    }
  },
  methods: {
    login () {
      console.log(this.loginBody.username)
      console.log(this.loginBody.password)
      HTTP.post('/signin',{
        username : this.loginBody.username,
        password : this.loginBody.password
      }).then(response => {
        this.resp = response.data
        this.alertVisible = false
        console.log(response)
      })
      .catch(e => {
        this.alertVisible = true
        this.alertMessage = e
        console.log(e)
      })
    },
    
    loginSuccessful (req) {
      if (!req.data.token) {
        this.loginFailed()
        return
      }

      localStorage.token = req.data.token
      this.error = false

      this.$router.replace(this.$route.query.redirect || '/authors')
    },

    loginFailed () {
      this.error = 'Login failed!'
      delete localStorage.token
    }
  }
}
</script>
<style lang="scss" scoped>
 body {
  background: #605B56;
}

.login-wrapper {
  background: #fff;
  width: 70%;
  margin: 12% auto;
}

.form-signin {
  max-width: 330px;
  padding: 10% 15px;
  margin: 0 auto;
}
.form-signin .form-signin-heading,
.form-signin .checkbox {
  margin-bottom: 10px;
}
.form-signin .checkbox {
  font-weight: normal;
}
.form-signin .form-control {
  position: relative;
  height: auto;
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  padding: 10px;
  font-size: 16px;
}
.form-signin .form-control:focus {
  z-index: 2;
}
.form-signin input[type="email"] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.form-signin input[type="password"] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
</style>