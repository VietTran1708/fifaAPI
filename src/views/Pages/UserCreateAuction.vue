<template>
  <div>
    <div class="header pb-8 pt-5 pt-lg-8 d-flex align-items-center profile-header"
         style="min-height: 600px; background-image: url(img/theme/profile-cover.jpg); background-size: cover; background-position: center top;">
      <b-container fluid>
        <!-- Mask -->
        <!-- Header container -->
        <b-container fluid class="d-flex align-items-center">
          <b-row >
            <b-col lg="8" md="10">
              <h1 class="display-2 text-black">Create your auction</h1>
            </b-col>
          </b-row>
        </b-container>
      </b-container>
    </div>

    <b-container fluid class="mt--6">
      <b-row>
        <b-col xl="12" class="order-xl-1">
          <create-auction-form></create-auction-form>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
import CreateAuctionForm from './UserProfile/CreateAuctionForm.vue';
export default {
  components: {
    CreateAuctionForm,
  }
};
</script>
<style>
</style>
