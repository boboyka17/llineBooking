<script setup>

</script>

<template>
    <div class="card">
        <div class="card-body p-4">
            <h2 class="text-warning text-center fw-bold mb-0">Aopor Smart</h2>
            <div class="text-center mb-3">
                <small class="text-muted">Ao Por Smart pier is the first pier in THAILAND</small>
            </div>
            <form class="row">
                <div class="mb-3 col-lg-12">
                    <label class="form-label">ชื่อมัคคุเทศก์</label>
                    <input type="text" class="form-control" placeholder="กรุณากรอกชื่อมัคคุเทศก์">
                </div>
                <div class="mb-3 col-8">
                    <label class="form-label">เบอร์โทรศัพท์</label>
                    <input type="text" class="form-control" placeholder="กรุณากรอกเบอร์โทรศัพท์">
                </div>
                <div class="mb-3 col-4">
                    <label class="form-label">จำนวนลูกค้า</label>
                    <input type="number" class="form-control" placeholder="กรุณากรอกจำนวนลูกค้า">
                </div>
                <div class="mb-3 col-lg-12">
                    <label class="form-label">ชื่อบริษัท</label>
                    <input type="text" class="form-control" placeholder="กรุณากรอกชื่อบริษัท">
                </div>
                <div class="mb-3 col-lg-12">
                    <label class="form-label">ชื่อเรือ</label>
                    <input type="text" class="form-control" placeholder="กรุณากรอกชื่อเรือ">
                </div>
                <div class="d-flex">
                    <button class="btn btn-warning btn-block w-100">จองคิว</button>
                </div>
            </form>
        </div>
    </div>
</template>

<style scoped>
.card {
    border: none;
    box-shadow: rgba(100, 100, 111, 0.2) 0px 0px 10px 0px;
}
</style>