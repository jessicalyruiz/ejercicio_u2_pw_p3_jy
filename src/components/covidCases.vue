<template>
  <h1>Covid Cases USA</h1>
  <h3>Codigo de Estados</h3>

  <h4 v-for="item in estados">{{ item }}</h4>

<input  v-model="consultar" type="text" placeholder="Ingrese codigo estado">

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
      estados:null,
      estado:null,
      mostarcasos: true,
      positive: null,
      totalTestResults:null,
      hospitalizedCurrently:null,
      death:null,
      totalTestsViral:null,
      deathIncrease:null,
    };
  },
  watch:{
    consultar(value){
            console.log(value);
            if(!value.includes("?"))return;
              console.log('vamos conultar')  
        this.mostarResultados()
        }
    },
  methods: {
    consultar(){

    },
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
        for (let i = 0; i < 54; i++) {
            const { state } =await this.consumirAPI();
            vectoestados[i]=state;
        }
        this.estados=vectoestados;
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


    }
}
</script>

<style>
.resultados{
    display: grid;
}
input{
    margin: 10px;
}
</style>