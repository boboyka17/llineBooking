<script setup>
import liff from "@line/liff";
import axios from "axios"
import { ref } from 'vue';
const props = defineProps(['pictureUrl', 'userId', 'displayName'])

const is_load = ref(false)

const msg_load = ref('ระบบกำลังจองคิว..')

const name = ref({
    name: "Test",
    is_invalid: false
});
const phone = ref({
    name: "0844708645",
    is_invalid: false
});
const total = ref({
    name: 10,
    is_invalid: false
});
const company = ref({
    name: "ซูซาน",
    is_invalid: false
});
const boat = ref({
    name: "ไททานิค",
    is_invalid: false
});

function validate_submit() {
    if (name.value.name == "") {
        name.value.is_invalid = true;
        return false
    }
    name.value.is_invalid = false;
    if (phone.value.name == "") {
        phone.value.is_invalid = true;
        return false
    }
    phone.value.is_invalid = false;
    if (total.value.name == "" || total.value.name < 0) {
        total.value.is_invalid = true;
        return false
    }
    total.value.is_invalid = false;
    if (company.value.name == "") {
        company.value.is_invalid = true;
        return false
    }
    company.value.is_invalid = false;
    if (boat.value.name == "") {
        boat.value.is_invalid = true;
        return false
    }
    boat.value.is_invalid = false;
    return true
}

function handlePush(data_res) {
    msg_load.value = "กำลังแจ้งเตือน"
    const msg = "ลำดับคิวที่ \n" + data_res.order + "เวลา \n" + data_res.time + "วันที่ \n" + data_res.date + "ชื่อมัคคุเทศก์ : " + name.value.name + "\n" + "เบอร์โทรศัพท์ : " + phone.value.name + "\n" + "จำนวนลูกค้า : " + total.value.name + "\n" + "ชื่อบริษัท : " + company.value.name + "\n" + "ชื่อเรือ : " + boat.value.name
    let data = JSON.stringify({
        "to": data_res.userId,
        "messages": [
            {
                "type": "text",
                "text": msg
            }
        ]
    });

    let config = {
        method: 'post',
        maxBodyLength: Infinity,
        url: 'https://api.line.me/v2/bot/message/push',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': 'Bearer GL1O9hUMeEwtqOl7D3wSz5X7cvtcPks8DLKTPl/lK66WzIY6Xupgi/EFBTl6gixg59fOxi1kbRmzdRJ5S09cggH5tfV72H0IU6X+YA0osoPQKWPbvk7U5aSspM1zraP/QC3yU7IsBQTIHgtN5duxnAdB04t89/1O/w1cDnyilFU=',
        },
        data: data
    };

    axios.request(config)
        .then((response) => {

        })
        .catch((error) => {
            console.log(error);
        });
}

function handleGet(userId) {
    msg_load.value = "กำลังเรียกข้อมูล"
    let config = {
        method: 'get',
        maxBodyLength: Infinity,
        url: 'https://script.google.com/macros/s/AKfycbxLov75J1Q1WfFEebpC51B0PpSZA9tD3SFM2jeFyM-eIHYPzbK0KeZOT20cBCjm0RdT/exec?action=' + userId,
        headers: {}
    };

    axios.request(config)
        .then((response) => {
            // handlePush(response.data)
            return
        })
        .catch((error) => {
            console.log(error);
        });

}

function handlePost() {
    msg_load.value = "ระบบกำลังบันทึกข้อมูล"
    var myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");

    let data = JSON.stringify({
        "userId": props.userId,
        "pictureUrl": props.pictureUrl,
        "displayName": props.displayName,
        "name": name.value.name,
        "phone": phone.value.name,
        "total": total.value.name,
        "company": company.value.name,
        "boat": boat.value.name
    });
    let config = {
        method: 'post',
        maxBodyLength: Infinity,
        url: 'https://script.google.com/macros/s/AKfycbwLbF1cSKiVDdmALAYBcdDkvNlafevpK6-vRm2B7YUDWkdnClWfS_2Dpi2A7DWD2RoL/exec?action=addUser',
        data: data
    };

    axios.request(config)
        .then((response) => {
            handleGet(props.userId);
        })
        .catch((error) => {
            console.log(error);
        });
}

