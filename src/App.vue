<template>
  <div id="app" class="container text-center">
    <h1 class="mb-3">Text To Speech</h1>
    <div class="row">
      <div class="col-lg-6 mx-auto">
        <form>
          <div class="form-group">
            <textarea
              name
              id="text-input"
              class="form-control form-control-lg"
              placeholder="Type here..."
            ></textarea>
          </div>
          <div class="form-group">
            <label for="rate">Rate</label>
            <div id="rate-value" class="badge badge-primary float-right">{{rate}}</div>
            <input
              type="range"
              id="rate"
              class="custom-range"
              min="0.5"
              max="2"
              :value="rate"
              v-on:change="handleChange"
              step="0.1"
            >
          </div>
          <div class="form-group">
            <label for="pitch">Pitch</label>
            <div id="pitch-value" class="badge badge-primary float-right">{{pitch}}</div>
            <input
              type="range"
              id="pitch"
              class="custom-range"
              min="0"
              max="2"
              :value="pitch"
              v-on:change="handleChange"
              step="0.1"
            >
          </div>
          <div class="form-group">
            <select name id="voice-select" class="form-control form-control-lg"></select>
          </div>
          <button v-on:click.prevent="speak" class="btn btn-success btn-lg btn-block">Speak</button>
          <button v-on:click.prevent="togglePause"
                  class="btn btn-info btn-lg btn-block"
          >{{pauseStatus}}</button>
          <button v-on:click.prevent="stop" class="btn btn-danger btn-lg btn-block">Stop</button>
          <label for="toggleRepeat" class="toggle-repeat mt-1">
            Repeat
            <input type="checkbox" id="toggleRepeat" class="toggle-repeat_input" v-model="repeat"/>
          </label>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data: () => ({
    synth: window.speechSynthesis,
    voices: [],
    rate: 1,
    pitch: 1,
    pauseStatus: 'Pause',
    repeat: false,
    manualStop: false,
  }),
  methods: {
    handleChange(e) {
      const { value, id } = e.target;
      if (id === 'rate') {
        this.rate = value;
        return null;
      }
      this.pitch = value;
      return null;
    },
    getVoices() {
      this.voices = this.synth.getVoices();
      if (this.voices.length > 0) {
        this.voices.map((voice) => {
          const option = document.createElement('option');

          option.textContent = `${voice.name}(${voice.lang})`;
          option.setAttribute('data-lang', voice.lang);
          option.setAttribute('data-name', voice.name);
          this.voiceSelect.appendChild(option);
          return null;
        });
      }
    },
    speak() {
      const {
        synth, textInput, voices, voiceSelect, repeat,
      } = this;
      if (synth.speaking) {
        console.error('Already speaking...');
        return;
      }
      if (textInput.value !== '') {
        const speakText = new SpeechSynthesisUtterance(textInput.value);

        speakText.onend = () => {
          if (repeat) {
            this.speak();
          }
          console.log('Done speaking...');
        };

        speakText.onerror = (err) => {
          console.log('Something went wrong speaking:\n', err);
        };

        const selectedVoice = voiceSelect.selectedOptions[0].getAttribute(
          'data-name',
        );

        voices.map((voice) => {
          if (voice.name === selectedVoice) {
            speakText.voice = voice;
          }
          return null;
        });
        speakText.rate = this.rate;
        speakText.pitch = this.pitch;

        synth.speak(speakText);
        console.log('Speaking...');
      }
    },
    stop() {
      this.repeat = false;
      this.pauseStatus = 'Pause';
      this.synth.cancel();
    },
    togglePause() {
      const { synth } = this;
      if (!synth.speaking && !synth.paused) {
        return;
      }
      if (this.pauseStatus === 'Pause') {
        synth.pause();
        this.pauseStatus = 'Resume';
        return;
      }
      synth.resume();
      this.pauseStatus = 'Pause';
    },
  },
  mounted() {
    this.voiceSelect = document.getElementById('voice-select');
    this.textInput = document.getElementById('text-input');
    this.getVoices();

    if (this.synth.onvoiceschanged !== undefined) {
      this.synth.onvoiceschanged = this.getVoices;
    }
  },
};
</script>

<style >
#app {
  height: 100%;
  max-width: 100%;
  color: #fff;
}
textarea#text-input {
  height: 40vh;
}
</style>
