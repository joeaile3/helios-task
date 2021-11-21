<template>
  <div id="app">
    <transition name="fade" :v-leave="!userClicked">
      <div v-if="userClicked" class="card card_task animated fadeIn ">
        <div class="container">
          <div class="row">
            <div class="col-6">
              <div class="float-left ml-2 ml-md-4 mt-1">
                <i>
                  <font-awesome-icon :icon="iconClose" style="color: black; font-size: 1.5rem"
                                     @click="userClicked = false"/>
                </i>
              </div>
            </div>
          </div>
          <div class="row mt-5">
            <div style="margin-left:40%">
              <img :src="specificUser.picture.medium"/>
            </div>
            <div class="text-center mt-2" style="margin-left:0">
              <h5>{{ specificUser.name.title }} {{ specificUser.name.first }} {{ specificUser.name.last }}</h5>
            </div>
          </div>
          <div class="d-flex flex-row mt-5">
            <div class="col-md-4">
              <p class="grey">Email</p>
            </div>
            <div class="col-md-8">
              <p class="info">{{ specificUser.email }}</p>
            </div>
          </div>
          <hr>
          <div class="d-flex flex-row mt-5">
            <div class="col-md-4">
              <p class="grey">Gender</p>
            </div>
            <div class="col-md-8">
              <p class="info">{{ specificUser.gender }}</p>
            </div>
          </div>
          <hr>
          <div class="d-flex flex-row mt-5">
            <div class="col-md-4">
              <p class="grey">Number</p>
            </div>
            <div class="col-md-7">
              <p class="info">{{ specificUser.cell }}</p>
            </div>
          </div>
          <hr>
          <div class="d-flex flex-row mt-5">
            <div class="col-md-4">
              <p class="grey">Date of Birth</p>
            </div>
            <div class="col-md-8">
              <p class="info">{{ specificUser.dob.date }}</p>
            </div>
          </div>
          <hr>
        </div>
      </div>
    </transition>
    <div class="list-group w-50 d-flex" style="margin-left:25%;">
      <a v-for="user in users" :key="user.id.value"
         class="list-group-item list-group-item-action flex-column align-items-start" @click="getSpecificUser(user)">
        <div class="d-flex w-100 float-left flex-row">
          <div>
            <img :src="user.picture.thumbnail"/>
          </div>
          <div style="margin-left: 10px">
            <p style="margin-bottom: 0">{{ user.name.title }} {{ user.name.first }} {{ user.name.last }}</p>
            <small>{{ user.email }}</small>
          </div>
        </div>
      </a>
    </div>
    <scroll-loader :loader-method="getUsers"></scroll-loader>
  </div>
</template>

<script>

import {faTimesCircle} from "@fortawesome/free-solid-svg-icons";
import moment from "moment";

export default {
  name: 'App',

  data() {
    return {
      users: [],
      page: 1,
      iconClose: faTimesCircle,
      userClicked: false,
      specificUser: {},
    }
  },

  methods:
      {
        moment,
        getUsers() {
          //this is used to load the next 20 users
          let results = 20 * this.page

          //GET api call that takes the number of users as well as the seed so that the same users remain
          this.axios.get('https://randomuser.me/api/?results=' + results + '&seed=foobar').then((response) => {
            this.users = response.data.results
            this.page++;
          })
        },
        getSpecificUser(user) {
          this.specificUser = user
          // make the first letter of the gender uppercase
          this.specificUser.gender = this.specificUser.gender.charAt(0).toUpperCase() + this.specificUser.gender.slice(1)
          //change the format of the date of birth
          this.specificUser.dob.date = moment(String(this.specificUser.dob.date)).format('DD MMMM YYYY')
          this.userClicked = true
        },
      },
  created() {
    this.getUsers()
  }

}
</script>

<style>
.card_task {
  height: 100vh;
  width: 25rem;
  position: fixed !important;
  z-index: 12 !important;
  top: 0;
  right: 0;
  float: right;

  overflow-x: hidden;
  padding-top: 20px;
  background: #FFFFFF;
  transition: opacity .5s;
  /* S1 */
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
}

.grey {
  color: grey !important;
}

.info {
  color: black !important;
}

</style>
