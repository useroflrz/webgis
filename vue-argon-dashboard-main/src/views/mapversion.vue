<template>
  <div id="map_ctner">
    <pmap_container>

    </pmap_container>
  </div>
</template>

<script>
import pmap_container from './components/puremap.vue';

export default {
  components: {
    pmap_container
  },
}

</script>

<style>
#map_ctner {
  height: 700px;
  width: 1350px;

}
</style>