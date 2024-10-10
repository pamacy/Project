<template>
    <div class="text-end pe-3">
      <CartInfo />
    </div>
    <!-- Main Menu -->
    <nav class="navbar navbar-expand-lg bg-dark bg-opacity-50">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Apple KU</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNav"
          aria-controls="navbarNav"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item">
              <router-link to="/" style="text-decoration: none">
                <div class="nav-link">Home</div>
              </router-link>
            </li>
            <li class="nav-item" v-if="memName === null">
              <router-link to="/Login" style="text-decoration: none">
                <div class="nav-link">Login</div>
              </router-link>
            </li>
            <li class="nav-item" v-else>
              <a href="#" @click="memlogout" style="text-decoration: none">
                <div class="nav-link">Logout</div>
              </a>
            </li>
            <li class="nav-item">
              <router-link to="/cartlist" style="text-decoration: none">
                <div class="nav-link fw-bold">Cart</div>
              </router-link>
            </li>
            <li class="nav-item" v-if="memName">
              <router-link to="/pagemember" style="text-decoration: none">
                <div class="nav-link fw-bold">{{ memName }}</div>
              </router-link>
            </li>
          </ul>
        </div>
      </div>
    </nav>
</template>

<script>
import axios from "axios";
axios.defaults.withCredentials = true;
import { EventBus } from "../event-bus";
import CartInfo from "./CartInfo.vue";

export default {
  name: "MainMenu",
  components: {
    CartInfo,
  },
  data() {
    return {
      memName: null,
      backendMessage: null,
    };
  },
  mounted() {
    this.memName = sessionStorage.getItem("memName");
    EventBus.on("loginok", this.updateMemName);
  },
  beforeUnmount() {
    EventBus.off("loginok", this.updateMemName);
  },
  methods: {
    updateMemName() {
      this.memName = sessionStorage.getItem("memName");
    },
    async memlogout() {
      const cf = window.confirm("ต้องการออกจากระบบ?");
      if (cf) {
        try {
          const response = await axios.get(
            `http://localhost:3000/members/logout`
          );
          this.backendMessage = response.data.messagelogout;

          if (this.backendMessage === "success") {
            sessionStorage.clear();
            this.memName = null;
            this.$router.push("/");
            EventBus.emit("memlogout");
          } else {
            alert("Logout ไม่สำเร็จ");
          }
        } catch (err) {
          console.error(err);
        }
      }
    },
  },
};
</script>

