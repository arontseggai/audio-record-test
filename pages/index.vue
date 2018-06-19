<template>
  <section id="app">
    <div class="container">
      <button @click="recordAudio">START RECORDING</button>
      <button @click="stopRecording">STOP RECORDING</button>
      <button @click="playAudio">PLAY AUDIO</button>
    </div>
    <div>
      <ul>
        <li><span>audioChunks:</span>{{ audioChunks }}</li>
        <li><span>audioBlob:</span>{{ audioBlob }}</li>
        <li><span>audiUrl:</span>{{ audioUrl }}</li>
        <li><span>audio:</span>{{ audio }}</li>
      </ul>
    </div>
  </section>
</template>

<script>
  export default {
    data() {
      return {
        mediaRecorder: null,
        audioChunks: [],
        audioBlob: null,
        audioUrl: '',
        audio: null
      }
    },
    methods: {
      recordAudio() {
  
  
        navigator.mediaDevices.getUserMedia({
            audio: true
          })
          .then(stream => {
            this.mediaRecorder = new MediaRecorder(stream);
            this.mediaRecorder.start();
  
            this.audioChunks = [];
  
            this.mediaRecorder.addEventListener("dataavailable", event => {
              this.audioChunks.push(event.data);
            });
  
            this.mediaRecorder.addEventListener("stop", () => {
              this.audioBlob = new Blob(this.audioChunks);
              this.audioUrl = URL.createObjectURL(this.audioBlob);
              this.audio = new Audio(this.audioUrl)
            });
  
          });
  
  
      },
      stopRecording() {
        this.mediaRecorder.stop()
      },
      playAudio() {
        console.log(this.audio)
        this.audio.play()
      }
    }
  }
</script>

<style>
  .container {
    padding-top: 20%;
    display: flex;
    justify-content: space-around;
  }
  
  button {
    padding: 10px 20px;
    background-color: lightcyan;
  }
  
  button:hover {
    background-color: lightskyblue;
  }
</style>
