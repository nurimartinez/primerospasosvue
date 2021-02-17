<template>
  <div class="main-table">
    <div class="main-form">
    <div class="form">
      <input type="text" v-model="producto" placeholder="Producto">
      <input type="number" v-model="precio" placeholder="Precio">
      <input type="number" v-model="cantidad" placeholder="Cantidad">
      <button @click="agregar">Agregar</button>
    </div>
    </div>
    <table>
      <thead>
        <tr>
          <th class="bg-gray col-2">Producto</th>
          <th class="bg-gray col-1">Cantidad</th>
          <th class="bg-gray col-1">Precio</th>
          <th class="bg-gray col-1">Subtotal</th>
          <th class="bg-gray col-1">Eliminar</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="producto in productos" :key="producto.index">
          <td class="col-2">{{ producto.concepto }}</td>
          <td class="col-1">{{ producto.precio }}</td>
          <td class="col-1"><input type="number" v-model="producto.cantidad"></td>
          <td class="col-1">{{producto.precio * producto.cantidad}}</td>
          <td class="col-1"><div class="centre"><img src="../assets/delete.png" @click="eliminar(producto.index)"></div></td>
        </tr>
      </tbody>
      <tfoot>
        <tr>
          <td class="bg-green" colspan="6">TOTAL : {{ sumar }} </td>
        </tr>
      </tfoot>
    </table>
  </div>
</template>

<script>
import { computed, reactive, ref } from 'vue'
export default {
  name: 'Formulario',
  setup(){
    let producto = ref()
    let cantidad = ref()
    let precio = ref()
        
    let productos = reactive([
      {
        concepto: 'Pantalones vaqueros chico',
        cantidad: 2,
        precio: 30.99
      },
      {
        concepto: 'Camiseta básica chico',
        cantidad: 4,
        precio: 6.99
      },
      {
        concepto: 'Pijama Unisex',
        cantidad: 1,
        precio: 12.50
      },
      {
        concepto: 'Deportivas Nike',
        cantidad: 1,
        precio: 80.00
      }
    ])

    const agregar = () => {
      let newItem = {}
      newItem.concepto = producto.value
      newItem.cantidad = cantidad.value
      newItem.precio = precio.value
      productos.push(newItem)
    }

    const eliminar = (index) => {
      productos.splice(index, 1)
    }

    const sumar = computed ( () => {
      let suma = 0
      productos.map(producto => suma += (producto.precio * producto.cantidad))
      return suma.toFixed(2)
    });
    

    return {
      productos,
      sumar,
      agregar,
      producto,
      cantidad,
      precio,
      eliminar
    }
  }
}
</script>

<style lang="scss" scoped>
  
  * {
    box-sizing: border-box;
  }

  .form {
    width: 90%;
    max-width: 800px;
    margin: 0 auto;
    padding: 10px;
    background-color: rgb(243, 217, 70);
    border: 1px solid black;
    margin-bottom: 1em;
    display: flex;
    justify-content: space-around;

    input {
      line-height: 1.5em;
      width: 27%;
      background-color: rgba(255, 255, 255, 0.39);
      border: none;
    }
  }
  
  table {
    width: 90%;
    max-width: 800px;
    margin: 0 auto;

    input {
    text-align: right;
  }
  }

  td, th {
    border: 1px solid black;
  }

  td {
    font-size: 0.9rem;
    line-height: 1.5rem;
  }  

  img {
    max-width: 20%;
    line-height: 1.5rem;
  }

  .col-1 {
    width: 12.5%;
  }

  .col-2 {
    width: 50%;
  }

  .bg-gray {
    background-color: gray;
    color: white;
    line-height: 1.5rem;
  }
  .bg-green {
    background-color: rgb(5, 173, 123);
    color: white;
    line-height: 1.5rem;
    font-weight: bold;
  }

  .centre {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>