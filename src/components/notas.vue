<template lang="html">

  <section class="notas">
    <div class="box">
      <form>
        <input type="text" v-on:keyup.enter="anadirElemento" v-on:keyup="teclaPulsada" v-model="input">
        <input type="button" v-bind:disabled="isButtonDisabled" v-on:click="anadirElemento" value="Enviar">
      </form>
    </div>
    <div class="cuerpo">
      <p>Fitro de tareas: <input v-on:keyup.enter="anadirElemento" v-on:keyup="teclaPulsada" v-model="empiezaPor"></p>

      <p>Hay {{tareasPendientes}} tareas en total </p>
      <button v-on:click="borrarTareasCompletadas">Borrar las tareas seleccioandas</button>

      <ol>
        <li v-on:click="cambiarEstadoTarea(index)" v-for="(todo, index) in filtrolistado" v-bind:key="todo+index" v-bind:class="{ completado: todo.completado}">
          {{ todo.titulo }} La tarea tiene prioridad {{todo.prioridad}}
        </li>
      </ol>
    </div>
    
  </section>

</template>

<script lang="js">

  export default  {
    name: 'notas',
    props: [],
    mounted () {
      if (localStorage.listaTareas) {
        this.todos = JSON.parse(localStorage.listaTareas);
      }
    },
    data () {
      return {
        input: "",
        todos: [],
        isButtonDisabled: true,
        empiezaPor: ""
      }
    },
    methods: {
      anadirElemento: function () {
        if (this.input.length > 0) {
          this.todos.push({
            titulo: this.input,
            prioridad: 0,
            fechaCreacion: new Date(),
            completado: false
          });
        this.input = "";
        this.actualizarLocalStorage();
        }
      },
      cambiarEstadoTarea: function (posicion) {
        this.todos[posicion].completado = !this.todos[posicion].completado;
        this.actualizarLocalStorage();
      },
      borrarTareasCompletadas: function () {
        this.todos = this.todos.filter(todo => !todo.completado);
        this.actualizarLocalStorage();
      },
      teclaPulsada: function () {
        if (this.input.length > 0) {
          this.isButtonDisabled = false;
        } else {
          this.isButtonDisabled = true;
        }
      },
      actualizarLocalStorage: function () {
        localStorage.listaTareas = JSON.stringify(this.todos);
      }
    },
    computed: {
      totalTareas: function (){
        return this.todos.lenght;
      },
      tareasCompletadas: function () {
        return this.todos.filter(todo => todo.completado).length;
      },
      tareasPendientes: function () {
        return this.todos.filter(todo => !todo.completado).length;
      },
      filtrolistado: function () {

        if (this.empiezaPor == "") {
          return this.todos;
        }
        let filtrolistado = this.todos.filter(todo => {
          if (todo.titulo.indexOf(this.empiezaPor) >= 0) {
            return true;
          } else {
            return false;
          }
        });

        filtrolistado = filtrolistado.sort( (todo1, todo2) => {
          if (todo1.titulo < todo2.titulo) {
            return -1;
          } else if (todo1.titulo > todo2.titulo) {
            return 1;
          } else {
            return 0;
          }
        });

        return filtrolistado;
      }
    }
  }


</script>

<style scoped>

</style>
