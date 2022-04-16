<template>
  <card>
    <b-row align-v="center" slot="header" >
      <b-col cols="8">
        <h3 class="mb-0">Edit profile </h3>
      </b-col>
      <b-col cols="4" class="text-right">
        <a href="#!" class="btn btn-sm btn-primary">Save edit</a>
      </b-col>
    </b-row>

    <b-form @submit.prevent="updateProfile">
      <h6 class="heading-small text-muted mb-4">User information</h6>

      <div class="pl-lg-4">
        <b-row>
          <b-col lg="6">
            <base-input
              type="text"
              label="Username"
              placeholder="Username"
              v-model="user.username"
            >
            </base-input>
          </b-col>
          <b-col lg="6">
            <base-input
              type="email"
              label="Email address"
              placeholder="aaron@email.com"
              v-model="user.email"
            >
            </base-input>
          </b-col>
        </b-row>
        <b-row >
          <b-col lg="6">
            <base-input
              type="text"
              label="First Name"
              placeholder="First Name"
              v-model="user.firstName"
            >
            </base-input>
          </b-col>
          <b-col lg="6">
            <base-input
              type="text"
              label="Last Name"
              placeholder="Last Name"
              v-model="user.lastName"
            >
            </base-input>
          </b-col>
        </b-row>
      </div>
      <hr class="my-4">

      <!-- Address -->
      <h6 class="heading-small text-muted mb-4">Contact information</h6>

      <div class="pl-lg-4">
        <b-row>
          <b-col lg="4">
            <base-input
              type="text"
              label="City"
              placeholder="City"
              v-model="user.city"
            >
            </base-input>
          </b-col>
          <b-col lg="4">
            <base-input
              type="text"
              label="Country"
              placeholder="Country"
              v-model="user.country"
            >
            </base-input>
          </b-col>
          <b-col lg="4">
            <base-input
              label="Telephone"
              placeholder="Telephone"
              v-model="user.telephone"
            >
            </base-input>
          </b-col>
        </b-row>
      </div>

      <hr class="my-4">
      <!-- Description -->

    </b-form>
  </card>
</template>
<script>
export default {
  data() {
    return {
      user: {
        company: '',
        username: 'ArronRamsey',
        email: '',
        firstName: 'Ramsey',
        lastName: 'Aaron',
        city: 'Ha Noi',
        country: 'Viet Nam',
        telephone: '0855556607',
        aboutMe: ``
      }
    };
  },
  methods: {
    updateProfile() {
      alert('Your data: ' + JSON.stringify(this.user));
    }
  }
};
</script>
<style></style>
