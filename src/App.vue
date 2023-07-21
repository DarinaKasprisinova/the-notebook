<template >
  <div :class="{ light: darkMode }">
    
      <div class="dark-mode-switch" >
        <label class="switch">
          <input type="checkbox" @change="toggleDarkMode" />
          <span class="slider round"></span>
        </label>
      </div>

      <h1>Notebook</h1>
      <NoteForm />
   
  </div>
</template>

<script>
import NoteForm from "./components/NoteForm.vue";

export default {
  name: "App",
  components: { NoteForm },
  data() {
    return {
      darkMode: false,
    };
  },
  methods: {
    toggleDarkMode() {
      this.darkMode = !this.darkMode;
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
  color: #d7d4d4;
  margin-top: 50px;
}
body {
  margin: 0;
  background: #2c3e50;
}

body.ligh {
  margin: 0;
  background: white;
}

/* Dark mode css styles */

.dark-mode-switch {
  position: fixed;
  top: 10px;
  right: 10px;
  z-index: 9999;
}

.switch {
  position: relative;
  display: inline-block;
  width: 40px;
  height: 24px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  border-radius: 24px;
  transition: 0.4s;
}

.slider.round {
  background-color: #000;
}

.slider.round::before {
  position: absolute;
  content: "";
  height: 18px;
  width: 18px;
  left: 3px;
  bottom: 3px;
  background-color: white;
  border-radius: 50%;
  transition: 0.4s;
}

input:checked + .slider {
  background-color: #2196f3;
}

input:checked + .slider.round {
  background-color: rgba(255, 255, 255, 0.775);
}

input:checked + .slider::before {
  transform: translateX(16px);
}

input:checked + .slider.round::before {
  transform: translateX(20px);
}

.light {
  background: #d7d4d4;
  color: rgb(17, 17, 17);
}

.light p {
  color: rgb(70, 54, 54);
}

.light #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ff0000;
  margin-top: 50px;
}
.light body {
  margin: 0;
  background: crimson;
}
</style>
