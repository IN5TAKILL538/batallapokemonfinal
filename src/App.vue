<script setup>
import axios from "axios";
import { ref } from "vue";


let nombre = ref("");
let image = ref("");
let numpokedex = ref("");
let stats = ref("");

let nombre2 = ref("");
let image2 = ref("");
let numpokedex2 = ref("");
let stats2 = ref("");

let numeros1 = ref([]);
let numeros2 = ref([]);
let numi = ref(0); // Cambia a ref
let i = ref(0); // Cambia a ref

let statshp = ref("");
let statsatk = ref("");
let statsdf = ref("");
let statsatksp = ref("");
let statsdfsp = ref("");
let statsspeed = ref("");

let statshp2 = ref("");
let statsatk2 = ref("");
let statsdf2 = ref("");
let statsatksp2 = ref("");
let statsdfsp2 = ref("");
let statsspeed2 = ref("");

let contador1 = 0
let contador2 = 0
let pokeganador = ref();
let entrenadorganador = ref("")

function numerospokedex() {
  numeros1.value = []; // Limpia el arreglo antes de llenarlo
  numeros2.value = [];

  for (let j = 0; j < numi.value; j++) {
    const numeroAleatorio1 = Math.floor(Math.random() * 1000) + 1;
    numeros1.value.push(numeroAleatorio1);
    const numeroAleatorio2 = Math.floor(Math.random() * 1000) + 1;
    numeros2.value.push(numeroAleatorio2);
    console.log(numeroAleatorio1);
    console.log(numeroAleatorio2);
  }

}

async function listarPokemon() {
  document.body.style.height = "auto";
  document.getElementById("cuerpo").style.display = "flex"
  document.getElementById("header").style.display = "none"
  document.getElementById("nextcombat").style.display = "block"
  document.getElementById("resultadolocal").style.display = "block"
  document.getElementById("resultados").style.display = "flex"


  // Obtener el número de Pokémon a mostrar
  numi.value = parseInt(document.getElementById("numpokemon").value); // Asegúrate de que numi sea un número
  console.log(numi.value);

  document.getElementById("contador").style.display = "flex"
  document.getElementById("contador2").style.display = "flex"



  // Llenar los números aleatorios
  numerospokedex();

  // Asegúrate de que i esté dentro del rango
  if (i.value < numi.value) {
    let url1 = 'https://pokeapi.co/api/v2/pokemon/' + numeros1.value[i.value];
    let url2 = 'https://pokeapi.co/api/v2/pokemon/' + numeros2.value[i.value];
    try {
      let { data } = await axios.get(url1);
      console.log(data);

      nombre.value = data.name;
      numpokedex.value = data.id;
      image.value = data.sprites.front_default;
      statshp.value = data.stats[0].base_stat;
      statsatk.value = data.stats[1].base_stat;
      statsdf.value = data.stats[2].base_stat;
      statsatksp.value = data.stats[3].base_stat;
      statsdfsp.value = data.stats[4].base_stat;
      statsspeed.value = data.stats[5].base_stat;
      stats.value = statsatk.value + statsdf.value + statsatksp.value + statsdfsp.value + statsspeed.value;

      let { data: data2 } = await axios.get(url2);
      nombre2.value = data2.name;
      numpokedex2.value = data2.id;
      image2.value = data2.sprites.front_default;
      statshp2.value = data2.stats[0].base_stat;
      statsatk2.value = data2.stats[1].base_stat;
      statsdf2.value = data2.stats[2].base_stat;
      statsatksp2.value = data2.stats[3].base_stat;
      statsdfsp2.value = data2.stats[4].base_stat;
      statsspeed2.value = data2.stats[5].base_stat;
      stats2.value = statsatk2.value + statsdf2.value + statsatksp2.value + statsdfsp2.value + statsspeed2.value;

      i.value++; // Incrementa i después de obtener los datos
      compararEstadisticas()

    } catch (error) {
      alert("Ocurrió un error al obtener los datos. Por favor, inténtalo de nuevo.");
    }
  } else {
    if (contador1 > contador2) {
      entrenadorganador.value = "el Ganador es el entrendor #1"
    }
    else if (contador1 < contador2) {
      entrenadorganador.value = "el Ganador es el entrendor #2"
    }
    else {
      entrenadorganador.value = "EMPATE"
    }
    document.getElementById("nextcombat").textContent = "Resultado final"
    document.getElementById("resultadoglobal").style.display = "block"
    document.getElementById("resetear").style.display = "block"
    
  }
  function compararEstadisticas() {
    const tipoBatalla = document.getElementById("batalla").value;

    let statPokemon1, statPokemon2;
    let ganador;

    switch (tipoBatalla) {
      case "total":
        statPokemon1 = parseInt(stats.value);
        statPokemon2 = parseInt(stats2.value);
        break;
      case "hp":
        statPokemon1 = parseInt(statshp.value);
        statPokemon2 = parseInt(statshp2.value);
        break;
      case "attack":
        statPokemon1 = parseInt(statsatk.value);
        statPokemon2 = parseInt(statsatk2.value);
        break;
      case "defense":
        statPokemon1 = parseInt(statsdf.value);
        statPokemon2 = parseInt(statsdf2.value);
        break;
      case "special-attack":
        statPokemon1 = parseInt(statsatksp.value);
        statPokemon2 = parseInt(statsatksp2.value);
        break;
      case "special-defense":
        statPokemon1 = parseInt(statsdfsp.value);
        statPokemon2 = parseInt(statsdfsp2.value);
        break;
      case "speed":
        statPokemon1 = parseInt(statsspeed.value);
        statPokemon2 = parseInt(statsspeed2.value);
        break;
      default:
        return; // Si no hay selección válida, salir
    }

    if (statPokemon1 > statPokemon2) {
      ganador = nombre.value + ' gana con ' + statPokemon1 + ' ' + tipoBatalla + ' sobre ' + statPokemon2 + ' de ' + nombre2.value;
      contador1++
    } else if (statPokemon1 < statPokemon2) {
      ganador = nombre2.value + ' gana con ' + statPokemon2 + ' ' + tipoBatalla + ' cotra ' + statPokemon1 + ' de ' + nombre.value;
      contador2++
    } else {
      ganador = "Es un empate! tienes otro intento";

    }
    pokeganador.value = ganador
    // Muestra el resultado en un alert
  }
}
async function reset() {
  let mostrar = ref(true)
  let nomostrar = ref(true)

  nombre = ref("");
  image = ref("");
  numpokedex = ref("");
  stats = ref("");

  nombre2 = ref("");
  image2 = ref("");
  numpokedex2 = ref("");
  stats2 = ref("");

  numeros1 = ref([]);
  numeros2 = ref([]);
  numi = ref(0); // Cambia a ref
  i = ref(0); // Cambia a ref

  statshp = ref("");
  statsatk = ref("");
  statsdf = ref("");
  statsatksp = ref("");
  statsdfsp = ref("");
  statsspeed = ref("");

  statshp2 = ref("");
  statsatk2 = ref("");
  statsdf2 = ref("");
  statsatksp2 = ref("");
  statsdfsp2 = ref("");
  statsspeed2 = ref("");

  contador1 = 0
  contador2 = 0
  pokeganador = ref();
  entrenadorganador = ref("")
  document.getElementById("header").style.display = "flex"
  document.getElementById("iniciar").style.display = "block"
  document.getElementById("cuerpo").style.display = "none"
  document.getElementById("poke1").style.display = "none"
  document.getElementById("poke2").style.display = "none"
  document.getElementById("resultados").style.display = "none"
  document.body.style.width = "100%"
   document.body.style.height = "100vh"
}
</script>

