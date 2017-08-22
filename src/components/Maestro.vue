<template>
    <div id="personas">

        <h1>Personas</h1>
        <ul>
            <button @click="nuevaPersona">+Nuevo Persona</button>
            <li v-for="persona in datapadre.personas" @click="selectPersona(persona)">
                {{persona.Apellidos}},{{persona.Nombre}} - {{persona.Edad}} años
            </li>
        </ul>

    </div>
</template>

<script>

export default {
    name: 'maestro',
    props:
    ['datapadre'],
    data() {
        return {};
    },
    methods: {
        getPersonas: function () {
            let _this = this;
            $.ajax({
                type: 'GET',
                url: 'http://localhost:50406/api/Personas/',
                success: function (response) {
                    _this.datapadre.personas = response;
                }
            });

        },
        selectPersona: function (persona) {
            this.datapadre.editMode = false;
            this.datapadre.currentPersona = persona;
            this.datapadre.newPersona = { Nombre: "", Apellidos: "", Edad: "" };
        },
        nuevaPersona: function (persona) {
            this.datapadre.editMode = true;
            this.datapadre.currentPersona = null;
        },
        error: function (xhr, textStatus, errorThrown) {
            alert("Error!->" + errorThrown + "-->" + xhr.responseText);
        }
    },
    created() {
        this.getPersonas();
    }
}

</script>

<style scoped>
li {
    list-style-type: none
}

ul {
    float: left;
}
</style>
