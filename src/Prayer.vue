<script  >
export default {
  name: "PrayerComponent",

   // props  
  props: ["time", "prayer"],

     // define methods
  methods: {

      // function for converting 24-time-format 
    check(time) {
      time = time
        .toString()
        .match(/^([01]\d|2[0-3])(:)([0-5]\d)(:[0-5]\d)?$/) || [time];
      if (time.length > 1) {
        time = time.slice(1);
        time[5] = +time[0] < 12 ? " AM" : " PM";
        time[0] = +time[0] % 12 || 12;
      }
      return time.join("");
    },

    // function for getting particular prayer image 
    prayerImage(prayer) {
      switch (prayer) {
        case "Fajir": {
          return new URL("./assets/fajir.png", import.meta.url);
        }
        case "Dhuhr": {
          return new URL("./assets/dhuhr.jpeg", import.meta.url);
        }
        case "Isha'a": {
          return new URL("./assets/isha.png", import.meta.url);
        }
        case "Magrib": {
          return new URL("./assets/magrib.jpeg", import.meta.url);
        }
        case "Asr": {
          return new URL("./assets/asr.jpeg", import.meta.url);
        }
        default:
          new URL("./assets/logo.svg", import.meta.url);
      }
    },
  },
};
</script>

<!-- view -->
<template >
  <div class="container">

    <!-- Prayer Name -->
    <p class="prayerName">{{ prayer }}</p>

    <!-- Prayer image -->
    <div class="imageBorder">
    <img :src="prayerImage(prayer)" style="width: 250px; height: 320px" />
    </div>

    <!-- Prayer Time -->
    <p class="time">{{ check(time) }}</p>
  </div>
</template>


<style scoped>
.container {
  text-align: center;
}
.prayerName {
  font-size: 19px;
  font-weight: bold;
  background-color: white;
  width: 100px;
  padding-inline-start: 10px;
  margin-bottom: 10px;
}
.time {
  font-size: 26px;
  font-weight: bold;
  margin-bottom: 40px;
}
.imageBorder{
     border: 10px solid #fff; 
}
</style>

