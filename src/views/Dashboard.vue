<template>
  <div id="ficha-arquivo-rows">
      <div class="ficha-arquivo-row" v-for="item in arquivos" :key="item.id">
        <div id="ficha-title">
          Ficha de {{ item.nome }} 
        </div>
        <div class="order-number">
            #ID: {{ item.id }}
        </div>
              <div id="text">Nome: {{ item.nome }}</div>
              <div>Raça: {{ item.raca }}</div>
              <div>Classe: {{ item.classe }}</div>
              <div>Nível: {{ item.nivel }}</div>
              <div>Vida Máxima: {{item.vidamax}}</div>
              <div>Habilidade Principal: {{item.habilidade}}</div>
              <div>Força: {{ item.forca }}</div>
              <div>Destreza: {{ item.destreza }}</div>
              <div>Constituição: {{ item.constituicao }}</div>
              <div>Inteligencia: {{ item.inteligencia }}</div>
              <div>Sabedoria: {{ item.sabedoria }}</div>
              <div>Carisma: {{ item.carisma }}</div>
              <div>Tamanho: {{item.tamanho}}</div>
              <div>Velocidade: {{item.deslocamento}}</div>
              <div>Idioma: <p> {{item.idioma}} </p></div>
              <div>Caracteristicas: <p> {{item.caracteristicas}} </p></div>
              <div>Descrição: <p> {{item.descricao}} </p> </div>
              <div>
                <button><v-icon class="like-btn" @click="like">mdi-thumb-up-outline</v-icon></button><br> <br>
                <button v-if="item.uid === uid" class="delete-btn" @click="deleteitem(item.id)"><v-icon>mdi-delete</v-icon></button>
              </div>
              
      </div>
  </div>
</template>

<script>
import * as fb from "@/plugins/firebase"
export default {
  name: "Dashboard",
    data() {
      return {
        arquivos: null,
        arquivo_id: null,
        uid: '',
      }
    },
    methods: {
      async getcriacao() {
        const req = await fetch("http://localhost:3000/arquivos");

        const data = await req.json();

        this.arquivos = data;
      },

},
    mounted() {
      this.uid = fb.auth.currentUser.uid;
      this.getcriacao();
    }

}
</script>

<style scoped>

#ficha-title {
   font-weight: bold;
   color:#222;
   padding: 5px 10px;
   border-left: 4px solid #8b0292;
 }
#ficha-arquivo-rows,
.ficha-arquivo-row{
  display: flex;
  flex-wrap: wrap;
}

.ficha-arquivo-row div{
  width: 14%;
}

.ficha-arquivo-row{
  width: 100%;
  padding: 15px;
  border-bottom: 1px solid #CCC;
}

#ficha-arquivo-heading .order-id,
.ficha-arquivo-row .order-number {
  width: 10%;
}
.like-btn {
   background-color: #bf64c4;
   color: #222;
  font-weight: bold;
   border: 2px solid #8b0292;
   padding: 9.5px;
   font-size: 25px;
   margin: 0 auto;
   cursor: pointer;
   transition: .5s;
}
.like-btn:hover{
   background-color: transparent;
   color: #222;
 }

.delete-btn {
   background-color: #ca5353;
   color: #222;
   font-weight: bold;
   border: 2px solid #a00303;
   padding: 10px;
   font-size: 16px;
   margin: 0 auto;
   cursor: pointer;
   transition: .5s;
}

.delete-btn:hover{
   background-color: transparent;
   color: #222;
 }


</style>