<template>
  <div class="row text-center">
    <!-- Carousel wrapper -->
    <div id="carouselVideoExample" class="carousel slide carousel-fade" data-mdb-ride="carousel">
        <!-- Indicators -->
        <div class="carousel-indicators">
            <button type="button" data-bs-target="#carouselVideoExample" data-bs-slide-to="0" class="active" aria-current="true" aria-label="Slide 1"></button>
            <button type="button" data-bs-target="#carouselVideoExample" data-bs-slide-to="1" aria-label="Slide 2"></button>
            <button type="button" data-bs-target="#carouselVideoExample" data-bs-slide-to="2" aria-label="Slide 3"></button>
        </div>

        <!-- Inner -->
        <div class="carousel-inner ">
            <!-- Single item -->
            <div class="carousel-item active" style="height: 500px">
                <video class="img-fluid" autoplay loop muted>
                    <source src="https://www.apple.com/105/media/us/iphone/family/2024/cf19f185-dd7e-4350-97ff-e44860713b54/anim/welcome/large_2x.mp4" />
                </video>
                <div class="textpxx carousel-caption d-none d-md-block text-md-start">
                    <h2 class="relative block w-full overflow-hidden">DOMINATOR TITANIUM</h2>
                    <p class="font-medium w-4/5">
                        Introducing the new CORSAIR DOMINATOR® <br>
                        TITANIUM DDR5 Memory, combining an elegant, <br>
                        restrained aesthetic with high-end performance you <br>
                        can rely on.
                    </p>
                    <a type="button" class="btn btn-warning shadow kanit-regular ml-auto rounded-0" style="height:max-content" asp-action="Signup" asp-controller="Customer">สั่งซื้อตอนนี้</a>
                </div>
            </div>

            <!-- Single item -->
            <div class="carousel-item" style="height: 500px">
                <video class="img-fluid" autoplay loop muted>
                    <source src="https://www.apple.com/105/media/us/ipad/2024/45762adb-901a-4726-8b0c-1f3ee092b09a/anim/welcome-hero/large_2x.mp4" />
                </video>
                <div class="carousel-caption d-none d-md-block text-md-start">
                    <h2 class="relative block w-full overflow-hidden">K70 CORE RGB</h2>
                    <p class="font-medium w-4/5 ">
                        The CORSAIR K70 CORE gaming keyboard <br> empowers your best play and makes gaming and <br> typing amazing, with effortless media control <br>
                        and CORSAIR Red linear switches.
                    </p>
                    <a type="button" class="btn btn-warning shadow kanit-regular ml-auto rounded-0" style="height:max-content" asp-action="Signup" asp-controller="Customer">สั่งซื้อตอนนี้</a>
                </div>
            </div>

            <!-- Single item -->
            <div class="carousel-item" style="height: 500px">
                <video class="img-fluid" autoplay loop muted>
                    <source src="https://www.apple.com/105/media/us/mac/family/2024/60fc0159-4236-4a03-8534-f5ba07e538c5/anim/welcome/large_2x.mp4" />
                </video>
                <div class="carousel-caption d-none d-md-block text-md-start">
                    <h2 class="relative block w-full overflow-hidden">iCUE LINK</h2>
                    <p class="font-medium w-4/5 ">
                        Experience the future of DIY PC building <br> with iCUE LINK smart components.
                    </p>
                    <a type="button" class="btn btn-warning shadow kanit-regular ml-auto rounded-0" style="height:max-content" asp-action="Signup" asp-controller="Customer">สั่งซื้อตอนนี้</a>
                </div>
            </div>
        </div>

        <button class="carousel-control-prev" type="button" data-bs-target="#carouselVideoExample" data-bs-slide="prev">
            <span class="carousel-control-prev-icon" aria-hidden="true"></span>
            <span class="visually-hidden">Previous</span>
        </button>
        <button class="carousel-control-next" type="button" data-bs-target="#carouselVideoExample" data-bs-slide="next">
            <span class="carousel-control-next-icon" aria-hidden="true"></span>
            <span class="visually-hidden">Next</span>
        </button>
    </div>
    
</div>

    <div class="row my-5">
        <h1 class="text-center kanit-bold">สินค้าแนะนำ</h1>
        <p class="fw-light w-75 mx-auto text-center kanit-medium">
            ยินดีต้อนรับทุกท่านที่มาเยี่ยมชมหน้าสินค้าแนะนำของเรา! เรามีความภูมิใจที่จะแนะนำสินค้าที่ได้รับความนิยมและมีคุณภาพสูงสุดสำหรับคุณ
        </p>
    </div>

    <!-- Product details or other content can go below -->
    <div class="product-details">
      <!-- Your existing ProductShow code -->
    </div>

  <div class="row">
    <div class="col-lg-9 h1 text-dark">My Product</div>
    <div class="col-lg-2 text-end">
      <input type="text" v-model="stext" class="form-control" />
    </div>
    <div class="col">
      <button class="btn btn-primary" @click="searchProduct()">ค้นหา</button>
    </div>
  </div>

  <div class="row g-4">
    <div
      v-for="(pd, pdId) in products"
      :key="pdId"
      class="col-lg-4 col-md-6 col-sm-12 d-flex justify-content-center"
    >
      <div class="card mt-4" style="width: 18rem; background-color: #eeeeee">
        <img
          :src="`http://localhost:3000/img_pd/${pd.pdId}.jpg`"
          class="card-img-top p-2"
          alt="สินค้า"
        />
        <div class="card-body text-center">
          <h5 class="card-title">{{ pd.pdId }} {{ pd.pdName }}</h5>
          <p class="card-text">{{ pd.pdPrice }} บาท</p>
          <router-link :to="{ name: 'ProductShow', params: { pdId: pd.pdId } }" style="text-decoration: none;">
            <div class="btn btn-primary">ดูรายละเอียด</div>
          </router-link>
        </div>
      </div>
    </div>
  </div>
  
</template>


<script>
import axios from "axios";
axios.defaults.withCredentials=true
export default {
  name: "PageProduct",
  data() {
    return {
      products: [],
      stext: "",
    };
  },

  mounted() {
    axios
      .get("http://localhost:3000/products/ten")
      .then((res) => {
        this.products = res.data;
      })
      .catch((err) => {
        console.error(err);
      });
  },
  methods: {
    searchProduct() {
      axios
        .get(`http://localhost:3000/products/search/${this.stext}`)
        .then((res) => {
          this.products = res.data;
        })
        .catch((err) => {
          console.error(err);
        });
    },
  },
};
</script>

<style scoped>
.card {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s;
}

.card:hover {
  transform: scale(1.05);
}

.card img {
  border-radius: 10px;
  object-fit: cover;
}

.card-title {
  font-size: 1.2rem;
  font-weight: bold;
}

.card-text {
  font-size: 1rem;
}

.btn-primary {
  background-color: rgb(70, 70, 255);
  border: none;
}

.btn-primary:hover {
  background-color: rgb(81, 81, 255);
}

</style>
