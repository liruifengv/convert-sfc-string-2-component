
<script setup>
import {str} from './str.js'
import * as Vue from 'vue'

import { loadModule } from 'vue3-sfc-loader';

defineProps({
  msg: {
    type: String,
    required: true
  }
})

import { defineAsyncComponent, markRaw } from 'vue'

const parseToComponent = (sfcString) => {
    const id = 1;
    const options = {
        moduleCache: { vue: Vue },
        async getFile(url) {
            return Promise.resolve(sfcString);
        },
        addStyle(styleString) {
            let style = document.getElementById(id);
            if (!style) {
                style = document.createElement('style');
                style.setAttribute('id', id);
                const ref = document.head.getElementsByTagName('style')[0] || null;
                document.head.insertBefore(style, ref);
            }
            style.textContent = styleString;
        }
    };
    const component = loadModule(`${id}.vue`, options);
    return defineAsyncComponent(() => component);
}

const parsedComponent = markRaw(parseToComponent(str));

</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <h3>
      You’ve successfully created a project with
      <a href="https://vitejs.dev/" target="_blank" rel="noopener">Vite</a> +
      <a href="https://vuejs.org/" target="_blank" rel="noopener">Vue 3</a>.
    </h3>
    <parsedComponent />
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {

  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
