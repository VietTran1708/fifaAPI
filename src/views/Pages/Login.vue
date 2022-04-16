<template>
  <div>
    <div><notifications group="default" /></div>
    <!-- Header -->
    <div class="header bg-gradient-success py-7 py-lg-8 pt-lg-9">
      <b-container>
        <div class="header-body text-center mb-7">
          <b-row class="justify-content-center">
            <b-col xl="5" lg="6" md="8" class="px-5">
              <h1 class="text-white">Welcome back!</h1>
              <p class="text-lead text-white">Have a great time bidding on players.</p>
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
              <validation-observer v-slot="{handleSubmit}" ref="formValidator">
                <b-form role="form" @submit.prevent="handleSubmit(onSubmit)">
                  <base-input alternative
                              class="mb-3"
                              name="UserName"
                              :rules="{required: true}"
                              prepend-icon="ni ni-email-83"
                              placeholder="User name"
                              v-model="model.username">
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
                    <base-button type="primary" @click="login" class="my-4">Sign in</base-button>
                  </div>
                </b-form>
              </validation-observer>
            </b-card-body>
          </b-card>
          <b-row class="mt-3">
            <b-col cols="6">
              <router-link to="/dashboard" class="text-light"><small>Forgot password?</small></router-link>
            </b-col>
            <b-col cols="6" class="text-right">
              <router-link to="/register" class="text-light"><small>Create new account</small></router-link>
            </b-col>
          </b-row>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>

  import {BASE_API_URL} from "@/share/const";
  import commonHelpers from "@/share/common-helpers";
  const axios = require('axios').default;

  export default {
    data() {
      return {
        model: {
          username: 'minh10',
          password: 'minh10',
        }
      };
    },
    methods: {
      login() {
        axios.post(`${BASE_API_URL}/api/v1/auth/login`, {
          username: this.model.username,
          password: this.model.password,
        }).then(res => {
          // console.log(res.data.data.access_token)
          if (res.data.data.access_token) {
            localStorage.setItem('access_token', res.data.data.access_token);
            return window.location.href = '/dashboard'
          }
          return commonHelpers.showMessage('User name or password is not correct', 'warning', 'Fifa')
        }).catch(() => {
          return commonHelpers.showMessage('User name or password is not correct', 'warning', 'Fifa')
        })
      },
    }
  };
</script>
