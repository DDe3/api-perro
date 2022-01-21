<template>
  <img id="fondo" src="@/assets/fondo.jpg" alt="" />
  <div id="block_container">
    <div id="texto">
      <h1>Adopta una mascota</h1>
      <input
        type="number"
        min="0"
        v-model="precio"
        placeholder="Ingrese tu presupuesto"
        @focus="blurFondo"
        @blur="unBlurFondo"
      />
      <br />
      <p style="display: none" id="recomendacion">
        Ingrese su presupuesto en dólares
      </p>
      <button @click="buscarPerro">Buscar</button>
    </div>
    <div style="display: none" id="imagen">
      <h2>{{ respuesta }}</h2>
      <img id="perro" :src="url" alt="Error cargando imagen" />
    </div>
  </div>
</template>

<script>
export default {
  name: "Consultar",
  data() {
    return {
      precio: null,
      url: null,
      respuesta: null,
    };
  },
  methods: {
    blurFondo() {
      document.getElementById("recomendacion").style.display = "block";
      document.getElementById("imagen").style.display = "none";
      document.getElementById("fondo").style.opacity = 0.3;
      this.url = null
      this.respuesta = null
    },
    unBlurFondo() {
      document.getElementById("recomendacion").style.display = "none";
      document.getElementById("fondo").style.opacity = 1;
    },
    async buscarPerro() {
      if (this.precio != null) {
        if (this.precio >= 0) {
          document.getElementById("imagen").style.display = "block";
          this.respuesta = "Buscando perro ideal...";
          const data = await fetch(
            "https://dog.ceo/api/breeds/image/random"
          ).then((perro) => perro.json());
          const { message } = data;
          this.respuesta = "Con $" + this.precio + " puedes adoptar a: ";
          this.url = message;
        } else {
          document.getElementById("imagen").style.display = "none";
          this.precio = null;
        }
      }
    },
  },
};
</script>


<style>
#block_container {
  position: relative;
  white-space: nowrap;
  position: relative;
  z-index: 99;
  margin: auto;
}

#fondo {
  height: 100vh;
  width: 100vw;
  max-height: 100%;
  max-width: 100%;
  position: fixed;
  left: 0px;
  top: 0px;
  z-index: 98;
}

input {
  border-radius: 10px;
  padding: 10px 15px;
  width: 200px;
  margin: 0px 50px;
}

#perro {
  height: 50vh;
  width: 50vw;
}

button {
  padding: 10px;
  border-radius: 20px;
  background-color: cadetblue;
  width: 100px;
  font-size: 20px;
  margin: 10px;
}

button:hover {
  background-color: aqua;
}

h1,
h2 {
  color: aquamarine;
  text-shadow: 2px 2px black;
}

p {
  font-size: 10px;
  color: red;
  margin-top: 0px;
}
</style>