<template>
  <div class="contenedor">
    <div class="header" id="header">

      <select name="numpokemon" id="numpokemon">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
        <option value="6">6</option>
      </select>
      <button id="iniciar" @click="listarPokemon()">INICIAR</button>
    </div>
    <div class="cuerpo" id="cuerpo">
      <div class="selecion">

        <select name="tipobatalla" id="batalla">
          <option value="total">TOTAL</option>
          <option value="hp">HP</option>
          <option value="attack">ATAQUE</option>
          <option value="defense">DEFENSA</option>
          <option value="special-attack">ATAQUE ESPECIAL</option>
          <option value="special-defense">DEFENSA ESPECIAL</option>
          <option value="speed">VELOCIDAD</option>
        </select>
      </div>
      <div class="pokemon" id="poke1">
        <div class="contador" id="contador">
          <h1 class="txtcontador">{{ contador1 }}</h1>
        </div>
        <div class="contenedorname"><div id="name1">{{ nombre }}</div></div>
        <img :src="image" alt="" class="imgpoke">
      </div>
      <div class="pokemon"><img src="https://i.pinimg.com/originals/38/e6/8c/38e68c4baccbd17cc50aa752810a1301.gif"
          alt="" id="vs"></div>
      <div class="pokemon" id="poke2">
        <div class="contador" id="contador2">
          <h1 class="txtcontador">{{ contador2 }}</h1>
        </div>
        <div class="contenedorname"><div id="name2">{{ nombre2 }}</div></div>
        <img :src="image2" alt="" class="imgpoke">
      </div>

    </div>
    <div class="resultados" id="resultados">
      <div class="btnnext"><button id="nextcombat" @click="listarPokemon()">Siguiente Combate</button></div>
      <button id="resultadolocal">{{ pokeganador }}</button> <br>
      <button id="resultadoglobal">{{ entrenadorganador }}</button>
      <button id="resetear" @click="reset()">RESET</button>
    </div>
  </div>
</template>

<style>
body {
  height: 100vh;
  width: 100%;
  background-image: url(./img/estadio.jpeg);
  background-position: center;
  background-size: cover;
}

.contador {
  display: none;
  align-items: center;
  justify-content: center;
  font-family: cursive;
}
.txtcontador{
  font-family: cursive;
}
.contenedorname{
  display: flex;
  width: 100%;
  align-items: center ;
  justify-content: center;
  text-align: center;
}
.pokemon{
  align-items: center ;
  justify-content: center;
  text-align: center;
}

.contenedor {
  height: 100%;
  width: 100%;
}

.selecion {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: center;
}

#name1{
  align-items: center;
  font-family: cursive;
  background-color: black;
  border-radius: 50%;
  opacity: 0.7;
  width: 120px;
}
#name2{
  font-family: cursive;
  background-color: black;
  border-radius: 50%;
  opacity: 0.7;
  width: 120px;
  }
#vs {
  height: 60px;
  width: 60px;
  opacity: 0.6;
}

.cuerpo {
  display: none;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
}
.resultados {
  display: none;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
}


.header {

  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}

.imgpoke {
  width: 260px;
  height: 260px;
}

#numpokemon {
  width: 100px;
  height: 20px;
}

#batalla {
  width: 100px;
  height: 20px;
}

#nextcombat {
  display: none;
  font-family: cursive;

}

#resetear {
  display: none;
}

.btnnext {
  display: flex;
  justify-content: center;
  align-items: center;
}

@media (max-width: 660px) {

  .imgpoke {
    width: 120px;
    height: 120px;
  }
}

#resultadolocal {
  display: none;
  font-family: cursive;
  background-color: black;
  color: white;
}

#resultadoglobal {
  display: none;
  font-family: cursive;
  background-color: black;
  color: white;
}
</style>