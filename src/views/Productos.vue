<template>
  <div class="agregar">
    <h1>Listado de Productos</h1>
    <Formulario />
  </div>
</template>

<script>
import Formulario from '@/components/Formulario.vue'

export default {
  name: "Productos",
  components: {
    Formulario
  }
}
</script>