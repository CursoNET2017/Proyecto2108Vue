<template>
  <div id="app">
    <maestroPersona :datapadre="datos"></maestroPersona>
    <detallePersona :datapadre="datos"></detallePersona>

  </div>
</template>

<script>
import maestroPersona from './components/MaestroPersona.vue'
import detallePersona from './components/DetallePersona.vue'


export default {
  name: 'app',
  data() {
    return {
      datos: {
        backup: [],
        personas: [],
        currentPersona: null,
        newPersona: { Nombre: "", Apellidos: "", Edad: "", DNI: "" },
        editMode: true
      }
    }
  },
  components: {
    maestroPersona, detallePersona
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

* {
  float: left;
}
</style>
