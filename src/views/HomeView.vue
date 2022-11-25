<template>
  <div class="home flex flex-col h-screen max-h-screen">
    <!-- Search/Results -->
    <div
      class="search z-20 flex justify-center relative px-4 pt-8 pb-32 bg-cover"
    >
      <!-- search -->
      <div class="w-full max-w-screen-sm">
        <h4 class="text-3xl text-center text-white pb-4">Enter IP</h4>
        <div class="inpt flex">
          <input
            type="text"
            v-model="IP"
            class="
              flex-1
              text-center
              py-3
              px-2
              rounded-tl-md rounded-bl-md
              focus:outline-none
            "
            placeholder="Enter an IP address or leave it empty to get your default IP"
          />
          <span @click="getIpInfo"
            class="
              flex
              justify-center
              items-center
              rounded-tr-md rounded-br-md
              text-white
              bg-black
              px-4
              cursor-pointer
            "
            ><i class="far fa-arrow-alt-circle-right"></i
          ></span>
        </div>
      </div>
      <!-- Results Modal -->
      <IPinfo v-if="IPinfo" :IPinfo="IPinfo" />
    </div>
    <!-- Map -->
    <div id="map" class="z-10 h-full"></div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import IPinfo from "@/components/IPInfoModal.vue";
import leaflet from "leaflet";
import { onMounted, ref } from "vue";
import axios from 'axios'

export default {
  name: "HomeView",
  components: {
    IPinfo,
  },
  setup() {
    let mymap;
    const IP = ref('');
    const IPinfo = ref(null);
    const info = ref(null);

    onMounted(() => {
      mymap = leaflet.map("map").setView([51.505, -0.09], 9);

      leaflet.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
        maxZoom: 19,
        attribution:
          '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
        accessToken: "pk.eyJ1IjoicmViZWxuaWlpIiwiYSI6ImNsYXdpODhjZzA3YWkzb3A3MXZ1ejh4NmkifQ.0L2IAWRhxgeZ8UVQsNU1sg" 
      }).addTo(mymap);
    });

    const getIpInfo = async () => {
      try{
        // const res = await axios.get(`https://geo.ipify.org/api/v2/country?apiKey=at_pjD1G6TUbkXoXBbZAZ0Cocs6158tD&ipAddress=${IP.value}`);
        const {data} = await axios.get(`https://api.ipgeolocation.io/ipgeo?apiKey=052a5bca96b841fab06c8d1fd391a263&ip=${IP.value}`);
        IPinfo.value ={
          address: data.ip,
          location: data.country_name,
          timezone: data.time_zone.name,
          isp: data.isp
        };
        info.value = {
          latitude: data.latitude,
          longitude: data.longitude
        }
        // console.log(data)
        leaflet.marker([info.value.latitude, info.value.longitude]).addTo(mymap);
        mymap.setView([info.value.latitude, info.value.longitude], 13);
      }catch(err){console.log(err.message)}
    }
    

    return {IP, IPinfo,info, getIpInfo}
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;700;900&display=swap");

.home {
  font-family: "Roboto" . sans-serif;
}

.search {
  background-image: url("../assets/cool-background.png");
}
</style>
