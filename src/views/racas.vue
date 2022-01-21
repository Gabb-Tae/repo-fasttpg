<template>
    <v-container class="pa-8" fluid>
    <h1 class="h1">Raças</h1>
        <v-list>
          <v-list-raca v-for="raca of racas" v-bind:key="raca.id"> 
            <v-list-raca-content><h3 id="raca-title">{{raca.title}}
              </h3>Força: {{raca.Força}}, Destreza: {{raca.Destreza}}, 
               Constituição: {{raca.Constituição}}, Inteligencia; {{raca.Inteligencia}}, 
               Sabedoria: {{raca.Sabedoria}}, Carisma: {{raca.Carisma}} <br>
            Tamanho: {{raca.vida}}, <br>
            Deslocamento: {{raca.Deslocamento}} <br>
            Idioma: {{raca.idioma}} <br>
            Caracteristicas: {{raca.Caracteristicas}} 
            <br>
            </v-list-raca-content> <br>
          </v-list-raca>
        </v-list>
    </v-container>
</template>

<script>
export default {
  data(){
    return {
      racas: null, 
      } 
  },
  methods: {
    async getFichas(){
      const req = await fetch("http://localhost:3000/Fichas");
      const data = await req.json();

      this.racas = data.racas;}
},
  mounted(){
    this.getFichas()
  }
}
</script>

<style scoped>
 #raca-title {
   font-weight: bold;
   color:#222;
   padding: 5px 10px;
   border-left: 4px solid #8b0292;
 }


</style>