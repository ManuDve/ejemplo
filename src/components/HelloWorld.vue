<template>
  <section v-if="nuevasNovelas">
    <div v-for="novela in nuevasNovelas" :key="novela.id">
      <h2>{{ novela.title }}</h2>
      <img v-if="novela.images[0]" :src="novela.images[0].path + '.jpg'" alt="">
      <img v-else :src="novela.thumbnail.path + '.jpg'" alt="">
      <p>Paginas: {{ novela.pageCount }}</p>
      <p>ISBN: {{ novela.isbn }}</p>
      <p><strong>Descripción: </strong> {{ novela.description }}</p>
      <hr>
    </div>
  </section>
</template>

<script>
import { MD5 } from "md5-js-tools";
export default {
  data() {
    return {
      nuevasNovelas: null
    }
  },
  async created() {
    try {
      let marvelApiStart = "https://gateway.marvel.com/v1/public/comics?format=graphic%20novel&apikey=";
      const marvelPublicKey = '701c9e424072247e27307a909cfab7ae';
      const marvelPrivateKey = '61397ba16153cf7360d94edd0d6363be33f291ec';
      let ts = new Date().getTime();
      let hash = MD5.generate(ts + marvelPrivateKey + marvelPublicKey);
      let requestUrl = marvelApiStart + marvelPublicKey + "&ts=" + ts + "&hash=" + hash
      let promesa = await fetch(requestUrl);
      let datos = await promesa.json();
      let novelas = await datos.data.results
      this.nuevasNovelas = novelas;
      console.log(novelas)
    } catch (error) {
      console.log(error)
    }
  }
}
</script>

<style scoped>

section {
  max-width: 1200px;
  margin: auto;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  gap: 1em;
}

div {
  max-width: 500px;
  font-size: 0.9rem;
  border: 1px solid #e9e9e9;
  padding: 1em;
  border-radius: 5px;
}

img {
max-width: 400px;
}

</style>