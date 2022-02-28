<template>
  <div id="app">
    <vue-dmn

        url="diagram.dmn"
        v-on:error="handleError"
        v-on:shown="handleShown"
    ></vue-dmn>
  </div>
</template>

<script>
import VueDmn from './components/VueDmn.vue'

export default {
  name: 'App',
  components: {
    VueDmn,
  },
  methods: {
    handleError: function(err) {
      console.error('failed to show diagram', err);
    },
    handleShown: function() {
      console.log('diagram shown');
    }
  }
}
</script>

<style>
.vue-dmn-diagram-container {
  height: 600px;
}
</style>
