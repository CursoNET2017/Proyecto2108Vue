<template>
    <div id="panel">
        <div class="linea">
            <label>Calle: </label>
            <input v-if="datapadre.currentDomicilio != null" name="calle" type="text" v-model="datapadre.currentDomicilio.Calle" @input="validarDomicilio" placeholder="Introduce Calle">
            <input v-else name="calle" type="text" v-model="datapadre.newDomicilio.Calle" @input="validarDomicilio" placeholder="Introduce Calle">
        </div>
        <div class="linea">
            <label>Portal: </label>
            <input v-if="datapadre.currentDomicilio != null" name="portal" type="text" v-model="datapadre.currentDomicilio.Portal" @input="validarDomicilio" placeholder="Introduce Portal">
            <input v-else name="portal" type="text" v-model="datapadre.newDomicilio.Portal" @input="validarDomicilio" placeholder="Introduce Portal">
        </div>
        <div class="linea">
            <label>Piso: </label>
            <input v-if="datapadre.currentDomicilio != null" name="piso" type="number" v-model="datapadre.currentDomicilio.Piso" @input="validarDomicilio" placeholder="Introduce Piso">
            <input v-else name="piso" type="number" v-model="datapadre.newDomicilio.Piso" @input="validarDomicilio" placeholder="Introduce Piso">
        </div>
        <div class="linea">
            <label>CP: </label>
            <input v-if="datapadre.currentDomicilio != null" name="CP" type="text" v-model="datapadre.currentDomicilio.CP" @input="validarDomicilio" placeholder="Introduce CP">
            <input v-else name="CP" type="text" v-model="datapadre.newDomicilio.CP" @input="validarDomicilio" placeholder="Introduce CP">
        </div>
        <div id="botones">
            <button @click="addDomicilio" :disabled="datapadre.currentDomicilio || !isValid">Añadir</button>
            <button @click="updateDomicilio" :disabled="!datapadre.currentDomicilio || !datapadre.editMode || !isValid">Actualizar</button>
            <button @click="deleteDomicilio" :disabled="!datapadre.currentDomicilio">Borrar</button>
        </div>

    </div>
</template>

<script>

export default {
    name: 'detalleDomicilio',
    props: ['datapadre'],
    data() {
        return { isValid: false };
    },
    methods: {
        validarDomicilio: function () {
            var domicilioToValidate = this.datapadre.currentDomicilio ? this.datapadre.currentDomicilio : this.datapadre.newDomicilio;
            var _isValid = true;
            if (!(domicilioToValidate.Calle.length > 0 && domicilioToValidate.Calle.length <= 20)) _isValid = false;
            if (!(domicilioToValidate.Portal.length > 0 && domicilioToValidate.Portal.length <= 20)) _isValid = false;
            if (!(domicilioToValidate.Piso > 0 && domicilioToValidate.Piso <= 150)) _isValid = false;

            var regExpCP = /\d{8}[A-Za-z]$/;
            if (!regExpCP.test(domicilioToValidate.CP)) _isValid = false;
            this.isValid = _isValid;
        },
        addDomicilio: function () {
            let _this = this.datapadre;
            $.ajax({
                type: 'POST',
                url: 'http://localhost:50406/api/Domicilios/',
                data: _this.newDomicilio,
                success: function (response) {
                    _this.domicilios.push(response);
                    _this.newDomicilio = { Calle: "", Portal: "", Piso: "", CP: "", Localidad: "", Region: "" };
                    _this.backup = JSON.parse(JSON.stringify(_this.domicilios));
                }
            });
        },
        updateDomicilio: function () {
            let _this = this.datapadre;
            $.ajax({
                type: 'PUT',
                url: 'http://localhost:50406/api/Domicilios/' + _this.currentDomicilio.Id,
                data: _this.currentDomicilio,
                success: function (response) {
                    var index = _this.backup.findIndex((el) => el.Id == _this.currentDomicilio.Id);
                    _this.domicilios[index] = _this.currentDomicilio;
                    _this.backup = JSON.parse(JSON.stringify(_this.domicilios));
                    _this.currentDomicilio = null;
                }
            });

        },
        deleteDomicilio: function () {
            let _this = this.datapadre;
            $.ajax({
                type: 'DELETE',
                url: 'http://localhost:50406/api/Domicilios/' + _this.currentDomicilio.Id,
                success: function (response) {
                    var index = _this.backup.findIndex((el) => { el.Id == _this.currentDomicilio.Id });
                    _this.domicilios.splice(index, 1);
                    _this.backup = JSON.parse(JSON.stringify(_this.domicilios));
                    _this.currentDomicilio = null;
                }
            });
        },
        error: function (xhr, textStatus, errorThrown) {
            alert("Error!->" + errorThrown + "-->" + xhr.responseText);
        }
    },
    created() {
        this.validarDomicilio();
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
