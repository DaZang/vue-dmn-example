<template>
  <div id="app">
    <vue-bpmn
        url="diagram.bpmn"
        v-on:error="handleError"
        v-on:shown="handleShown"
    ></vue-bpmn>
  </div>
</template>

<script>
import VueBpmn from './components/VueBpmn.vue'

export default {
  name: 'App',
  components: {
    VueBpmn
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
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.vue-bpmn-diagram-container {
  height: 10000px;
}
</style>
