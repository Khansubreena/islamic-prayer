<script  >
import PrayerComponent from "./Prayer.vue";
export default {
  components: {
    PrayerComponent,
  },

  // state
  data() {
    const cities = [
      "Delhi",
      "Goa",
      "Gujarat",
      "Haryana",
      "Jammu",
      "Jharkhand",
      "Karnataka",
      "Kerala",
      "Ladakh",
      "Puducherry",
      "TamilNadu",
    ];
    const days = ["Today", "Tomorrow"];
    return {
      // Initalize States
      cityData: cities,
      dayData: days,
      day: "Today",
      city: "Delhi",
      prayTimes: null,
      timeData: null,
      IshaTime: null,
      error: null,
    };
  },
  methods: {
    // function for getting particular city data
    async changeCountry(event) {
      // const city = event?.target.value ?? this.city

      this.prayTimes = null;
      this.error = null;
      fetch(
        `https://dailyprayer.abdulrcs.repl.co/api/${
          event?.target.value ?? "Delhi"
        }`
      )
        .then(async (response) => {
          const data = await response.json();

          // check for error response
          if (!response.ok) {
            // get error message from body or default to response statusText
            const error = (data && data.message) || response.statusText;
            return Promise.reject(error);
          }
          // Check if response has proper data
          if (data?.Error === undefined) {
            // store all data in prayTimes state
            this.prayTimes = data;
            const { today } = data;
            this.timeData = today;
            this.IshaTime = today?.["Isha'a"];
          } else {
            this.error = data?.Error;
            this.prayTimes = null;
            console.error("There was an error!", data?.Error);
          }
        })
        .catch((error) => {
          console.error("There was an error!", error);
        });
    },
    changeDay(event) {
      if (event.target.value === "Today") {
        console.log("today");
        (this.timeData = this.prayTimes?.today),
          (this.IshaTime = this.prayTimes?.today?.["Isha'a"]);
      } else {
        console.log("tom");
        (this.timeData = this.prayTimes?.tomorrow),
          (this.IshaTime = this.prayTimes?.tomorrow?.["Isha'a"]);
      }
    },
  },
  created() {
    this.changeCountry();
  },
};
</script>

<!-- view -->
<template >
  <div class="app">
    <!-- Dropdown for selecting City -->
    <select v-model="city" class="select" @change="changeCountry($event)">
      <option v-for="(item, id) in cityData" :value="item" :key="id">
        {{ item }}
      </option>
    </select> {{" "}}

  
    <!-- Name of selected city -->
    <p v-if="prayTimes !== null" class="city">City: {{ prayTimes?.city }}</p>

      <!-- Dropdown for selecting Day -->
    <select v-model="day" class="selectday" @change="changeDay($event)">
      <option v-for="(item, id) in dayData" :value="item" :key="id">
        {{ item }}
      </option>
    </select>
    <p class="prayertimetext">{{ `${day} Prayer Times` }}</p>

    <!-- Loader Component-->
    <div v-if="prayTimes === null && error === null" class="loader"></div>

    <!-- Error Component-->
    <p class="error" v-if="prayTimes === null && error !== null">
      {{ error }} !!
    </p>

    <!-- Prayer Component -->
    <div v-if="prayTimes !== null && error === null" class="com">
      <PrayerComponent :time="timeData.Fajr" prayer="Fajir" />
      <PrayerComponent :time="timeData.Dhuhr" prayer="Dhuhr" />
      <PrayerComponent :time="timeData.Asr" prayer="Asr" />
      <PrayerComponent :time="timeData.Maghrib" prayer="Magrib" />
      <PrayerComponent :time="IshaTime" prayer="Isha'a" />
    </div>
  </div>
</template>


<style scoped>
.app {
  background-image: url("./assets/bg.jpg");
  width: 1670px;
  height: 800px;
  padding: 30px;
}
.city {
  font-size: 19px;
  font-weight: bold;
  background-color: white;
  width: 150px;
  padding-inline-start: 10px;
  margin-top: 40px;
}

.com {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
}
.prayertimetext {
  text-align: center;
  font-size: 25px;
  font-weight: bold;
  margin-bottom: 40px;
}

.button {
  background-color: white;
  font-size: 19px;
  font-weight: bold;
  margin-left: 40px;
  border-radius: 5px;
  width: 120px;
  height: 30px;
}
.error {
  text-align: center;
  font-size: 19px;
  font-weight: bold;
  background-color: white;
  padding: 20px;
  color: red;
}

.loader {
  text-align: center;
  border: 16px solid #f3f3f3; /* Light grey */
  border-top: 16px solid green; /* Blue */
  border-radius: 50%;
  width: 90px;
  height: 90px;
  animation: spin 2s linear infinite;
  position: relative;
  left: 800px;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.select {
  padding: 12px;
  width: 400px;
  font-size: 21px;
}
.selectday {
  padding: 5px;
  width: 200px;
  font-size: 15px;
}
</style>

