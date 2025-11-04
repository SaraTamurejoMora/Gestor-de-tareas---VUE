
<template>
<header>
  <h1>Gestor de Tareas</h1>
</header>
<main>
  <div>
    <h2>Tareas</h2>
    <input v-model="nombreTarea" placeholder="Nombre de la tarea"></input>
    <button @click="añadirTarea" class="añadir">Añadir</button>

    <input type="checkbox" v-model="mostrarPendientes" id="mostrarPendientes" />
    <label for="mostrarPendientes">Mostrar solo pendientes</label>

    <div v-for="tarea in tareasFiltradas" :key="tarea.titulo">
      <span :class="{ completada: tarea.estado === 'completada' }">
        {{ tarea.titulo }}
      </span> 
      <button @click="completar(tarea)">{{ tarea.estado === 'pendiente' ? 'Completar' : 'Reabrir' }}</button>
      <button  @click="eliminarTarea(tarea)" class="eliminar">Eliminar</button>
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

//Mostrar solo tareas pendientes con el checkbox
const mostrarPendientes = ref(false)
const tareasFiltradas = computed(() => {
  if (mostrarPendientes.value) {
    return listaTareas.value.filter(tarea => tarea.estado === "pendiente")
  }
  return listaTareas.value
})
</script>

<style scoped>
/*Para tachar las tareas*/
.completada {
  text-decoration: line-through;
  color: #888;
};

body {
  background-color: #121212;
  font-family: 'Poppins', sans-serif;
  color: #e0e0e0;
  margin: 0;
  padding: 0;
}

/* Encabezado */
header {
  background-color: #1f1f1f;
  padding: 1rem;
  text-align: center;
  color: #ffffff;
  border-bottom: 2px solid #333;
}

/* Contenedor principal */
main {
  display: flex;
  justify-content: center;
  margin-top: 2rem;
}

main > div {
  background-color: #1e1e1e;
  border-radius: 10px;
  padding: 1.5rem;
  width: 100%;
  max-width: 500px;
  box-shadow: 0 0 10px rgba(0,0,0,0.3);
}

/* Título */
h2 {
  color: #81a1c1;
  border-bottom: 1px solid #333;
  padding-bottom: 6px;
  margin-bottom: 10px;
  font-weight: 500;
}

/* Inputs */
input[type="text"] {
  width: 100%;
  padding: 8px;
  border: 1px solid #444;
  border-radius: 6px;
  background-color: #2a2a2a;
  color: #f1f1f1;
  margin-bottom: 10px;
}

input[type="text"]:focus {
  outline: none;
  border-color: #81a1c1;
}

/* Checkbox y label */
input[type="checkbox"] {
  accent-color: #81a1c1;
}

label {
  margin-left: 5px;
  color: #ccc;
}

/* Botones */
button {
  margin: 5px;
  padding: 8px 12px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  color: white;
  background-color: #3b82f6;
  transition: background 0.2s;
}

button:hover {
  background-color: #2563eb;
}

/* Lista de tareas */
div > div {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #2a2a2a;
  padding: 8px 10px;
  border-radius: 6px;
  margin: 6px 0;
  color: #ffffffff;
}

span {
  flex-grow: 1;
  text-align: left;
  font-size: 1rem;
  color: #00b894;
}

/* Tarea completada */
.completada {
  text-decoration: line-through;
  color: #888;
}

/* Totales */
p {
  font-weight: 500;

  color: #ffffffff;
}
.eliminar{
  background-color: red;
}

.añadir{
  background-color: #00b894;
}

</style>