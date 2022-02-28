<template>
  <div
      ref="container"
      className="vue-dmn-diagram-container"
  />
</template>

<script>
import 'dmn-js/dist/assets/dmn-js-decision-table.css'
import 'dmn-js/dist/assets/diagram-js.css'
import 'dmn-js/dist/assets/dmn-js-shared.css'
import 'dmn-js/dist/assets/dmn-js-drd.css'
import 'dmn-js/dist/assets/dmn-js-literal-expression.css'
import 'dmn-js/dist/assets/dmn-font/css/dmn.css'
import DmnJS from 'dmn-js/dist/dmn-modeler.development.js'

export default {
  name: 'vue-dmn',
  props: {
    url: {
      type: String,
      required: true,
    }
  },
  data: function () {
    return {
      diagramXML: null,
    }
  },
  mounted: function () {
    var container = this.$refs.container
    var self = this
    this.dmnViewer = new DmnJS({
      container: container,
    })
    this.dmnViewer.on('import.done', function (event) {
      var error = event.error
      var warnings = event.warnings
      if (error) {
        self.$emit('error', error)
      } else {
        self.$emit('shown', warnings)
      }
      this.dmnViewer
          .getActiveViewer()
          .get('canvas')
          .zoom('fit-viewport');
    })
    if (this.url) {
      this.fetchDiagram(this.url)
    }
  },
  beforeDestroy: function () {
    this.dmnViewer.destroy()
  },
  watch: {
    url: function () {
      this.$emit('loading')
      this.fetchDiagram(this.url)
    },
    diagramXML: async function (xml) {
      this.dmnViewer.importXML(xml)
    },
  },
  methods: {
    fetchDiagram: function (url) {
      var self = this
      fetch(url)
          .then(function (response) {
            return response.text()
          })
          .then(function (text) {
            self.diagramXML = text
          })
          .catch(function (err) {
            self.$emit('error', err)
          })
    },
  },
}
</script>

<style>
.vue-dmn-diagram-container {
  height: 100%;
  width: 100%;
}
</style>