function handleLine() {
    const msg = "ชื่อมัคคุเทศก์ : " + name.value.name + "\n" + "เบอร์โทรศัพท์ : " + phone.value.name + "\n" + "จำนวนลูกค้า : " + total.value.name + "\n" + "ชื่อบริษัท : " + company.value.name + "\n" + "ชื่อเรือ : " + boat.value.name
    liff
        .init({
            liffId: import.meta.env.VITE_LIFF_ID, // Use own liffId
        })
        .then(() => {
            if (!liff.isLoggedIn()) {
                liff.login({ redirectUri: window.location.href });
            } else {
                liff
                    .sendMessages([
                        {
                            type: "text",
                            text: msg,
                        },
                    ])
                    .then(() => {
                        handlePost();
                        liff.closeWindow();
                    })
                    .catch((err) => {
                        console.log("error", err);
                    });
            }
        })
        .catch((err) => {
            console.log(err.code, err.message);
        });
}

function FormSubmit() {
    if (validate_submit()) {
        is_load.value = true

        // handlePost()
        handleLine();

    }
}


</script>

<template>
    <div class="card">

        <div class="card-body p-4">
            <h2 class="text-warning text-center fw-bold mb-0">Aopor Smart</h2>
            <div class="text-center mb-3">
                <small class="text-muted">Ao Por Smart pier is the first pier in THAILAND</small>
            </div>
            <div class="text-center">
                <img :src=props.pictureUrl class="avatar mb-3">
                <h2> {{ props.displayName }}</h2>
            </div>

            <form class="row">
                <div class="mb-3 col-lg-12">
                    <label class="form-label">ชื่อมัคคุเทศก์</label>
                    <input v-model="name.name" type="text" class="form-control" :class="{ 'is-invalid': name.is_invalid }"
                        placeholder="กรุณาระบุชื่อมัคคุเทศก์">
                    <div class="invalid-feedback">
                        กรุณาระบุชื่อมัคคุเทศก์
                    </div>
                </div>
                <div class="mb-3 col-8">
                    <label class="form-label">เบอร์โทรศัพท์</label>
                    <input v-model="phone.name" type="text" class="form-control" :class="{ 'is-invalid': phone.is_invalid }"
                        placeholder="กรุณาระบุเบอร์โทรศัพท์">
                    <div class="invalid-feedback">
                        กรุณาระบุเบอร์โทรศัพท์
                    </div>
                </div>
                <div class="mb-3 col-4">
                    <label class="form-label">จำนวนลูกค้า</label>
                    <input v-model="total.name" type="number" class="form-control"
                        :class="{ 'is-invalid': total.is_invalid }" placeholder="กรุณาระบุจำนวนลูกค้า">
                    <div class="invalid-feedback">
                        กรุณาระบุจำนวนลูกค้า 1 ขึ้นไป
                    </div>
                </div>
                <div class="mb-3 col-lg-12">
                    <label class="form-label">ชื่อบริษัท</label>
                    <input v-model="company.name" type="text" class="form-control"
                        :class="{ 'is-invalid': company.is_invalid }" placeholder="กรุณาระบุชื่อบริษัท">
                    <div class="invalid-feedback">
                        กรุณาระบุชื่อบริษัท
                    </div>
                </div>
                <div class="mb-3 col-lg-12">
                    <label class="form-label">ชื่อเรือ</label>
                    <input v-model="boat.name" type="text" class="form-control" :class="{ 'is-invalid': boat.is_invalid }"
                        placeholder="กรุณาระบุชื่อเรือ">
                    <div class="invalid-feedback">
                        กรุณาระบุชื่อเรือ
                    </div>
                </div>
                <div class="d-flex">
                    <button @click="FormSubmit" type="button" :disabled="is_load" class="btn btn-warning btn-block w-100" d>

                        <div v-if="is_load">
                            <span class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
                            {{ msg_load }}
                        </div>
                        <div v-else>
                            จองคิว
                        </div>
                    </button>
                </div>
            </form>
        </div>
    </div>
</template>

<style scoped>
.avatar {
    vertical-align: middle;
    width: 150px;
    height: 150px;
    border-radius: 50%;
}

.card {
    border: none;
    box-shadow: rgba(100, 100, 111, 0.2) 0px 0px 10px 0px;
}
</style>