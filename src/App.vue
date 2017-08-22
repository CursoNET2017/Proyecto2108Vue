<template>
  <div id="app">
    <maestro :datapadre="datos"></maestro>
    <detalle :datapadre="datos"></detalle>

  </div>
</template>

<script>
import maestro from './components/Maestro.vue'
import detalle from './components/Detalle.vue'


export default {
  name: 'app',
  data() {
    return {
      datos: {
        personas: [],
        currentPersona: null,
        newPersona: { Nombre: "", Apellidos: "", Edad: "", DNI:"",
        editMode: true}
      }
    }
  },
  components: {
    maestro, detalle
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
