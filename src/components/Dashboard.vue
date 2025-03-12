<template>
  <div class="dashboard">
    <h1 class="neon-purple">Cyberise Dashboard</h1>
    <div class="timers">
      <div v-for="(timer, index) in timers" :key="index" class="timer-container">
        <div class="ticket-form">
          <div class="form-group">
            <label :for="'ticketName' + index">Ticket Name:</label>
            <input type="text" v-model="timer.ticketName" :id="'ticketName' + index" readonly />
          </div>
          <div class="form-group">
            <label :for="'hours' + index">Hours:</label>
            <input type="number" v-model="timer.hours" :id="'hours' + index" min="0" />
          </div>
          <div class="form-group">
            <label :for="'minutes' + index">Minutes:</label>
            <input type="number" v-model="timer.minutes" :id="'minutes' + index" min="0" max="59" />
          </div>
          <div class="form-group">
            <label :for="'seconds' + index">Seconds:</label>
            <input type="number" v-model="timer.seconds" :id="'seconds' + index" min="0" max="59" />
          </div>
          <div class="timer">
            <p>Timer: {{ formattedTime(timer) }}</p>
            <p>Price: {{ price(timer) }} Ar</p>
          </div>
          <div class="button-group">
            <button @click="startTimer(index)">Start</button>
            <button class="stop-button" @click="stopTimer(index)">Stop</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import sound1 from '@/assets/sounds/sound1.mp3';
import sound2 from '@/assets/sounds/sound2.mp3';
import sound3 from '@/assets/sounds/sound3.mp3';
import sound4 from '@/assets/sounds/sound4.mp3';
import soundA from '@/assets/sounds/soundA.mp3';
import soundB from '@/assets/sounds/soundB.mp3';
import soundC from '@/assets/sounds/soundC.mp3';
import soundD from '@/assets/sounds/soundD.mp3';

export default {
  data() {
    return {
      timers: [
        { ticketName: '1', hours: 0, minutes: 15, seconds: 0, timer: null, elapsedTime: 0, pricePer15Min: 500, sound: new Audio(sound1) },
        { ticketName: '2', hours: 0, minutes: 15, seconds: 0, timer: null, elapsedTime: 0, pricePer15Min: 500, sound: new Audio(sound2) },
        { ticketName: '3', hours: 0, minutes: 15, seconds: 0, timer: null, elapsedTime: 0, pricePer15Min: 500, sound: new Audio(sound3) },
        { ticketName: '4', hours: 0, minutes: 15, seconds: 0, timer: null, elapsedTime: 0, pricePer15Min: 500, sound: new Audio(sound4) },
        { ticketName: 'A', hours: 0, minutes: 15, seconds: 0, timer: null, elapsedTime: 0, pricePer15Min: 500, sound: new Audio(soundA) },
        { ticketName: 'B', hours: 0, minutes: 15, seconds: 0, timer: null, elapsedTime: 0, pricePer15Min: 500, sound: new Audio(soundB) },
        { ticketName: 'C', hours: 0, minutes: 15, seconds: 0, timer: null, elapsedTime: 0, pricePer15Min: 500, sound: new Audio(soundC) },
        { ticketName: 'D', hours: 0, minutes: 15, seconds: 0, timer: null, elapsedTime: 0, pricePer15Min: 500, sound: new Audio(soundD) },
      ],
    };
  },
  methods: {
    formattedTime(timer) {
      const totalSeconds = timer.hours * 3600 + timer.minutes * 60 + timer.seconds - timer.elapsedTime;
      const hours = Math.floor(totalSeconds / 3600);
      const minutes = Math.floor((totalSeconds % 3600) / 60);
      const seconds = totalSeconds % 60;
      return `${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
    },
    price(timer) {
      const remainingMinutes = Math.ceil((timer.hours * 3600 + timer.minutes * 60 + timer.seconds - timer.elapsedTime) / 60);
      return Math.ceil(remainingMinutes / 15) * timer.pricePer15Min;
    },
    startTimer(index) {
      const timer = this.timers[index];
      if (timer.timer) return;
      timer.elapsedTime = 0;
      timer.timer = setInterval(() => {
        const totalDuration = timer.hours * 3600 + timer.minutes * 60 + timer.seconds;
        if (timer.elapsedTime < totalDuration) {
          timer.elapsedTime++;
        } else {
          clearInterval(timer.timer);
          timer.timer = null;
          timer.sound.play();
        }
      }, 1000);
    },
    stopTimer(index) {
      const timer = this.timers[index];
      clearInterval(timer.timer);
      timer.timer = null;
    },
  },
  beforeDestroy() {
    this.timers.forEach(timer => clearInterval(timer.timer));
  },
};
</script>

<style scoped>
.dashboard {
  padding: 20px;
  background-color: #121212;
  color: #ffffff;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.timers {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}
.timer-container {
  background-color: #333333;
  color: #ffffff;
  padding: 10px;
  text-align: center;
  border-radius: 5px;
}
.ticket-form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.form-group {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.form-group input {
  width: 100px; /* Set a fixed width for all inputs */
  text-align: center; /* Center the text inside the input */
}
.timer {
  margin-top: 10px;
}
.button-group {
  display: flex;
  justify-content: space-between;
}
button {
  margin-top: 10px;
  background-color: #227a61;
  color: #ffffff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
}
button:hover {
  background-color: #1565c0;
}
.stop-button {
  background-color: #e53935;
}
.stop-button:hover {
  background-color: #c62828;
}
.neon-purple {
  color: black; /* Texte noir */
  text-shadow: 0 0 5px #c0ff00, 0 0 10px #c0ff00, 0 0 15px #c0ff00, 0 0 20px #c0ff00; /* Contour lumineux vert citron */
}
</style>