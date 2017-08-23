<template>
  <div id="app">
    <div>
      <maestroPersona :datapadre="datos"></maestroPersona>
      <detallePersona :datapadre="datos"></detallePersona>
    </div>
    <div>
      <maestroDomicilio :datapadre="datos"></maestroDomicilio>
      <detalleDomicilio :datapadre="datos"></detalleDomicilio>
    </div>
    <div>
      <maestroBanco :datapadre="datos"></maestroBanco>
      <detalleBanco :datapadre="datos"></detalleBanco>
    </div>

  </div>
</template>

<script>
import maestroPersona from './components/MaestroPersona.vue'
import detallePersona from './components/DetallePersona.vue'
import maestroDomicilio from './components/MaestroDomicilio.vue'
import detalleDomicilio from './components/DetalleDomicilio.vue'
import maestroBanco from './components/MaestroBanco.vue'
import detalleBanco from './components/DetalleBanco.vue'


export default {
  name: 'app',
  data() {
    return {
      datos: {
        backup: [],
        personas: [],
        currentPersona: null,
        domicilios: [],
        currentDomicilios: null,
        cuentas: [],
        currentCuentas: null,
        newPersona: { Nombre: "", Apellidos: "", Edad: "", DNI: "" },
        newCuenta: { Cuenta: "", Banco: "", Saldo: "", Activa: "" },
        newDomicilio: { Calle: "", Portal: "", Piso: "", CP: "", Localidad: "", Region: "" },
        editMode: true
      }
    }
  },
  components: {
    maestroPersona, detallePersona, maestroBanco, detalleBanco, maestroDomicilio, detalleDomicilio
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
