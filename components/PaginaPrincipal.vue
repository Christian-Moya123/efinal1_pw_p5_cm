<template>
    <div class="container">
        <div v-if="!mostrarMensaje" class="juego">
            <div class="texto">
                <div class="contadorPuntaje">
                    <h1>Puntaje: {{ puntaje }}</h1> 
                </div>
                <div class="contadorIntentos">
                    <h1>Intentos: {{ intentos }}</h1> 
                </div>
            </div>
            <div class="imagenes">
                <Imagen :nombre="nombres[0]" :imagen="imagenes[0]" />
                <Imagen :nombre="nombres[1]" :imagen="imagenes[1]" />
                <Imagen :nombre="nombres[2]" :imagen="imagenes[2]" />
            </div>
            <button @click="jugar">JUGAR</button>
        </div>

        <div v-if="mostrarMensaje" class="mensajes">

            <div v-if="ganar" class="Ganar">
                <h2>
                    Puntaje: {{ puntaje }}
                </h2>
                <h2>Felicitaciones has ganado un premio de $10.000,00</h2>
                <button @click="reiniciar">Nuevo Juego</button>
            </div>

            <div v-if="perder" class="Perder">
                <h2>Has utilizado tus 5 intentos</h2>
                <h2>El juego ha terminado, intentalo nuevamente</h2>
                <button @click="reiniciar">Nuevo Juego</button>
            </div>
            
        </div>
    </div>
</template>

<script>

import Imagen from "./Imagen.vue"

export default {
    components: { Imagen },
    data() {
        return {
            puntaje: 0,
            intentos: 0,
            ganar: false,
            perder: false,
            imagenes: [
                "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
                "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
                "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",
            ],
            nombres: ["xxxxxxxx", "xxxxxxxx", "xxxxxxxx"],
            mostrarMensaje: false,
        };
    },

    methods: {
        async jugar() {
            if (this.intentos < 5) {
                for (let i = 0; i < 3; i++) {
                    const { answer, image } = await fetch("https://yesno.wtf/api").then((respuesta) => respuesta.json());

                    this.imagenes[i] = image;
                    this.nombres[i] = answer;
                }

                const contadorYes = this.nombres.filter((respuesta) => respuesta === "yes").length;

                if (contadorYes === 3) {
                    this.puntaje += 5;
                    console.log("es 5")
                } else if (contadorYes === 2) {
                    this.puntaje += 2;
                    console.log("es 2")
                } else if (contadorYes === 1) {
                    this.puntaje += 1;
                    console.log("es 1")
                }

                this.intentos++;

                if (this.puntaje >= 10) {
                    this.ganar = true;
                    this.mostrarMensaje = true;
                } else if (this.intentos === 5 && this.puntaje < 10) {
                    this.perder = true;
                    this.mostrarMensaje = true;
                }
            }
        },

        reiniciar() {

            this.puntaje = 0;
            this.intentos = 0;
            this.ganar = false;
            this.perder = false;
            this.imagenes = [
                "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
                "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
                "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",
            ];
            this.nombres = ["xxxxxxxx", "xxxxxxxx", "xxxxxxxx"];
            this.mostrarMensaje = false;
        },
    }

}
</script>

<style>
.container {
    display: flex;
    justify-content: center;
    align-items: center;
}

.imagenes {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 40px;
    width: 750px;
    gap: 40px;

}

.texto {
    display: flex;
    font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
    gap: 40px;
}

.juego {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
}



button {
    width: 100px;
    height: 50px;

}

.Perder {
    color: red;
}

.Ganar {
    color: blue;

}
</style>