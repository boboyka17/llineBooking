<script setup>
import liff from "@line/liff";
import { ref } from 'vue';
import Navbar from './components/Navbar.vue';
import Form from './components/Form.vue';
import Complete from './components/Complete.vue';
import axios from "axios"

const pictureUrl = ref(null);
const userId = ref(null);
const displayName = ref(null);
const data = ref([]);
const is_form = ref(true);


const queryString = window.location.search;
const urlParams = new URLSearchParams(queryString);
const query = urlParams.get('query')
if (query == 'complete') {
  is_form.value = false
}
function get_data(userId) {
  let config = {
    method: 'get',
    maxBodyLength: Infinity,
    url: 'https://script.google.com/macros/s/AKfycbxLov75J1Q1WfFEebpC51B0PpSZA9tD3SFM2jeFyM-eIHYPzbK0KeZOT20cBCjm0RdT/exec?action=' + userId,
    headers: {}
  };

  axios.request(config)
    .then((response) => {
      console.log(response.data);
      data.value = response.data
    })
    .catch((error) => {
      console.log(error);
    });
}
liff
  .init({
    liffId: import.meta.env.VITE_LIFF_ID, // Use own liffId
  })
  .then(() => {
    if (!liff.isLoggedIn()) {
      liff.login({ redirectUri: window.location.href });
    } else {
      liff.getProfile().then(profile => {
        pictureUrl.value = profile.pictureUrl;
        userId.value = profile.userId;
        displayName.value = profile.displayName;
        // get_data(profile.userId)
      }).catch(err => console.error(err));
    }
  })
  .catch((err) => {
    console.log(err.code, err.message);
  });
</script>

<template>
  <div>
    <Navbar />
    <div class="container my-5 px-5">

      <Form v-if="is_form" :pictureUrl=pictureUrl :userId=userId :displayName=displayName />

      <Complete v-else="is_form" />
    </div>
  </div>
</template>

