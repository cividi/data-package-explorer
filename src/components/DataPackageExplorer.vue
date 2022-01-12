<script setup>
// import { ref } from 'vue'

defineProps({
  datapackage: Object
})

</script>

<template>
  <h1 class='font-bold text-xl my-2'>
    {{ datapackage.title }}
  </h1>
  <h2 class="font-bold mt-4">Resourcen</h2>
  <p>
    {{ datapackage.views[0].description }}
  </p>
  <div
    v-for="resource,i in datapackage.resources"
    :key="resource.name"
  >
    <div
      class="my-4"
      v-if="resource.mediatype != 'application/vnd.mapbox-vector-tile'"
    >
      <p class="text-xs">#{{ i + 1 }}</p>
      <p>{{ resource.name }}.geojson</p>
      <p class="text-sm text-gray-500">{{ resource.mediatype }}</p>
      <p><a :id="resource.name" :onclick="download" class="cursor-pointer">Download</a></p>
    </div>
  </div>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>

<script>
export default {
  methods: {
    download(e) {
      const resource = this.datapackage.resources.filter(r => r.name === e.target.id)

      if(resource.length > 0) {
        const r = resource[0]

        let element = document.createElement('a');
        element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(JSON.stringify(r.data)));
        element.setAttribute('download', `${this.datapackage.name}-${e.target.id}.geojson`);

        element.style.display = 'none';
        document.body.appendChild(element);

        element.click();

        document.body.removeChild(element);
      }
    }
  }
}
</script>
