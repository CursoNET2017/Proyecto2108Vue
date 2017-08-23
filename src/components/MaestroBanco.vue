<template>
    <div id="maestro">

        <h1>Cuenta Bancaria</h1>
        <button @click="nuevaCuenta">+Nueva Cuenta</button>
        <ul>
            <li v-for="cuenta in datapadre.backup" @click="selectCuenta(cuenta)">
                {{cuenta.Banco}},{{cuenta.Cuenta}} - {{cuenta.Saldo}} €
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
        getCuenta: function () {
            let _this = this;
            $.ajax({
                type: 'GET',
                url: 'http://localhost:50406/api/CuentaBancarias/',
                success: function (response) {
                    _this.datapadre.backup =JSON.parse(JSON.stringify(response));
                    _this.datapadre.cuenta =JSON.parse(JSON.stringify(response));
                }
            });

        },
        selectCuenta: function (cuenta) {
            this.datapadre.editMode = true;
            this.datapadre.currentCuenta = JSON.parse(JSON.stringify(cuenta));
            this.datapadre.newCuenta = { Cuenta: "", Banco: "", Saldo: "", Activa: "" };
        },
        nuevaCuenta: function (cuenta) {
            this.datapadre.editMode = false;
            this.datapadre.currentCuenta = null;
        },
        error: function (xhr, textStatus, errorThrown) {
            alert("Error!->" + errorThrown + "-->" + xhr.responseText);
        }
    },
    created() {
        this.getCuenta();
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
