<script setup>
import { ref, onMounted } from "vue";

const isShowSidebar = ref(true);
const userInfo = ref(null);
const btcPrice = ref(null);
const imagesList = ref(null);

const toggleSidebar = () => {
  isShowSidebar.value = !isShowSidebar.value;
}

const login =  async() => {
  const response = await fetch("https://randomuser.me/api/");
  const user = await response.json();
  user.results.forEach((e) => {
    userInfo.value = e.name;
  })
}

const getBTCPrice = async() => {
  const response = await fetch("https://api.coindesk.com/v1/bpi/currentprice.json");
  const btc = await response.json();
  btcPrice.value = btc;
  console.log("btc",   btcPrice.value);
}

const getRendomImages = async() => {
  const response = await fetch("https://picsum.photos/v2/list?limit=9");
  const images = await response.json();
  imagesList.value = images;
}

onMounted(() => {
  getRendomImages();
  getBTCPrice()
})
</script>

<template>
  <main>
    <div class="bg-slate-200 text-black h-16 w-full flex justify-between items-center px-10">
            <button 
              class="bg-slate-400 p-2 rounded-lg text-white active:bg-slate-700 focus:outline-none focus:ring focus:ring-slate-300"
              @click="toggleSidebar"
              >
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="h-6">
                    <title>menu</title>
                    <path d="M3,6H21V8H3V6M3,11H21V13H3V11M3,16H21V18H3V16Z" />
                </svg>
            </button>
            <button 
            class="bg-sky-400 px-10 py-3 rounded-lg font-medium text-white active:bg-sky-700 focus:outline-none focus:ring focus:ring-sky-300"
            @click="login"
            v-if="!userInfo"
            >Login</button>
            <p class="font-medium" v-if="userInfo">{{ userInfo.title }} {{ userInfo.first }} {{ userInfo.last }}</p>
        </div>
        <div class="h-[calc(100vh-64px)]">
          <div class="flex h-full relative">
            <div class="w-64 h-full absolute md:relative" :class="{'hidden': !isShowSidebar}">
              <div class="bg-slate-100 border-r h-full">
                <div class="p-2">
                    this is side bar
                </div>
              </div>
            </div>
            <div class="w-[calc(100%-0rem)] md:w-[calc(100%-16rem)] h-full p-4 md:px-10 overflow-auto"  :class="{'w-[calc(100%-0rem)] md:w-[calc(100%-0rem)]': !isShowSidebar }">
              <div class="rounded-lg py-4 mb-4">
                <div class="grid grid-cols-12 gap-4" v-if="btcPrice">
                <div class="col-span-12">
                  <div class="text-2xl font-medium">
                    {{ btcPrice.disclaimer }}
                  </div>
                </div>
                <div class="col-span-12 md:col-span-6 lg:col-span-4">
                  <div class="p-4 bg-yellow-500 text-white text-3xl font-semibold rounded-lg">
                    &euro;  {{ btcPrice.bpi.EUR.rate }}
                  </div>
                </div>
                <div class="col-span-12 md:col-span-6 lg:col-span-4">
                  <div class="p-4 bg-yellow-500 text-white text-3xl font-semibold rounded-lg">
                    &pound; {{ btcPrice.bpi.GBP.rate }}
                  </div>
                </div>
                <div class="col-span-12 md:col-span-6 lg:col-span-4">
                  <div class="p-4 bg-yellow-500 text-white text-3xl font-semibold rounded-lg">
                    &#36; {{ btcPrice.bpi.USD.rate }}
                  </div>
                </div>
              </div>
              </div>
              <div>
                 <div class="grid grid-cols-12 gap-4 " v-if="imagesList">
                  <div class="bg-blue-400 col-span-12 md:col-span-6 lg:col-span-4 rounded-lg" v-for="img in imagesList">
                    <img :src="img.download_url" :alt="img.author" class="rounded-lg">
                  </div>
                 </div>
              </div>
            </div>
          </div>
        </div>
  </main>
</template>
