<template>
  <div>
    <h1 v-show="isLoading">
      Cargando personaje ...
    </h1>
    <div v-show="!isLoading" class="card">
      <div class="card-body">
        <h1>Detalle Personaje:</h1>
        <table v-show="!isLoading">
          <tr>
            <th>Nombre</th>
            <td>{{ character.name }}</td>
          </tr>
          <tr>
            <th>De la casa</th>
            <td>{{ character.house }}</td>
          </tr>
          <tr>
            <th>Titulos</th>
            <td v-html="titlesBadge"></td>
          </tr>
          <tr>
            <th>Libros:</th>
            <td v-html="booksBadge"></td>
          </tr>
          <tr>
            <th>Sexo</th>
            <td>{{ character.male ? 'Masculino' : 'Femenino'  }}</td>
          </tr>
          <tr>
            <th>Fecha creación</th>
            <td>{{ character.createdAt }}</td>
          </tr>
        </table>
      </div>
    </div>
    <a v-show="!isLoading"  @click="goToBack" class="btn btn-link" href=""><i class="fa fa-arrow-left" aria-hidden="true"></i> Volver</a>
  </div>
</template>


<script>
  import {getACharacter} from '../services/got.service.js'

  export default {
    name: 'detail-component',

    /**
     * @description the data block represents all the local variable of this component.
     */
    data() {
      return {
        character: [],
        isLoading: false,
        booksBadge:'',
        titlesBadge:''
      }
    },

    /**
     * @description the create function is the first one to be execute when the component is being created (see vue js lifecycle).
     */
    created() {
      this.isLoading = true;
      console.log('route', this.$route.params.id);
      getACharacter(this.$route.params.id)
        .then(res => {
          this.character = res.data;
          this.isLoading = false;

          this.character.books.forEach((book) => {
            this.booksBadge += `<span class="badge badge-primary">${book}</span>  ` ;
          });
          this.character.titles.forEach((title) => {
            this.titlesBadge += `<span class="badge badge-primary">${title}</span>  ` ;
          });
        })
    },

    /**
     * @description the methods block represents all the local methods of this component.
     */
    methods: {
      goToBack() {
        this.$router.push({name: 'list'});
      }
    }

  }
</script>
<style>
  table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
  }

  td, th {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
  }

  tr:nth-child(even) {
    background-color: #dddddd;
  }
</style>
