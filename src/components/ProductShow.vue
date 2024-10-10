<template>
    <div v-for="(pd, pdId) in products" :key="pdId" class="mt-5">
        <div class="row">
            <div class="col-md-6 col-sm-12">
                <div class="card mx-auto" style="width: 18rem;">
                    <img :src="`http://localhost:3000/img_pd/${pd.pdId}.jpg`" alt="Product Image" class="card-img-top">
                </div>
            </div>
            <div class="col-md-6 col-sm-12">
                <table class="table">
                    <tr class="table-secondary">
                        <td><h4>{{ pd.pdId }}</h4></td>
                        <td><h4>{{ pd.pdName }}</h4></td>
                    </tr>
                    <tr>
                        <td><h5>ราคา</h5></td>
                        <td><h5>{{ pd.pdPrice }}</h5></td>
                    </tr>
                    <tr>
                        <td><h5>ยี่ห้อ</h5></td>
                        <td><h5>{{ pd.brand?.brandName || 'ไม่มีข้อมูล' }}</h5></td>
                    </tr>
                    <tr>
                        <td><h5>รายละเอียด</h5></td>
                        <td><h5>{{ pd.pdRemark }}</h5></td>
                    </tr>
                </table>
                <div class="d-flex justify-content-between mt-3">
                    <button class="btn btn-primary" @click="chkLogin()">
                        <i class="bi bi-cart lead"></i> ใส่ตะกร้า&nbsp;
                    </button>
                    <button class="btn btn-warning">
                        <i class="bi bi-pencil-fill lead"></i> แก้ไข&nbsp;&nbsp;
                    </button>
                </div>
            </div>
        </div>
    </div>
    <div class="mt-4">
        <button class="btn btn-secondary" @click="goBack">
            <i class="bi bi-arrow-left"></i> ย้อนกลับ
        </button>
    </div>
</template>

<script>
import axios from 'axios';
import { EventBus } from "../event-bus";

axios.defaults.withCredentials = true;

export default {
    name: 'ProductShow',
    data() {
        return {
            products: [], // Array เก็บข้อมูลสินค้า
            id: null, // รหัสสินค้าที่ส่งมา
            memEmail: null, // เพื่อตรวจสอบว่า login หรือยัง
            cartId: null, // เพื่อตรวจสอบตะกร้า
            backendMessage: null // เพื่อเก็บข้อความจาก backend
        };
    },
    mounted() {
        // อ่านค่าพารามิเตอร์ที่ส่งมาจาก routes
        this.id = this.$route.params.pdId;
        // ระบุ id ของสินค้าที่ต้องการ
        this.fetchProductData();
    },
    methods: {
        async fetchProductData() {
            try {
                const res = await axios.get(`http://localhost:3000/products/${this.id}`);
                this.products = res.data;
            } catch (err) {
                console.error(err);
            }
        },
        async chkLogin() {
            await this.chkSession(); // ตรวจสอบว่า Login แล้วหรือยัง
            if (this.memEmail == null) {
                alert("ต้อง Login เข้าระบบก่อน");
                return false; // ถ้ายังไม่ Login บอกให้ Login
            }
            await this.chkCart(); // ถ้า Login แล้วให้ตรวจสอบว่ามี Cart หรือยัง
            if (this.cartId == null) {
                await this.addCart(); // ถ้ายังไม่มีให้สร้างตะกร้า
            }
            this.addCartDtl(); // ถ้ามีตะกร้าแล้วให้เอาสินค้าใส่ตะกร้า
        },
        async chkSession() {
            try {
                const res = await axios.get(`http://localhost:3000/members/getss`);
                this.memEmail = res.data.email;
                console.log(this.memEmail);
            } catch (err) {
                console.error(err);
            }
        },
        async chkCart() {
            console.log('chkCart');
            try {
                const res = await axios.get(`http://localhost:3000/carts/chkcart`);
                console.log("Cart", res.data);
                this.cartId = res.data.id;
            } catch (err) {
                console.error(err);
            }
        },
        async addCart() {
            console.log("addCart");
            let customer = { // สร้างชุดข้อมูลส่งไปให้ Backend POST
                cusId: this.memEmail
            };
            try {
                const response = await axios.post(`http://localhost:3000/carts/addcart`, customer);
                this.backendMessage = response.data.messageAddCart;
                this.cartId = response.data.messageAddCart;
                console.log(response.data);
            } catch (err) {
                console.log(err);
            }
        },
        async addCartDtl() {
            console.log("addCartDtl");
            let cartdtl = { // สร้างชุดข้อมูลส่งไปให้ Backend POST
                cartId: this.cartId,
                pdId: this.id,
                pdPrice: this.products[0].pdPrice
            };
            try {
                const response = await axios.post(`http://localhost:3000/carts/addcartdtl`, cartdtl);
                EventBus.emit('cartdtlOK');
                this.backendMessage = response.data.messageAddCartDtl;
                console.log(response.data);
            } catch (err) {
                console.log(err);
            }
        },
        goBack() {
            this.$router.go(-1); // ไปยังหน้าก่อนหน้า
        }
    }
}
</script>

<style>
/* คุณสามารถเพิ่ม CSS ที่ต้องการที่นี่ */
</style>
