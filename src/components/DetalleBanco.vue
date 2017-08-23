<template>
    <div id="panel">
        <div class="linea">
            <label>Cuenta Bancaria: </label>
            <input v-if="datapadre.currentCuenta != null" name="cuenta" type="text" v-model="datapadre.currentCuenta.Cuenta" @input="validarCuenta" placeholder="Introduce Cuenta Bancaria">
            <input v-else name="cuenta" type="text" v-model="datapadre.newCuenta.Cuenta" @input="validarCuenta" placeholder="Introduce Cuenta Bancaria">
        </div>
        <div class="linea">
            <label>Banco: </label>
            <input v-if="datapadre.currentCuenta != null" name="banco" type="text" v-model="datapadre.currentCuenta.Banco" @input="validarCuenta" placeholder="Introduce Banco">
            <input v-else name="banco" type="text" v-model="datapadre.newCuenta.Banco" @input="validarCuenta" placeholder="Introduce Banco">
        </div>
        <div class="linea">
            <label>Saldo: </label>
            <input v-if="datapadre.currentCuenta != null" name="saldo" type="number" v-model="datapadre.currentCuenta.Saldo" @input="validarCuenta" placeholder="Introduce Saldo">
            <input v-else name="saldo" type="number" v-model="datapadre.newCuenta.Saldo" @input="validarCuenta" placeholder="Introduce Saldo">
        </div>
        <div class="linea">
            <label>Activa: </label>
            <input name="activa" type="checkbox" v-model="datapadre.newCuenta.Activa" @input="validarCuenta">
        </div>
        <div id="botones">
            <button @click="addCuenta" :disabled="datapadre.currentCuenta || !isValid">Añadir</button>
            <button @click="updateCuenta" :disabled="!datapadre.currentCuenta || !datapadre.editMode || !isValid">Actualizar</button>
            <button @click="deleteCuenta" :disabled="!datapadre.currentCuenta">Borrar</button>
        </div>

    </div>
</template>

<script>

export default {
    name: 'detalleBanco',
    props: ['datapadre'],
    data() {
        return { isValid: false };
    },
    methods: {
        validarCuenta: function () {
            var cuentaToValidate = this.datapadre.currentCuenta ? this.datapadre.currentCuenta : this.datapadre.newCuenta;
            var _isValid = true;
            if (!(cuentaToValidate.Cuenta.length > 0 && cuentaToValidate.Cuenta.length <= 20)) _isValid = false;
            if (!(cuentaToValidate.Banco.length > 0 && cuentaToValidate.Banco.length <= 20)) _isValid = false;
            this.isValid = _isValid;
        },
        addCuenta: function () {
            let _this = this.datapadre;
            $.ajax({
                type: 'POST',
                url: 'http://localhost:50406/api/CuentaBancarias/',
                data: _this.newCuenta,
                success: function (response) {
                    _this.cuenta.push(response);
                    _this.newCuenta = { Cuenta: "", Banco: "", Saldo: "", Activa: "" };
                    _this.backup = JSON.parse(JSON.stringify(_this.cuenta));
                }
            });
        },
        updateCuenta: function () {
            let _this = this.datapadre;
            $.ajax({
                type: 'PUT',
                url: 'http://localhost:50406/api/CuentaBancarias/' + _this.currentCuenta.Id,
                data: _this.currentCuenta,
                success: function (response) {
                    var index = _this.backup.findIndex((el) => el.Id == _this.currentCuenta.Id);
                    _this.cuenta[index] = _this.currentCuenta;
                    _this.backup = JSON.parse(JSON.stringify(_this.cuenta));
                    _this.currentCuenta = null;
                }
            });

        },
        deleteCuenta: function () {
            let _this = this.datapadre;
            $.ajax({
                type: 'DELETE',
                url: 'http://localhost:50406/api/CuentaBancarias/' + _this.currentCuenta.Id,
                success: function (response) {
                    var index = _this.backup.findIndex((el) => { el.Id == _this.currentCuenta.Id });
                    _this.cuenta.splice(index, 1);
                    _this.backup = JSON.parse(JSON.stringify(_this.cuenta));
                    _this.currentCuenta = null;
                }
            });
        },
        error: function (xhr, textStatus, errorThrown) {
            alert("Error!->" + errorThrown + "-->" + xhr.responseText);
        }
    },
    created() {
        this.validarCuenta();
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
