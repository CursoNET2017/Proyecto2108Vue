<template>
    <div id="maestro">

        <h1>Personas</h1>
        <button @click="nuevaPersona">+Nuevo Persona</button>
        <ul>
            <li v-for="persona in datapadre.backup" @click="selectPersona(persona)">
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
                    _this.datapadre.backup =JSON.parse(JSON.stringify(response));
                    _this.datapadre.personas =JSON.parse(JSON.stringify(response));
                }
            });

        },
        selectPersona: function (persona) {
            this.datapadre.editMode = true;
            this.datapadre.currentPersona = JSON.parse(JSON.stringify(persona));
            this.datapadre.newPersona = { Nombre: "", Apellidos: "", Edad: "", DNI: "" };
        },
        nuevaPersona: function (persona) {
            this.datapadre.editMode = false;
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
#maestro {
    width: 50%;
}

h1{
    width:100%;
}

ul {
    width: 100%;
}

li {
    list-style-type: none;
    width:100%;
}

li:hover {
    cursor: pointer;
}

.selected{
    background-color:brown;
}
</style>
