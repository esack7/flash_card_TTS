<template>
  <div id="app">
    <div class="container text-center">
      <h1 class="mb-5">Text To Speech</h1>
      <div class="row">
        <div class="col-md-6 mx-auto">
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
              <div id="rate-value" class="badge badge-primary float-right">1</div>
              <input
                type="range"
                id="rate"
                class="custom-range"
                min="0.5"
                max="2"
                value="1"
                step="0.1"
              >
            </div>
            <div class="form-group">
              <label for="pitch">Pitch</label>
              <div id="pitch-value" class="badge badge-primary float-right">1</div>
              <input
                type="range"
                id="pitch"
                class="custom-range"
                min="0"
                max="2"
                value="1"
                step="0.1"
              >
            </div>
            <div class="form-group">
              <select name id="voice-select" class="form-control form-control-lg"></select>
            </div>
            <button class="btn btn-light btn-lg btn-block">Speak</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: 'app',
  // components: {
  //   HelloWorld,
  // },
};
</script>

<style >
#app {
  background-color: #000;
  color: #fff;
}

body {
  background-color: #000;
}
</style>
