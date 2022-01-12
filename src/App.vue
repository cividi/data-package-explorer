<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import DataPackageExplorer from './components/DataPackageExplorer.vue'
import { ref } from 'vue'

let url = ref(null)
let loaded = ref(false)
let datapackage = ref({})

if(window.location.hash) {
  url = window.location.hash.substring(1)
  
  if(isValidUrl(url)){
    fetchDataPackage(url).then(function(data) {
      datapackage.value = data
      loaded.value = true
    })
  }
} else {
  url = null
}

function isValidUrl(_string) {
  // const matchpattern = /^https?:\/\/(?:www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)$/gm;
  const matchpattern = /^(?:\w+:)?\/\/([^\s\.]+\.\S{2}|localhost[\:?\d]*)\S*$/;
  return matchpattern.test(_string);
}

async function fetchDataPackage(url) {
  let req = new Request(url);

  return await fetch(req)
  .then(function(res) {
    if (!res.ok && res.type == 'application/json') {
      throw new Error(`HTTP error! status: ${res.status}`)
    }
    return res.json()
  })
  .then(function(json) {
    return json
  })
}

</script>

<template>
  <div class="my-8">
    <h1 class="font-bold">Data Package Explorer</h1>
    <a class="text-sm" :href="url" target="_blank">JSON: {{ url }}</a><br>
    <a class="text-sm" href="https://specs.frictionlessdata.io/data-package/" target="_blank">Spezifikation</a>
  </div>
  <DataPackageExplorer :datapackage="datapackage" v-if="loaded" />
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
