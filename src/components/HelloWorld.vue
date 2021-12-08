<template>
<div class="hello">
    <h1>{{msg}}</h1>
    <div class="DivBuscador-container">
        <form @submit.prevent="buscar" class="formBuscador">
            <div class="DivBuscador">
                <input @keypress="input" @keyup="buscar" v-model="buscador" class="InputBuscador" />
                <label class="lblBuscador">Buscar</label>
            </div>
        </form>
    </div>

    <div class="listadoDivContainer" v-if="listado.length > 0">
        <div class="listadoDiv" v-for="item in listado" :key="item.title">
            <h1>{{item.title}}</h1>
            <p>{{item.pageid}} </p>
            <p v-html="item.snippet"></p>
        </div>
    </div>

    <div v-else>
        <h1>No hay búsquedas </h1>
     </div>

</div>
</template>

<script>
import axios from "axios";
export default {
    name: 'HelloWorld',
    props: {
        msg: String
    },
    data() {
        return {
            buscador: '',
            listado: [],
            texto: ''
        }
    },
    methods: {
        buscar: function () {
            if (this.buscador.length >= 1) {
                var url = "https://en.wikipedia.org/w/api.php?";
                var params = "action=query" +
                    "&list=search" +
                    "&srsearch=" + this.buscador +
                    "&utf8=" +
                    "&format=json" +
                    "&origin=*";
                var path = url + params;
                console.log(path)
                axios.get(path).then((response) => {
                        console.log("Ya envío la respuesta y aceptó")
                        console.log(response.data.query.search)
                        this.listado = response.data.query.search
                        console.log(this.listado)
                    })
                    .catch((error) => {
                        console.log(error);
                    })
            } else {
                this.listado = []
            }

        },
        input: function () {
            var input = document.getElementsByClassName("InputBuscador");
            if (input != null) {
                for (var i = 0; i < input.length; i++) {
                    input[i].addEventListener("keyup", function () {
                        if (this.value.length >= 1) {
                            this.nextElementSibling.classList.add("fijar");
                        } else {
                            this.nextElementSibling.classList.remove("fijar");
                        }
                    })
                }
            }
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style src="@/assets/css/buscador.css"></style>
