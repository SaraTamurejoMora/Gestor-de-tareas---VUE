
<template>
<header>
  <h1>Gestor de Tareas</h1>
</header>
<main>
  <div>
    <h2>Tareas</h2>
    <input v-model="nombreTarea" placeholder="Nombre de la tarea"></input>
    <button @click="añadirTarea">Añadir</button>

    <input type="checkbox" v-model="mostrarPendientes" id="mostrarPendientes" />
    <label for="mostrarPendientes">Mostrar solo pendientes</label>

    <div v-for="tarea in listaTareas" :key="listaTareas.titulo">
      <span :class="{ completada: tarea.estado === 'completada' }">
        {{ tarea.titulo }}
      </span> 
      <button @click="completar(tarea)">{{ tarea.estado === 'pendiente' ? 'Completar' : 'Reabrir' }}</button>
      <button  @click="eliminarTarea(tarea)">Eliminar</button>
    </div>

    <div>
      <p>Totales: {{ tareasTotales }}</p> | Pendientes: {{ totalPendientes }}</div>

  </div>

</main>
</template>

<script setup>
import { ref, computed} from 'vue'
//Lista tareas
  const listaTareas = ref([
    {titulo: "Acabar proyecto VUE", estado: "pendiente"},
    {titulo: "Estudiar examen REACT", estado: "pendiente"},
    {titulo: "Subir blog inglés", estado: "completada"},
    {titulo: "Hacer practica redes", estado: "completada"}
  ])

  //Contadores de tareas
  const tareasTotales = computed(() => listaTareas.value.length)
  const tareasPendientes = computed(() => listaTareas.value.filter(tarea =>tarea.estado === "pendiente"))
  const totalPendientes = computed(() => tareasPendientes.value.length)


  //Completar tarea
  const completar = (tarea) => {
    tarea.estado = tarea.estado === 'pendiente' ? 'completada' : 'pendiente'
  }

//Añadir tareas 
//TODO:Falta hacer que no se borre al recargar pagina
const nombreTarea= ref('')
const añadirTarea = () => {
  const titulo = nombreTarea.value.trim()
  if (!titulo) return
  listaTareas.value.push({ titulo, estado: 'pendiente' })
  nombreTarea.value = ''
}

//Eliminar tareas
const eliminarTarea = (tarea) => {
  listaTareas.value = listaTareas.value.filter(t => t !== tarea)
}
</script>

<style scoped>
.completada {
  text-decoration: line-through;
  color: #888;
}
</style>

