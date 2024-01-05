<script setup>
import { ref, onMounted } from "vue";
import liff from "@line/liff";
import axios from "axios"

const items = ref([])

function get_data(userId) {
    let config = {
        method: 'get',
        maxBodyLength: Infinity,
        url: 'https://script.google.com/macros/s/AKfycbxLov75J1Q1WfFEebpC51B0PpSZA9tD3SFM2jeFyM-eIHYPzbK0KeZOT20cBCjm0RdT/exec?action=' + userId,
        headers: {}
    };

    axios.request(config)
        .then((response) => {
            items.value = response.data
        })
        .catch((error) => {
            console.log(error);
        });
}

onMounted(() => {
    liff
        .init({
            liffId: import.meta.env.VITE_LIFF_ID, // Use own liffId
        })
        .then(() => {
            if (!liff.isLoggedIn()) {
                liff.login({ redirectUri: window.location.href });
            } else {
                liff.getProfile().then(profile => {
                    get_data(profile.userId)
                }).catch(err => console.error(err));
            }
        })
        .catch((err) => {
            console.log(err.code, err.message);
        });
})

</script>

<template>
    <div class="card">
        <div class="card-body p-5">
            <h1 class="text-center"> ลำดับคิว : {{ items.order }}</h1>
            <h5> ชื่อ : {{ items.name }}</h5>
            <h5> เบอร์โทรศัพท์ : 0{{ items.phone }}</h5>
            <h5> จำนวน {{ items.total }}</h5>
            <h5> บริษัท {{ items.company }}</h5>
            <h5> ชื่อเรือ {{ items.company }}</h5>
            <span class="text-muted"> วันที่ : {{ new Date(items.date).toLocaleDateString('th-TH', {
                year: 'numeric',
                month: 'long',
                day: 'numeric',
            }) }}</span>
            <span class="text-muted"> เวลา : {{ new Date(items.time).toLocaleTimeString('th-TH') }} น.</span>
        </div>
    </div>
</template>
