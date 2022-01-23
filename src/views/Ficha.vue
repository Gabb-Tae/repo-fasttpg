<template>
  <div>
    <div>
      <Message :msg="msg" v-show="msg" />
      <form id="fichaform" @submit="createficha">
        <h3>Criação Rápida de Ficha</h3>
        <div class="input-container-nome" id="gerador_nomes">
        <input type="text" id="nome" name="nome" v-model="nome" placeholder="Nome do Personagem">
        <v-icon class="btn" slot="append" @click="gerar()">mdi-dice-multiple</v-icon>
        <v-icon slot="append" @click="adicionar">mdi-send</v-icon>
        </div>
        <div class="input-container">
          <label id="classe" for="classe">Classe: </label>
          <select name="classe" id="classe" v-model="classe">
              <option value="">Selecione sua Classe</option>
              <option v-for="classe in classes" 
              :key="classe.id" :value="classe">{{ classe.title }}
              </option>
          </select>
        </div>
        <div class="input-container">
          <label id="raca" for="raca">Raça: </label>
          <select name="raca" id="raca" v-model="raca">
              <option value="">Selecione sua Raça</option>
              <option v-for="raca in racas" 
              :key="raca.id" :value="raca">{{ raca.title }}
              </option>
          </select>
        </div>
        <div class="input-container">
          <label id="nivel" for="nivel">Nivel: </label>
          <select name="nivel" id="nivel" v-model="nivel">
              <option value="">Selecione seu Nivel</option>
              <option v-for="nivel in niveis" 
              :key="nivel.id" :value="nivel">{{ nivel.title }}
              </option>
          </select>
        </div>
        <div class="input-container">
          <input type="submit" class="submit-btn" value="Criar minha Ficha">
        </div>
      </form>
      <v-list>
      <v-list-item-group>
      <v-list-item v-for="ficha of fichas" :key="ficha.id">
        {{ficha.nome}}
      </v-list-item>
      </v-list-item-group>
    </v-list>
    </div>
  </div>
</template>

<script>
import Message from './Message.vue';
import gerarnome from '@/utils/gerarnome.js'
import gerarsobrenome from '@/utils/gerarsobrenome.js'
import * as fb from "@/plugins/firebase"
export default {
  name: "fichaform",
  data(){

    return {
      classes: null,
      racas: null,
      niveis: null,
      nome: '',
      classe: null,
      raca: null,
      nivel:  null,
      msg: null,
      uid: '',
      fichas: [],
    }
  },
  methods: {
    async buscarFichasDoServidor() {
      this.fichas = [];
      const logFichas = await fb.fichaCollection.where("owner", "==", this.uid).get();
      for (const doc of logFichas.docs){
        this.Fichas.push({
          id: doc.id,
         nome: doc.data().nome
        });
      }
    },
    async adicionar() {
      await fb.fichaCollection.add({
        nome: this.nome,
        dataGravacao: new Date().toISOString().slice(0, 10),
        owner: this.uid,
      });
      this.nome = "";
      this.buscarFichasDoServidor();
    },
    gerar(){
      this.nome = gerarnome()+gerarsobrenome()
    },
    async getFichas(){
      const req = await fetch("http://localhost:3000/Fichas");
      const data = await req.json();

      this.classes = data.classes;
      this.racas = data.racas;
      this.niveis = data.niveis;
    }, 
    async createficha(e){
      e.preventDefault();
      const forca = this.classe.Força + this.raca.Força
      const destreza = this.classe.Destreza + this.raca.Destreza
      const constituicao = this.classe.Constituição + this.raca.Constituição
      const inteligencia = this.classe.Inteligencia + this.raca.Inteligencia
      const vidamax = this.classe.vida * this.nivel.vida
      const sabedoria = this.classe.Sabedoria + this.raca.Sabedoria
      const carisma = this.classe.Carisma + this.raca.Carisma
      const tamanho = this.raca.tamanho
      const deslocamento = this.raca.Deslocamento
      const idioma = this.raca.idioma
      const caracteristicas = this.raca.Caracteristicas
      const uid = fb.auth.currentUser.uid;
      const habilidade = this.classe.habilidade
      const descricao = this.classe.descricao
      const data =  {
        nome: this.nome,
        classe: this.classe.title,
        raca: this.raca.title,
        nivel: this.nivel.title,
        vidamax,
        forca,
        destreza,
        constituicao,
        inteligencia,
        sabedoria,
        carisma,
        uid,
        tamanho,
        deslocamento,
        idioma,
        caracteristicas,
        habilidade,
        descricao


      }
      const dataJson = JSON.stringify(data);
      const req = await fetch("http://localhost:3000/arquivos",{
        method: "POST",
        headers:{ "Content-Type":"application/json"},
        body: dataJson
      });
      const res = await req.json();
      this.msg = `Ficha de ${res.nome} Criada com Sucesso, Confira o Arquivo`;

      setTimeout(() => this.msg = "", 3000);

      console.log(res);
      this.nome = "";
      this.classe = "";
      this.raca = "";
      this.nivel = "";
    }
  },
  mounted(){
    this.getFichas(),
    this.uid = fb.auth.currentUser.uid;
    this.buscarFichasDoServidor();
  },
  components: {
    Message
  }
}
</script>

<style scoped>
 #fichaform{
   max-width: 400px;
   margin: 0 auto;
 }

 .input-container{
   display: flex;
   flex-direction: column;
 }
 .input-container-nome {
  display: flex;
  flex-direction: row;
  width: 600px;
  margin-bottom: 15px;
  padding: 5px 10px;
  border-left: 4px solid #8b0292;

}
  #classe,#raca,#nivel {
   margin-bottom: 15px;
   color:#222;
   padding: 5px 10px;
   border-left: 4px solid #8b0292;
 }

 .label {
   font-weight: bold;
   margin-bottom: 15px;
   color:#222;
   padding: 5px 10px;
   border-left: 4px solid #8b0292;
 }

 .input,select {
   padding: 5px 10px;
   width: 300px;
 }

 .submit-btn{
   background-color: #a23ea7;
   color: #222;
   font-weight: bold;
   border: 2px solid #222;
   padding: 10px;
   font-size: 16px;
   margin: 0 auto;
   cursor: pointer;
   transition: .5s;
 }

 .submit-btn:hover{
   background-color: transparent;
   color: #222;
 }
</style>