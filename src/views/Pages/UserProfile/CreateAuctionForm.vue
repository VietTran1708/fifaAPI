<template>
  <card>
    <b-form @submit.prevent="updateProfile">
      <h6 class="heading-small text-muted mb-4">Please fill information of auction</h6>

      <div class="pl-lg-4">
        <b-row>
          <b-col lg="12">
            <form label="Select player">
              <select>
                <option value="volvo">Ronaldo</option>
                <option value="saab">Messi</option>
              </select>
            </form>
          </b-col>
          <b-col lg="12">
            <base-input
              type="text"
              label="Min price"
              placeholder="100000"
            >
            </base-input>
          </b-col>
        </b-row>
        <b-row >
          <b-col lg="12">
            <base-input
              type="time"
              label="End time"
              placeholder=""
            >
            </base-input>
          </b-col>
        </b-row>
      </div>

      <hr class="my-4">
      <!-- Create -->

      <b-row align-v="center" slot="header" >
        <b-col cols="12" class="text-right">
          <a href="#!" class="btn btn-sm btn-primary">Start auction</a>
        </b-col>
      </b-row>

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
