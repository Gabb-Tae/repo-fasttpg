<template>
    <v-container class="pa-8" fluid>
    <h1 class="h1">Classes</h1>
        <v-list>
          <v-list-classe v-for="classe of classes" v-bind:key="classe.id"> 
            <v-list-classe-content><h3 id="classe-title">{{classe.title}}
              </h3>
               Força: {{classe.Força}}, Destreza: {{classe.Destreza}}, 
               Constituição: {{classe.Constituição}}, Inteligencia; {{classe.Inteligencia}}, 
               Sabedoria: {{classe.Sabedoria}}, Carisma: {{classe.Carisma}} <br>
            Vida por Nível: {{classe.vida}}, <br>
            Habilidade Principal: {{classe.habilidade}} <br>
            Descrição: {{classe.descricao}} <br>
            </v-list-classe-content> <br>
          </v-list-classe>
        </v-list>
    </v-container>
</template>

<script>
export default {
  data(){
    return {
      classes: null, 
      } 
  },
  methods: {
    async getFichas(){
      const req = await fetch("http://localhost:3000/Fichas");
      const data = await req.json();

      this.classes = data.classes;}
},
  mounted(){
    this.getFichas()
  }
}
</script>

<style scoped>
 #classe-title {
   font-weight: bold;
   color:#222;
   padding: 5px 10px;
   border-left: 4px solid #8b0292;
 }

</style>