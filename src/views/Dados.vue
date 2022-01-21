<template>
  <div class="row">
    <div class="container">
        <h1>Dados online</h1>
                <div class="row">
            <div class="col-md-2">
                <div class="jumbotron m-0" id="jd20">
                    <div class="text-center">
                        <v-btn color=#8b0292 class="white--text" @click="rodar_d20()">Rolar</v-btn>
                    </div>
                    <div class="text-center">
                            <img src="../assets/dices/d20-icon.png" width="100" 
                            style="cursor: pointer;" id="id20" data-type="20" 
                            @click="rodar_d20()">
                    </div>
                    <div style="text-align: center; font-size: 18px; font-style: italic;" id="result"><h3>Resultado {{d20}}</h3></div>
                </div>
                <div class="card">
                <h1>D20</h1>
                <div class="caixa-resultado" id="box-res-20"></div>
                <button onclick="girarDadoD20()">Rolar!</button>
            </div>
            </div>

            <div class="col-md-2">
                <div class="jumbotron m-0" id="jd12">
                    <div class="text-center">
                        <v-btn color=#8b0292 class="white--text" @click="rodar_d12">Rolar</v-btn>
                    </div>
                    <div class="text-center">
                            <img src="../assets/dices/d12-icon.png" width="100" style="cursor: pointer;" id="id12" data-type="12" @click="rodar_d12">
                    </div>
                    <div style="text-align: center; font-size: 18px; font-style: italic;"><h3>{{d12}}</h3></div>
                    <input id="d20" type="button" value="D20" onclick="clicar12()">
                </div>
            </div>

        </div>
        <br>
    </div>
</div>
</template>

<script>
export default {
  async rodar_d20(){
  var d20 = (Math.floor(Math.random() *20)+1)
  return d20
}

  }
</script>

<style>

</style>