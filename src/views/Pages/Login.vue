<template>
  <div>
    <!-- Header -->
    <div class="header bg-gradient-success py-7 py-lg-8 pt-lg-9">
      <b-container>
        <div class="header-body text-center mb-7">
          <b-row class="justify-content-center">
            <b-col xl="5" lg="6" md="8" class="px-5">
              <h1 class="text-white">Welcome!</h1>
            </b-col>
          </b-row>
        </div>
      </b-container>
      <div class="separator separator-bottom separator-skew zindex-100">
        <svg x="0" y="0" viewBox="0 0 2560 100" preserveAspectRatio="none" version="1.1"
             xmlns="http://www.w3.org/2000/svg">
          <polygon class="fill-default" points="2560 0 2560 100 0 100"></polygon>
        </svg>
      </div>
    </div>
    <!-- Page content -->
    <b-container class="mt--8 pb-5">
      <b-row class="justify-content-center">
        <b-col lg="5" md="7">
          <b-card no-body class="bg-secondary border-0 mb-0">

            <b-card-body class="px-lg-5 py-lg-5">
              <div class="text-center text-muted mb-4">
                <small>Silahkan Masuk</small>
              </div>
              <validation-observer v-slot="{handleSubmit}" ref="formValidator">
                <b-form role="form" @submit.prevent="handleSubmit(onSubmit)">
                  <base-input alternative
                              class="mb-3"
                              name="Email"
                              :rules="{required: true, email: true}"
                              prepend-icon="ni ni-email-83"
                              placeholder="Email"
                              v-model="model.email">
                  </base-input>

                  <base-input alternative
                              class="mb-3"
                              name="Password"
                              :rules="{required: true, min: 6}"
                              prepend-icon="ni ni-lock-circle-open"
                              type="password"
                              placeholder="Password"
                              v-model="model.password">
                  </base-input>

                  <b-form-checkbox v-model="model.rememberMe">Remember me</b-form-checkbox>
                  <div class="text-center">
                    <base-button type="primary" native-type="button" class="my-4" @click="login()">Sign in</base-button>
                  </div>
                </b-form>
              </validation-observer>
            </b-card-body>
          </b-card>
          <b-row class="mt-3">
            <b-col cols="6">
              <router-link to="/dashboard" class="text-light"><small>Lupa Kata Sandi?</small></router-link>
            </b-col>
            <b-col cols="6" class="text-right">
              <router-link to="/register" class="text-light"><small>Buat Akun Baru</small></router-link>
            </b-col>
          </b-row>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
  import axios from 'axios'

  const head_form = {headers: {'Content-Type': 'multipart/form-data'}}

  export default {
      created() {
          axios.defaults.baseURL = 'http://localhost/obat/cuser/'
      },
      data() {
        return {
          model: {
            email: '',
            password: '',
            rememberMe: false
          }
        };
      },
      methods: {
          // redirectMe() {
          //       this.$router.push('/Profile') 
      
          // }
          login: function(){
                const arr_post = {
                    'email' : this.model.email,
                    'password' : this.model.password
                }
                axios.post("login",arr_post,head_form)
                    .then(response => (
                      localStorage.setItem("login-email", response['data']['data'][0]['email_address']),
                      localStorage.setItem("login-name", response['data']['data'][0]['name']),
                      this.$router.push('/Profile')
                    ))
                    .catch(error => alert(error));
            }
      }
  };
</script>