<template>
<h1>Predicción Meteorológica</h1>
<div class="container">
  <select v-model="provincia" @change="seleccionado">
    <option v-for="(pro, i) in provincias" :key="i" :value="pro.id"> {{ pro.nm }}</option>
  </select>
  <select v-model="municipio" @change="tiempo">
    <option v-for="(mun,i) in municipios" :key="i" :value="mun.COD_GEO"> {{ mun.NOMBRE }}</option>
  </select>
  <iframe v-if="link != undefined" :src="link"></iframe>
</div>
</template>


<script>
import { reactive, ref } from 'vue'
export default {
  name: 'Municipios',
  setup() {
    let provincias = reactive([])
    let municipios = reactive([])
    let provincia = ref()
    let municipio = ref()
    let link = ref()

    fetch('https://raw.githubusercontent.com/IagoLast/pselect/master/data/provincias.json')
      .then(res => res.json())
      .then(data => data.forEach(element => provincias.push(element)))

    const seleccionado = () => {
      municipios.splice(0)
      fetch(`https://www.el-tiempo.net/api/json/v2/provincias/${provincia.value}/municipios`)
        .then(res => res.json())
        .then(data => data.municipios.forEach(item => municipios.push(item)))
    }

    const tiempo = () => {
      fetch(`https://www.el-tiempo.net/api/json/v1/provincias/${provincia.value}/municipios/${municipio.value}/weather`)
        .then(res => res.json())
        .then(data => {
          console.log(data)
          link.value = `${data.origen.enlace}#tabla_con_scroll`})
    }
    
    return {
    provincias,
    provincia,
    municipios,
    municipio,
    seleccionado,
    tiempo,
    link
    }
  }
}
</script>

<style lang="scss" scoped>
  .container {
    width: 70%;
    height: 350px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;

    select {
      width: 50%;
      margin-bottom: 10px;
      border-radius: 5px;
    }

    iframe {
      width: 95%;
      height: 300px;
      border: none;
      overflow:hidden;
    }
  }
</style>