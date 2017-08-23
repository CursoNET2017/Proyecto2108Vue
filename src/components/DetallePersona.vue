<template>
    <div id="panel">
        <div class="linea">
            <label>Nombre: </label>
            <input v-if="datapadre.currentPersona != null" name="nombre" type="text" v-model="datapadre.currentPersona.Nombre" @input="validarPersona" placeholder="Introduce Nombre">
            <input v-else name="nombre" type="text" v-model="datapadre.newPersona.Nombre" @input="validarPersona" placeholder="Introduce Nombre">
        </div>
        <div class="linea">
            <label>Apellidos: </label>
            <input v-if="datapadre.currentPersona != null" name="apellidos" type="text" v-model="datapadre.currentPersona.Apellidos" @input="validarPersona" placeholder="Introduce Apellidos">
            <input v-else name="apellidos" type="text" v-model="datapadre.newPersona.Apellidos" @input="validarPersona" placeholder="Introduce Apellidos">
        </div>
        <div class="linea">
            <label>Edad: </label>
            <input v-if="datapadre.currentPersona != null" name="edad" type="number" v-model="datapadre.currentPersona.Edad" @input="validarPersona" placeholder="Introduce Edad">
            <input v-else name="edad" type="number" v-model="datapadre.newPersona.Edad" @input="validarPersona" placeholder="Introduce Edad">
        </div>
        <div class="linea">
            <label>DNI: </label>
            <input v-if="datapadre.currentPersona != null" name="DNI" type="text" v-model="datapadre.currentPersona.DNI" @input="validarPersona" placeholder="Introduce DNI">
            <input v-else name="DNI" type="text" v-model="datapadre.newPersona.DNI" @input="validarPersona" placeholder="Introduce DNI">
        </div>
        <div id="botones">
            <button @click="addPersona" :disabled="datapadre.currentPersona || !isValid">Añadir</button>
            <button @click="updatePersona" :disabled="!datapadre.currentPersona || !datapadre.editMode || !isValid">Actualizar</button>
            <button @click="deletePersona" :disabled="!datapadre.currentPersona">Borrar</button>
        </div>

    </div>
</template>

<script>

export default {
    name: 'detallePersona',
    props: ['datapadre'],
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

            var regExpDNI = /\d{8}[A-Za-z]$/;
            if (!regExpDNI.test(personToValidate.DNI)) _isValid = false;
            this.isValid = _isValid;
        },
        addPersona: function () {
            let _this = this.datapadre;
            $.ajax({
                type: 'POST',
                url: 'http://localhost:50406/api/Personas/',
                data: _this.newPersona,
                success: function (response) {
                    _this.personas.push(response);
                    _this.newPersona = { Nombre: "", Apellidos: "", Edad: "", DNI: "" };
                    _this.backup = JSON.parse(JSON.stringify(_this.personas));
                }
            });
        },
        updatePersona: function () {
            let _this = this.datapadre;
            $.ajax({
                type: 'PUT',
                url: 'http://localhost:50406/api/Personas/' + _this.currentPersona.Id,
                data: _this.currentPersona,
                success: function (response) {
                    var index = _this.backup.findIndex((el) => el.Id == _this.currentPersona.Id);
                    _this.personas[index] = _this.currentPersona;
                    _this.backup = JSON.parse(JSON.stringify(_this.personas));
                    _this.currentPersona = null;
                }
            });

        },
        deletePersona: function () {
            let _this = this.datapadre;
            $.ajax({
                type: 'DELETE',
                url: 'http://localhost:50406/api/Personas/' + _this.currentPersona.Id,
                success: function (response) {
                    var index = _this.backup.findIndex((el) => { el.Id == _this.currentPersona.Id });
                    _this.personas.splice(index, 1);
                    _this.backup = JSON.parse(JSON.stringify(_this.personas));
                    _this.currentPersona = null;
                }
            });
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
    margin-left: 5%;
    width: 45%;
}

.linea {
    width: 100%
}

label,
input {
    display: block;
}

label {
    width: 25%;
}

input {
    margin-left: 5%;
    width: 55%;
}

#botones {
    margin-top: 15px;
}
</style>
