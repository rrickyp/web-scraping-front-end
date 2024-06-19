<template>
  <div>
    <div>
      <label for="gym">Gym:</label>
      <select id="gym" v-model="selectedGym">
        <option v-for="(gym, index) in gyms" :key="index" :value="gym">{{ gym }}</option>
      </select>
    </div>
    <div>
      <label for="day">Day:</label>
      <select id="day" v-model="selectedDay">
        <option v-for="(day, index) in days" :key="index" :value="day">{{ day }}</option>
      </select>
    </div>
    <div>
      <label for="time-slot">Time Slot:</label>
      <select id="time-slot" v-model="selectedTimeSlot">
        <option v-for="(timeSlot, index) in timeSlots" :key="index" :value="timeSlot">{{ timeSlot }}</option>
      </select>
    </div>

    <div>
      <label for="email">Email:</label>
      <input id="email" type="email" v-model="email" />
    </div>

    <button @click="submitForm">Submit</button>
    <div v-if="response" class="response">
      <p>{{ responseMessage }}</p>
        <audio controls autoplay=true>
            <source src="https://sample-videos.com/audio/mp3/crowd-cheering.mp3" type="audio/mpeg">
            Your browser does not support the audio element.
        </audio>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      gyms: ['HKU B-Active', 'CSE Active'],
      selectedGym: null,
      days: ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'],
      selectedDay: null,
      timeSlots: ['2030-2200', '1845-2015', '1700-1830'],
      selectedTimeSlot: null,
      email: '',
      response: null,
      responseMessage: '',
    };
  },
  methods: {
    submitForm() {
      const params = new URLSearchParams({
        gym: this.selectedGym,
        day: this.selectedDay,
        time: this.selectedTimeSlot,
        email: this.email,
      });

      fetch(`https://web-scraping-gamma.vercel.app/searching?${params.toString()}`, {
        method: 'GET',
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Content-type': 'application/json',
        },
      })
        .then((response) => response.json())
        .then((data) => {
          this.response = data;
          if (data.status === 'ok') {
            this.responseMessage = 'Slot found';
            this.$refs.audioPlayer.play();
          } else {
            this.responseMessage = 'No slot found';
          }
        })
        .catch((error) => {
          this.responseMessage = 'Error: ' + error.message;
        });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.response {
  margin-top: 20px;
  padding: 10px;
  background-color: #f0f0f0;
  border-radius: 5px;
}
</style>