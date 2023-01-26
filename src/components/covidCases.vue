<template>
  <h1>Covid Cases USA</h1>
  <h3>Codigo de Estados</h3>

  <!--<h4 v-show="mostarInicio">{{ this.construirTodosEstados() }}</h4>-->
  <ul>
    <li v-for="item in estados"> {{ item }}</li>
  </ul>

<input v-on:keyup.enter="consultar()"  v-model="consultar" type="text" placeholder="Ingrese codigo estado">

<div v-if="mostarcasos" class="resultados">
    <h3>Resultados</h3>
    <label for="">Casos Positivos:</label>
    <input type="text" :value="positive">
    <label for="">Total de resultado de Pruebas</label>
    <input type="text" :value="totalTestResults">
    <label for="">Actualmente Hospitalizados</label>
    <input type="text" :value="hospitalizedCurrently">
    <label for="">Muertos</label>
    <input type="text" :value="death">
    <label for="">Casos positivos virales</label>
    <input type="text" :value="totalTestsViral">
    <label for="">Numero de aumento de muertes</label>
    <input type="text" :value="deathIncrease">
</div>
</template>

<script>
export default {
    data() {
    return {
      mostarInicio:false,
      estados:null,
      estado:null,
      mostarcasos: true,
      positive: null,
      totalTestResults:null,
      hospitalizedCurrently:null,
      death:null,
      totalTestsViral:null,
      deathIncrease:null,
      consultar:null,
    };
  },
  watch:{
    async consultar(evento, old){
            console.log(evento)
            console.log(old)
              console.log('vamos conultar')  
             const {positive,totalTestResults,hospitalizedCurrently, death,  totalTestsViral, deathIncrease }= await this.consumirAPIestado(evento)
             this.positive=positive
             this.totalTestResults=totalTestResults
             this.hospitalizedCurrently=hospitalizedCurrently
             this.death=death
             this.totalTestsViral=totalTestsViral
             this.deathIncrease=deathIncrease
        }
    },
  methods: {
    
    async consumirAPI() {
      const datat = await fetch("https://api.covidtracking.com/v1/states/current.json").then(
        (r) => r.json()
      );
      return datat;
    },
    async consumirAPIestado(state) {
      const data = await fetch("https://api.covidtracking.com/v1/states/" + state+"/current.json ").then(
        (r) => r.json()
      );
      return data;
    },
    async construirTodosEstados() {
        const vectoestados = [];
        const dataAPI=await this.consumirAPI();
        for (let i = 0; i < 54; i++) {
          
            vectoestados.unshift(dataAPI[i].state);
        }
        this.estados=vectoestados;
        console.log(vectoestados)
      return vectoestados;
    },
    async mostarResultados(state){
        this.mostarcasos=true;
        const {positive,totalTestResults, hospitalizedCurrently,death , totalTestsViral,deathIncrease }=await this.consumirAPIestado(state);
        this.positive=positive;
        this.totalTestResults=totalTestResults;
        this.hospitalizedCurrently=hospitalizedCurrently;
        this.death=death;
        this.totalTestsViral=totalTestsViral;
        this.deathIncrease=deathIncrease;


    },


    },
    
    mounted(){
      console.log("se monto");
      this.construirTodosEstados();
    },/*
    beforeCreate(){
      console.log("se antes de crear");
    },
    beforeUpdate(){
      console.log("se beforeUpdate");
    },
    beforeUnmount(){
      console.log("se beforeUnmount");
    },
    renderTracked(){
      console.log("se renderTracked");
    },
    updated(){
      console.log("se monto");
    },
    unmonted(){
      console.log("se updated");
    },
    deactivated(){
      console.log("se deactivated");
    },
    created(){
      console.log("se created");
    }*/

}
</script>

<style>
.resultados{
    display: grid;
    margin-left: 35%;
    margin-right: 35%;
}
input{
    margin: 10px;
}

ul {

  
  list-style-type: none;
  width: 160px;
  margin: 0;
  padding: 0;
  background: rgb(172, 206, 250);
  columns: 3; 
  margin-left: 45%;
  
}


</style>