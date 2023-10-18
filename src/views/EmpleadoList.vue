<template>
  <div id="appWrapper">
    <div class="container">
      <div class="search-box">
        <input 
          class="search-input" 
          type="text" name="q" 
          placeholder="Escriba un codigo o fecha..."
          v-model="query"
          @input="buscarEmpleado">       
        <ul class="result-list" :class="resultsVisibility">
          <li v-for="emple in empleados" class="result-item">
            <a href="#" class="result-link">
              <div class="result-title">{{emple.nombres}}</div>
              <div class="result-content">{{emple.apellidos}}</div>
            </a>
          </li>
        </ul>
      </div>
    </div>
  </div>
  <div class="row">
    <div class="col-lg-8 offset-lg-2">
      <div class="table-responsive">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>N°</th>
              <th>ID</th>
              <th>Nombres</th>
              <th>Apellidos</th>
              <th>Correo</th>
              <th>Telefono</th>
              <th></th>
            </tr>
          </thead>
          <tbody class="table-group-divider" id="contenido">
            <tr v-if="this.cargando">
              <td colspan="6"><h3>Cargando</h3></td>
            </tr>
            <tr v-else v-for="empl, i in empleados" :key="empl.id">
              <td v-text="(i+1)"></td>
              <td v-text="(empl.id)"></td>
              <td v-text="(empl.nombres)"></td>
              <td v-text="(empl.apellidos)"></td>
              <td v-text="(empl.email)"></td>
              <td v-text="(empl.telefono)"></td>
              <td>
                  <button type="button" class="btn btn-success" style="margin: 5px;">Detalles</button>
                  <button type="button" class="btn btn-danger">Eliminar</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from "axios";

  export default{
    data(){
      return{
        empleados: null,
        cargando: false,
        query:'',
        empleados: []
      }
    },
    
    mounted(){
      this.getEmpleados();
    },

    computed:{
      resultsVisibility(){
        return (this.query.length > 0) ? 'show' : 'hide';
      }
    },

    /*watch:{
      query(query){
        if(query===''){
          this.getEmpleados();
        }else if(query.length >= 3){
          this.buscarEmpleado();
        }
      }
    },*/
    methods:{
      buscarEmpleado(){
        this.empleados = null;
        axios.post("http://tienda.test/api/empleados/buscarEmpleado", {
          q: this.query
        }).then(res =>{
          this.empleados = res.data.empleados;
        }).catch(err =>{
          console.log(err.response);
        })
        //console.log(this.query);
      },

      getEmpleados(){
        this.cargando = true;
        this.empleados = null;
        axios.get("http://tienda.test/api/empleados")
        .then(res => {
          this.empleados = res.data.empleados;
          this.cargando = false;
        })
        .catch(err => {
          console.log(err);
          this.cargando = false;
        })
      }
    }
  }
</script>

<style>
  body{
    font-family: sans-serif;
  }

  ::-webkit-scrollbar{
    width: 10px;
  }

  ::-webkit-scrollbar-track{
    background: #f1f1f1;
  }

  ::-webkit-scrollbar-thumb{
    background: #888;
  }

  ::-webkit-scrollbar-thumb:hover{
    background: #555;
  }

  .container{
    padding: 30px;
  }
  .search-box{
    position: relative;
    display: flex;
    align-items: center;
    flex-direction: column;
  }

  .search-input{
    width: 600px;
    height: 30px;
    border-radius: 10px;
    border: 0;
    background: #eeeeee;
    padding: 10px 20px;
    font-size: 18px;
    outline: none;
  }

  .result-list.show{
    position: absolute;
    width: 640px;
    max-height: 250px;
    overflow-y: auto;
    list-style: none;
    background: #fff;
    padding: 0;
    top: 40px;
    border-radius: 10px;
    box-shadow: 1px 2px 8px 0px #b5b5b5;
  }

  .result-list.hide{
    display: none;
  }

  .result-item{
    border-bottom: 1px solid #ececec;
  }

  .result-link{
    text-decoration: none;
    color: #333;
    display: block;
    padding: 10px 15px;
  }

  .result-link:hover{
    background: #f9f9f9;
  }

  .result-title{
    font-size: 20px;
    font-weight: 600;
  }

  .result-content{
    font-size: 18px;
  }
</style>