<template>
<div class="background">
  <div id="register" class="text-center">
    <form class="form-register" @submit.prevent="register">
      <h1 class="h3 mb-3 font-weight-normal">Create Account</h1>
      <div class="alert alert-danger" role="alert" v-if="registrationErrors">
        {{ registrationErrorMsg }}
      </div>
      <label for="username" class="sr-only">Username</label>
      <input
        type="text"
        id="username"
        class="form-control"
        placeholder="Username"
        v-model="user.username"
        required
        autofocus
      />
      <label for="password" class="sr-only">Password</label>
      <input
        type="password"
        id="password"
        class="form-control"
        placeholder="Password"
        v-model="user.password"
        required
      />
      <input
        type="password"
        id="confirmPassword"
        class="form-control"
        placeholder="Confirm Password"
        v-model="user.confirmPassword"
        required
      />
      <router-link :to="{ name: 'login' }">Have an account?</router-link>
      <button class="btn btn-lg btn-primary btn-block" type="submit">
        Create Account
      </button>
    </form>
  </div>
  </div>
</template>

<script>
import authService from '../services/AuthService';

export default {
  name: 'register',
  data() {
    return {
      user: {
        username: '',
        password: '',
        confirmPassword: '',
        role: 'user',
      },
      registrationErrors: false,
      registrationErrorMsg: 'There were problems registering this user.',
    };
  },
  methods: {
    register() {
      if (this.user.password != this.user.confirmPassword) {
        this.registrationErrors = true;
        this.registrationErrorMsg = 'Password & Confirm Password do not match.';
      } else {
        authService
          .register(this.user)
          .then((response) => {
            if (response.status == 201) {
              this.$router.push({
                path: '/login',
                query: { registration: 'success' },
              });
              
            }
          })
          .catch((error) => {
            const response = error.response;
            this.registrationErrors = true;
            if (response.status === 400) {
              this.registrationErrorMsg = 'Bad Request: Validation Errors';
            }
          });
      }
    },
    clearErrors() {
      this.registrationErrors = false;
      this.registrationErrorMsg = 'There were problems registering this user.';
    },
  },
};
</script>

<style scoped>
#register {
  margin-top: 100px;
  margin-bottom: 100px;
}

.form-register {
  display: grid;
  gap: 10px;
  max-width: 300px;
  margin: 0 auto;
  padding: 20px;
  border-radius: 5px;
font-family: 'Dosis', monospace, sans-serif;
  background-color: rgb(236, 245, 226);
  border: solid 4px;
  border-radius: 25px;
}

router-link {
  padding-left: 15px;
}

.alert {
  margin-bottom: 20px;
}

label {
  display: none; 
}

input[type="text"],
input[type="password"] {
  margin-bottom: 10px;
  border-radius: 5px;
  padding: 10px;
  background-color: #CCCCFF;
  color: #333;
  font-family: 'Dosis', monospace, sans-serif;
}

router-link {
  display: block;
  margin-bottom: 10px;
}

button[type="submit"] {
  margin-top: 10px;
  background-color: #89c36f; 
  color: #fff;
  padding: 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  font-family: 'Dosis', monospace, sans-serif;
}

button[type="submit"]:hover {
  background-color: #2e7d32; 
}

.background {
  font-family: "Dosis", monospace, sans-serif;
  background: url(../assets/cropped-orange.jpg) no-repeat center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  padding-bottom: 50px;
  opacity: 0.9;
  font-size: 1.1em;
  padding-left: 5%;

}


</style>
