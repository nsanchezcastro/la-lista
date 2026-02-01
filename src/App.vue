<script setup>
import { ref, watch, onMounted } from 'vue'

// 1. Cargamos los datos al iniciar
// Intentamos leer de localStorage; si no hay nada, usamos un array vacío
const datosGuardados = JSON.parse(localStorage.getItem('mi-lista') || '[]')
const lista = ref(datosGuardados)

const nuevoProducto = ref('')

function agregar() {
  if (nuevoProducto.value.trim() !== '') {
    lista.value.push(nuevoProducto.value)
    nuevoProducto.value = ''
    // Guardar después de agregar
    guardarEnLocalStorage()
  }
}

function borrar(indice) {
  lista.value.splice(indice, 1)
  // Guardar después de borrar
  guardarEnLocalStorage()
}

// 2. Función para guardar
function guardarEnLocalStorage() {
  // localStorage solo guarda texto, por eso usamos JSON.stringify
  localStorage.setItem('mi-lista', JSON.stringify(lista.value))
}

// El parámetro { deep: true } es porque estamos vigilando un Array
watch(lista, () => {
  guardarEnLocalStorage()
}, { deep: true })
</script>
<template>
  <div class="contenedor">
    <h1>🛒 ¿Dónde está la lista?</h1>

    <div class="entrada">
      <input 
        v-model="nuevoProducto" 
        @keyup.enter="agregar"
        placeholder="¿Qué necesitas comprar?"
      >
      <button @click="agregar">Añadir</button>
    </div>

    <ul>
      <li v-for="(item, indice) in lista" :key="indice">
        {{ item }}
        <button @click="borrar(indice)" class="btn-borrar">x</button>
      </li>
    </ul>

    <p v-if="lista.length === 0">¡La lista está vacía! 🎉</p>
  </div>
</template>
<style scoped>
.contenedor { font-family: sans-serif; max-width: 400px; margin: 20px auto; }
.entrada { display: flex; gap: 10px; margin-bottom: 20px; }
input { flex-grow: 1; padding: 8px; border: 1px solid #ccc; border-radius: 4px; }
button { background: #42b883; color: white; border: none; padding: 8px 15px; cursor: pointer; border-radius: 4px; }
ul { list-style: none; padding: 0; }
li { display: flex; justify-content: space-between; padding: 10px; border-bottom: 1px solid #eee; }
.btn-borrar { background: #ff6666; padding: 2px 8px; font-size: 12px; }
</style>