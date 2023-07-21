<template>
  <div>
    <div class="dark-mode-switch">
      <div><span class="dark:text-white">{{ isDark ? "Light" : "Dark" }} Mode</span></div>
      <label class="switch">
        <input type="checkbox" @click="toggleDark()" />
        <span class="slider round"></span>
      </label>
    </div>

    <h1 class="text-4xl font-bold mt-8 dark:text-gray-800">Notebook</h1>
    <NoteForm />
  </div>
</template>

<script>
import NoteForm from "./components/NoteForm.vue";
import { useDark, useToggle } from '@vueuse/core';

export default {
  name: "App",
  components: { NoteForm },
  setup() {
    const isDark = useDark();
    const toggleDark = useToggle(isDark);
    return { isDark, toggleDark };
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
.dark-mode-switch label {
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

.dark {
  color: rgb(17, 17, 17);
}
.dark p {
  color: rgb(254, 251, 251);
}
 .dark #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #4e4e4e;
  background: rgb(235, 235, 235);
  margin-top: 50px;
}
.dark body {
  margin: 0;
  background: rgb(235, 235, 235);
}
.dark li {
  background: #2c3e50;
  color: #ffffff;
}
.dark form {
  background: #2c3e50;
  color: #ffffff;
}
.dark label{
  color: #ffffff;
}
.dark input {
  color: #ffffff;
  background: #2c3e50;
}

.dark h2 {
  font-size: 28px; 
  font-family: "Arial", sans-serif; 
  font-weight: bold;
  color: #4e4e4e;
  text-align: center;
  margin-bottom: 20px; 
}


.dark button {
    background: #d8d7d7;
    font-weight: bold;
    font-size: 16px;
    border: 0;
    font-size: 15px;
    padding: 10px 20px;
    margin-top: 20px;
    color: rgb(57, 56, 56);
    border-radius: 4px;
}
.dark button:hover {
  background-color: #45aaf2;
}

.dark h4 {
  font-size: 20px; 
  font-family: "Arial", sans-serif; 
}

</style>
