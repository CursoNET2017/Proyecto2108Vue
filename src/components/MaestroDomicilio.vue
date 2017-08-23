<template>
    <div id="maestro">

        <h1>Domicilios</h1>
        <button @click="nuevaDomicilio">+Nuevo Domicilio</button>
        <ul>
            <li v-for="domicilio in datapadre.backup" @click="selectDomicilio(domicilio)">
                {{domicilio.Portal}},{{domicilio.Calle}} - {{domicilio.Piso}} años
            </li>
        </ul>

    </div>
</template>

<script>

export default {
    name: 'maestro-domicilio',
    props:
    ['datapadre'],
    data() {
        return {};
    },
    methods: {
        getDomicilios: function () {
            let _this = this;
            $.ajax({
                type: 'GET',
                url: 'http://localhost:50406/api/Domicilios/',
                success: function (response) {
                    _this.datapadre.backup =JSON.parse(JSON.stringify(response));
                    _this.datapadre.domicilios =JSON.parse(JSON.stringify(response));
                }
            });

        },
        selectDomicilio: function (domicilio) {
            this.datapadre.editMode = true;
            this.datapadre.currentDomicilio = JSON.parse(JSON.stringify(domicilio));
            this.datapadre.newDomicilio = { Calle: "", Portal: "", Piso: "", CP: "", Localidad: "", Region: "" };
        },
        nuevaDomicilio: function (domicilio) {
            this.datapadre.editMode = false;
            this.datapadre.currentDomicilio = null;
        },
        error: function (xhr, textStatus, errorThrown) {
            alert("Error!->" + errorThrown + "-->" + xhr.responseText);
        }
    },
    created() {
        this.getDomicilios();
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
