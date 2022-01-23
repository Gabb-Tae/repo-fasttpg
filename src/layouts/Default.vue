<template>
  <v-app>
    <v-app-bar app color=#8b0292 dark elevation="1" >
      <v-app-bar-nav-icon @click.stop="sidebar = !sidebar"></v-app-bar-nav-icon>
      <v-img max-height="120" max-width="120" 
        src="../assets/images/logo-rpg.png"></v-img>
      <v-spacer></v-spacer>
      <v-icon> mdi-account</v-icon>
    </v-app-bar>
    <v-navigation-drawer app v-model="sidebar" :mini-variant.sync="mini">
      <v-list dense color="#8b0292" dark>
        <v-list-item>
          <v-list-item-action>
            <v-icon class="px=2" @click.stop="sidebar = !sidebar">mdi-chevron-left</v-icon>
          </v-list-item-action>
          <v-list-item-title>
          <h3>FastRPG</h3>
        </v-list-item-title>
        </v-list-item>
      </v-list>
      <v-list-item @click="mini = !mini">
        <v-list-item-avatar>
          <v-icon>mdi-account</v-icon>
        </v-list-item-avatar>
        <v-list-item-content>{{this.nome + this.sobrenome}}</v-list-item-content>
        <v-btn icon small><v-icon>mdi-chevron-left</v-icon></v-btn>
      </v-list-item>
      <v-divider></v-divider>
        <v-list>
          <v-list-item v-for="item of items" v-bind:key="item.title" link :to="item.to">
            <v-list-icon>
              <v-icon>{{item.icon}}</v-icon>
            </v-list-icon> 
            <v-list-item-content>{{item.title}}</v-list-item-content>
          </v-list-item>
        </v-list>
    </v-navigation-drawer>
    <v-main><router-view>
      </router-view></v-main>
    <v-footer app class="py-3">
      <span class="caption">FastRPG &copy;2021 <br>
      Equipe : Gabriel da Maia Leandro,

Gabrieli Traudete Ardini,

Beatriz Cristina de Arruda,

Maria Eduarda Nichelle Ferreira,

Guinnever Correa,

Lucas Danilo Born.</span></v-footer>

  </v-app>
</template>

<script>
import * as fb from '@/plugins/firebase'
export default {
  data() {
    return {
      nome: '',
      sobrenome: '',
      uid: '',
      temPerfil: false,
      sidebar: true,
      mini: false,
      items: [
        { title: "Home", icon:"mdi-script-text", to:"/"},
        { title:"Ficha", icon:"mdi-dice-d20",to:"/ficha"},
        { title:"Arquivo de Fichas", icon:"mdi-bookshelf",to:"/lista"},
        { title: "Classes", icon:"mdi-anvil",to:"/classes"},
        { title: "Raças", icon:"mdi-gnome",to:"/racas"},
        { title: "Dados", icon:"mdi-dice-multiple",to:"/dados"},
        { title: "Conversor", icon:"mdi-bitcoin",to:"/Conversor"},
        { title: "Perfil", icon:"mdi-account-cog", to:"/perfil"},
        { title: "Sair", icon:"mdi-exit-to-app", to:"/login"},
      
      ],
  async mounted(){
    this.uid = fb.auth.currentUser.uid;
    const userProfile = await fb.profileCollection.where("uid", "==", this.uid).get();
  if (userProfile.docs.length > 0) {
    this.temPerfil = true;
    const perfil = userProfile.docs[0];
    this.profileId = perfil.id;
    this.nome = perfil.data().nome;
    this.sobrenome = perfil.data().sobrenome;
  }
  },

    };
  }
}
</script>

<style>

</style>