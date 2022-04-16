<template>
  <div>
    <div><notifications group="default" /></div>
    <!-- Header -->
    <div class="header bg-gradient-success py-7 py-lg-8 pt-lg-9">
      <b-container class="container">
        <div class="header-body text-center mb-7">
          <b-row class="justify-content-center">
            <b-col xl="5" lg="6" md="8" class="px-5">
              <h1 class="text-white">Create an account</h1>
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
      <!-- Table -->
      <b-row class="justify-content-center">
        <b-col lg="6" md="8" >
          <b-card no-body class="bg-secondary border-0">
            <b-card-body class="px-lg-5 py-lg-5">
              <validation-observer v-slot="{handleSubmit}" ref="formValidator">
                <b-form role="form" @submit.prevent="handleSubmit(onSubmit)">
                  <base-input alternative
                              class="mb-3"
                              prepend-icon="ni ni-email-83"
                              placeholder="Email"
                              name="Email"
                              :rules="{required: true, email: true}"
                              v-model="email">
                  </base-input>

                  <base-input alternative
                              class="mb-3"
                              prepend-icon="ni ni-lock-circle-open"
                              placeholder="password"
                              type="password"
                              name="Password"
                              :rules="{required: true, min: 6}"
                              v-model="password">
                  </base-input>

                  <base-input alternative
                              class="mb-3"
                              prepend-icon="ni ni-lock-circle-open"
                              type="password"
                              placeholder="Confirm password"
                              name="Confirm password"
                              :rules="{required: true}"
                              v-model="rePassword">
                  </base-input>
<!--                  <div class="text-muted font-italic"><small>password strength: <span-->
<!--                    class="text-success font-weight-700">strong</span></small></div>-->
                  <b-row class=" my-4">
                    <b-col cols="12">
                      <base-input :rules="{ required: { allowFalse: false } }" name=Privacy Policy>
                        <b-form-checkbox v-model="agree">
                          <span class="text-muted">I agree with the <a href="#!">Privacy Policy</a></span>
                        </b-form-checkbox>
                      </base-input>
                    </b-col>
                  </b-row>
                  <div class="text-center">
                    <b-button type="submit" variant="primary" class="mt-4" @click="register">Create account</b-button>
                  </div>
                </b-form>
              </validation-observer>
            </b-card-body>
          </b-card>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
import {BASE_API_URL, COMMON_ERROR_MESSAGE} from "@/share/const";
import commonHelpers from "@/share/common-helpers";
const axios = require('axios').default;

  export default {
    name: 'register',
    data() {
      return {
        email: '',
        password: '',
        agree: false,
        rePassword: ''
      }
    },
    methods: {
      register() {
        if (!this.agree) {
          return commonHelpers.showMessage('You must accept the privacy policy for register', 'warning')
        }
        if (this.password !== this.rePassword) {
          return commonHelpers.showMessage('Password and re-password not match', 'warning')
        }
        if (this.email === '' || this.password === '') {
         return commonHelpers.showMessage('Please fill out of form for register', 'warning')
        }
        axios.post(`${BASE_API_URL}/api/v1/user/create-user`, {
          username: this.email,
          password: this.password
        }).then((res) => {
          if (res.data.data) {
            setTimeout(() => {
              commonHelpers.showMessage('Register successfully, please login', 'success')
            }, 1000)
            return window.location.href = '#/login';
          }
        }).catch((error) => {
          return commonHelpers.showMessage(error.response.data.message, 'warning')
        })
      }
    }

  };
</script>
<style></style>
