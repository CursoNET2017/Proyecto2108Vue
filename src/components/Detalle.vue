<template>
    <div id="panel">
        <label>Nombre: </label>
        <input v-if="datapadre.currentPersona != null" name="nombre" type="text" v-model="datapadre.currentPersona.Nombre" @input="validarPersona" placeholder="Introduce Nombre">
        <input v-else name="nombre" type="text" v-model="datapadre.newPersona.Nombre" @input="validarPersona" placeholder="Introduce Nombre">
        <label>Apellidos: </label>
        <input v-if="datapadre.currentPersona != null" name="apellidos" type="text" v-model="datapadre.currentPersona.Apellidos" @input="validarPersona" placeholder="Introduce Apellidos">
        <input v-else name="apellidos" type="text" v-model="datapadre.newPersona.Apellidos" @input="validarPersona" placeholder="Introduce Apellidos">
        <label>Edad: </label>
        <input v-if="datapadre.currentPersona != null" name="edad" type="number" v-model="datapadre.currentPersona.Edad" @input="validarPersona" placeholder="Introduce Edad">
        <input v-else name="edad" type="number" v-model="datapadre.newPersona.Edad" @input="validarPersona" placeholder="Introduce Edad">
        <input v-if="datapadre.currentPersona != null" name="DNI" type="text" v-model="datapadre.currentPersona.DNI" @input="validarPersona" placeholder="Introduce DNI">
        <input v-else name="DNI" type="text" v-model="datapadre.newPersona.DNI" @input="validarPersona" placeholder="Introduce DNI">
        <div id="botones">
            <button @click="addPersona" :disabled="datapadre.currentPersona || !isValid">Añadir</button>
            <button @click="updatePersona" :disabled="!datapadre.currentPersona || !datapadre.editMode && !isValid">Actualizar</button>
            <button @click="deletePersona" :disabled="!datapadre.currentPersona">Borrar</button>
        </div>

    </div>
</template>

<script>

export default {
    name: 'detalle',
    props:
    ['datapadre'],
    data() {
        return { isValid: false };
    },
    methods: {
        validarPersona: function () {
            var personToValidate = this.datapadre.currentPersona ? this.datapadre.currentPersona : this.datapadre.newPersona;
            var _isValid = true;
            if (!(personToValidate.Nombre.length > 0 && personToValidate.Nombre.length <= 20)) _isValid = false;
            if (!(personToValidate.Apellidos.length > 0 && personToValidate.Apellidos.length <= 20)) _isValid = false;
            if (!(personToValidate.Edad > 0 && personToValidate.Edad <= 150)) _isValid = false;
            this.isValid = _isValid;
        },
        addPersona: function () {
            let _this = this.datapadre;
            $.ajax({
                type: 'POST',
                url: 'http://localhost:50406/api/Personas/',
                data: { Nombre: _this.newPersona.Nombre, Apellidos: _this.newPersona.Apellidos, Edad: _this.newPersona.Edad, DNI: _this.newPersona.DNI},
                success: function (response) {
                    _this
                        .personas
                        .push({ Id: response.Id, Nombre: response.Nombre, Apellidos: response.Apellidos, Edad: response.Edad });
                    _this.newPersona = { Nombre: "", Apellidos: "", Edad: "", DNI: "" };
                }
            });
        },
        updatePersona: function () {
            let _this = this.datapadre;
            $.ajax({
                type: 'PUT',
                url: 'http://localhost:50406/api/Personas/'+_this.currentPersona.Id,
                data: { Id:_this.currentPersona.Id, Nombre: _this.currentPersona.Nombre, Apellidos: _this.currentPersona.Apellidos, Edad: _this.currentPersona.Edad, DNI: _this.newPersona.DNI },
                success: function (response) {
                    _this.personas.indexOf(response)
                }
            });

        },
        deletePersona: function () {
            let _this = this.datapadre;
            $.ajax({
                type: 'DELETE',
                url: 'http://localhost:50406/api/Personas/' + _this.currentPersona.Id,
                success: function (response) {
                    _this.personas
                        .splice(_this.personas.indexOf(
                            { Nombre: _this.currentPersona.Nombre, Apellidos: _this.currentPersona.Apellidos, Edad: _this.currentPersona.Edad }
                        ), 1);
                }
            });
            _this.currentPersona = { Nombre: "", Apellidos: "", Edad: "" };

        },
        error: function (xhr, textStatus, errorThrown) {
            alert("Error!->" + errorThrown + "-->" + xhr.responseText);
        }
    },
    created() {
        this.validarPersona();
    }
}

</script>

<style scoped>
#panel {
    float: right;
}
</style>
