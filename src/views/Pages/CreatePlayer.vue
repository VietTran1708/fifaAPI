<template>
  <div style="position: relative; top: 100px">
    <b-row class="ml-3 mb-3">
      <b-col>
        <b-btn @click="openCreatePlayer" class="mr-3" variant="outline-success">
          Create Player
        </b-btn>
      </b-col>
    </b-row>
    <b-modal id="createPlayer" title="Create Player" :hide-footer="true">
      <b-card
        title="Create player"
        img-src="https://picsum.photos/600/300/?image=25"
        img-alt="Image"
        img-top
        tag="article"
        class="mb-2"
      >
        <b-form-file class="mb-3" :accept="`image/*`" v-model="playerImg" @change="loadFile(playerImg)"></b-form-file>
        <img class="w-100" id="output"/>
        <b-input class="mt-3" placeholder="Player name" v-model="playerName"></b-input>
        <b-input class="mt-3" placeholder="Age" v-model="playerAge"></b-input>
        <b-input class="mt-3" placeholder="Region" v-model="playerRegion"></b-input>
        <b-input class="mt-3" placeholder="OVR" v-model="playerOVR"></b-input>
        <b-input class="mt-3" placeholder="Level" v-model="playerLevel"></b-input>
        <b-input class="mt-3" placeholder="Position" v-model="playerPosition"></b-input>
        <b-input class="mt-3 mb-3" placeholder="Season" v-model="playerSeason"></b-input>


        <b-btn variant="outline-success" @click="createPlayer">Create player</b-btn>
      </b-card>
    </b-modal>
    <b-row>
      <b-table
        class="ml-5 mr-5"
        hover
        responsive
        striped
        :fields="fieldsDataPlayer"
        :items="dataPlayer"
        :per-page="perPage"
        :current-page="currentPage"
      >
        <template v-slot:cell(manage)="row">
          <b-btn class="mr-3" variant="outline-warning">
            Edit
          </b-btn>
          <b-btn variant="outline-danger">
            Delete
          </b-btn>
        </template>
      </b-table>
    </b-row>
    <b-row>
      <div class="d-flex justify-content-center w-100">
        <b-pagination
          align="center"
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          aria-controls="my-table"
        >
          <template #first-text><span class="text-success">First</span></template>
          <template #prev-text><span class="text-danger">Prev</span></template>
          <template #next-text><span class="text-warning">Next</span></template>
          <template #last-text><span class="text-info">Last</span></template>
        </b-pagination>
      </div>
    </b-row>
  </div>
</template>
<script>

import {BASE_API_URL, COMMON_ERROR_MESSAGE, SUCCESS_COMMON} from "@/share/const";
import commonHelpers from "@/share/common-helpers";
const axios = require('axios').default;

export default {
  components: {
  },
  data() {
    return{
      playerName: null,
      playerImg: null,
      playerAge: null,
      playerRegion: null,
      playerOVR: null,
      playerLevel: null,
      playerPosition: null,
      playerSeason: null,
      accessToken: null,
      perPage: 9,
      currentPage: 1,
      dataPlayer: [],
      fieldsDataPlayer: [
        {key: 'id', label: 'Id', sortable: true},
        {key: 'name', label: 'Name', sortable: true},
        {key: 'ovr', label: 'OVR', sortable: true},
        {key: 'position', label: 'Position', sortable: true},
        {key: 'region', label: 'Region', sortable: true},
        {key: 'season', label: 'Season', sortable: true},
        {key: 'status', label: 'Status', sortable: true},
        {key: 'age', label: 'Age', sortable: true},
        {key: 'level', label: 'Level', sortable: true},
        {key: 'created_at', label: 'Created time', sortable: true},
        {key: 'updated_at', label: 'Updated time', sortable: true},
        {key: 'user_id', label: 'Belong to user', sortable: true},
        {key: 'manage', label: 'Manage', sortable: true},
      ],
    }
  },
  computed: {
    rows() {
      return this.dataPlayer.length
    }
  },
  methods: {
    loadFile () {
      setTimeout(() => {
        const output = document.getElementById('output');
        output.src = URL.createObjectURL(this.playerImg);
        output.onload = function() {
          URL.revokeObjectURL(output.src) // free memory
        }
      }, 200)
    },

    getImg (imgPath) {
      return `${BASE_API_URL}/event/get-image?image_path=${imgPath}`;
    },

    createPlayer() {
      let formData = new FormData(); // Currently empty
      formData.append('file', this.playerImg);
      formData.append('name', this.playerName);
      formData.append('age', this.playerAge);
      formData.append('region', this.playerRegion);
      formData.append('ovr', this.playerOVR);
      formData.append('level', this.playerLevel);
      formData.append('position', this.playerPosition);
      formData.append('season', this.playerSeason);
      axios.post(`${BASE_API_URL}/api/v1/user/create-player`, formData, {
        headers: { Authorization: `Bearer ${this.accessToken}`}
      })
        .then(res => {
          if (res.data.data) {
            this.getPlayer();
            this.$bvModal.hide('createPlayer')
            return commonHelpers.showMessage('Created player', 'success')
          }
        })
        .catch(error => {
          if (error.response.status === 401) {
            commonHelpers.showMessage('Please login', 'warning')
            return window.location.href = '/login'
          }
          if (error.response.status === 406) {
            return commonHelpers.showMessage('Please login by admin account', 'warning')
          }
          commonHelpers.showMessage('error', 'warning')
        })
    },

    getPlayer () {
      let param = {
      //   name: this.childEventNameSearch,
      //   title: this.childEventTitleSearch,
      //   content: this.childEventContentSearch,
      }
      axios.get(`${BASE_API_URL}/api/v1/user/player`, {
        headers: { Authorization: `Bearer ${this.accessToken}`},
        params: param
      })
        .then((res) => {
          if (res.data.data) {
            this.dataPlayer = res.data.data;
            return commonHelpers.showMessage(SUCCESS_COMMON, 'success')
          }
          commonHelpers.showMessage(COMMON_ERROR_MESSAGE, 'warning')
        })
        .catch((error) => {
          if (error.response.status === 401) {
            commonHelpers.showMessage('Vui Lòng Đăng Nhập', 'warning')
            return window.location.href = '/adm/login'
          }
          if (error.response.status === 406) {
            return commonHelpers.showMessage('Tài Khoản Không Có Quyền', 'warning')
          }
          commonHelpers.showMessage(COMMON_ERROR_MESSAGE, 'warning')
        })
    },
    openCreatePlayer() {
      this.$bvModal.show('createPlayer');
    }
  },
  mounted() {
  },
  created() {
    this.accessToken = localStorage.getItem('access_token');
    this.getPlayer()
  }


};
</script>
<style>
</style>
