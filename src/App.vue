<template>
  <h1>Stain translation</h1>
  <p>
    Data from
    <a href="https://github.com/ccipd/DL-kidneyhistologicprimitives"
      >DL-kidneyhistologicprimitives</a
    >
  </p>
  <div class="buttons">
    <button
      v-for="type in Object.keys(type_data)"
      :key="type"
      :class="{ active: selected_type === type }"
      type="button"
      @click="selected_type = type"
    >
      {{ type_data[type][0] }} to {{ type_data[type][1] }} ({{
        type_data[type][2]
      }})
    </button>
  </div>
  <div class="grid-wrapper">
    <div class="grid grid-header" v-if="selected_type">
      <h2>{{ type_data[selected_type][0] }} (real)</h2>
      <div>number</div>
      <h2>{{ type_data[selected_type][1] }} (fake)</h2>
    </div>
    <div class="grid" v-if="selected_type">
      <template v-for="n in type_data[selected_type][2]" :key="n">
        <Image
          :id="'real ' + String(n - 1)"
          :src="getImageUrl(n - 1, 'real')"
          :alt="'real ' + String(n - 1)"
        ></Image>
        {{ n - 1 }}
        <Image
          :id="'fake ' + String(n - 1)"
          :src="getImageUrl(n - 1, 'fake')"
          :alt="'fake ' + String(n - 1)"
        ></Image>
      </template>
    </div>
  </div>

  <p class="end">End</p>
</template>

<script setup>
import { ref } from "vue";
import Image from "./components/Image.vue";

// This starter template is using Vue 3 experimental <script setup> SFCs
// Check out https://github.com/vuejs/rfcs/blob/master/active-rfcs/0040-script-setup.md

const selected_type = ref(null);

const type_data = {
  he_pas: ["H&E", "PAS", 501], // From, To, Images
  pas_he: ["PAS", "H&E", 467],
};

function getImageUrl(n, mode) {
  return new URL(
    `./assets/cycle/${selected_type.value}/inference_${n}_${mode}.png`,
    import.meta.url
  ).href;
}
</script>

<style>
body,
html {
  margin: 0;
  padding: 0;
}

*,
::before,
::after {
  border-width: 0;
  border-style: solid;
  border-color: currentColor;
}

#app {
  --bg-color: #31333a;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica,
    Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #f0f0f0;
  padding: 1rem 0;
  background: var(--bg-color);
  min-height: 100vh;
}

.buttons {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
  max-width: 1024px;
}

.grid {
  display: grid;
  grid-template-columns: 1fr min-content 1fr;
  gap: 0.5rem;
  align-items: center;
}

.grid-wrapper {
  overflow: auto;
  margin: 0 auto;
  max-width: 1024px;
  max-height: 100vh;
}

.grid-header {
  position: sticky;
  top: 0;
  background: var(--bg-color);
}

.end {
  padding: 2rem;
  border-bottom: 1px solid #ffffff80;
}

img {
  margin: auto;
  width: 100%;
  height: 100%;
}

a {
  color: #e9d38c;
}

button {
  background: #e9d38c;
  padding: 1.5rem;
  margin: 0.5rem;
  cursor: pointer;
}

button.active {
  border: 0.5rem solid #71dd71;
}

button:focus {
  outline: 1px dotted;
  outline: 5px auto -webkit-focus-ring-color;
}
</style>
