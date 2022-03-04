<template>
  <div id="container">
    <!--Logo-->
    <div>
      <img src="../src/assets/img/Logo.svg" id="logo">
      <img src="../src/assets/img/pastel-paralax.png" id="pastel-paralax">
      <img src="../src/assets/img/pasteis-img.png" id="pasteis">
    </div>
    <!--Formulario-->
    <div class="row cell">
        <div id="form-title" class="d-flex">
          <div class="w7 small-w5">
            <p class="title-text">Monte aqui o seu cardápio. O que está esperando?</p>
          </div>
          <div class="w3 small-w5 d-flex switch-div">
              <p class="type-text">Comida</p>
              <label class="switch">
                <input type="checkbox" name="tipo" v-model="bebida" >
                <span class="slider round"></span>
              </label>
              <p class="type-text">Bebida</p>
          </div>
        </div>
        <div id="form-inputs" class="d-flex">
          <div class="w4 small-w5">
            <input class="basic-input" v-model="formulario.titulo_pedido" minlength="3" maxlength="60" type="text" name="titulo" placeholder="Título do pedido" required>
          </div>
          <div class="w4 small-w5">
            <input class="basic-input" v-model="formulario.sabor" minlength="3" maxlength="60" type="text" name="sabor" placeholder="Sabor" required>
          </div>
          <div class="w2 small-w5">
            <input class="basic-input" v-model="formulario.valor" type="number" name="valor" placeholder="R$" required>
          </div>
          <div class="w10">
            <textarea v-model="formulario.descricao" class="basic-input descricao-input" type="text" name="descrição" placeholder="Descrição"></textarea>
          </div>
          <div class="w10">
            <label class="basic-input image-input" for="imagem"><img v-if="this.formulario.image == ''" class="preview-img" src="../src/assets/img/preview_img.png"><img v-else class="preview-img" :src="this.formulario.image"><br>Jogue aqui o arquivo de imagem do seu pastel ou clique para localizar a pasta.</label>
            <input class="basic-input" v-on:change="formulario.image" style="display: none" type="file" id="imagem" name="imagem" accept="image/png, image/jpeg" @change="previewFiles">
          </div>
        </div>
    </div>
    <!--Botoes-->
    <div id="form-buttons">
      <button class="simple-btn" type="button" v-on:click="clearForm()">LIMPAR</button>
      <button class="simple-btn simple-btn--altcolor" type="button" v-on:click="saveForm()">CADASTRAR</button>
      <button class="simple-btn simple-btn--black" type="button" v-on:click="loadMock()">LOAD MOCK</button>
      <h1 v-if="error != ''" class="error-text">{{error}}</h1>
    </div>
    <!--Tabela-->
    <div>
      <hr class="red-line red-line-left">
      <p class="title-text">Veja como será apresentado ao cliente</p>
      <hr class="red-line red-line-right">
      <br>
      <div class="w10 small-w5" v-if="this.registros.length > 0">
        <input class="basic-input" v-model="filtro" type="text" name="titulo" placeholder="Filtre seu pedido aqui!">
      </div>
      <br>
      <br>
      <div v-for="data in tableData" :key="data.titulo_pedido" style="margin-top: 50px">
        <TableInfo :titulo="data.titulo_pedido" :sabor="data.sabor" :valor="data.valor" :descricao="data.descricao" :imagem="data.image">
        </TableInfo>
      </div>
    </div>
  </div>
  <!-- <img alt="Vue logo" src="./assets/img/logo.png"> -->
  <!-- <HelloWorld msg="Teste"/> -->
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import TableInfo from './components/TableInfo.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    TableInfo
  },
  data(){
    return{
      bebida: false,
      error: '',
      filtro: '',
      formulario: {
        titulo_pedido: '',
        sabor: '',
        valor: null,
        descricao: '',
        image: ''
      },
      registros: []
    }
  },
  methods: {
    clearForm(){
      this.formulario.titulo_pedido = '';
      this.formulario.sabor = '';
      this.formulario.valor = null;
      this.formulario.descricao = '';
      this.formulario.image = '';
      this.error = '';
    },
    previewFiles(event){
      if(event.target.files.length > 0){
        var path = (window.URL || window.webkitURL).createObjectURL(event.target.files[0]);
        this.formulario.image = path;
      }
    },
    saveForm(){
      if(this.formulario.titulo_pedido.length < 3){
        this.error = "Título deve ter pelo menos 3 letras.";
        return;
      }
      if(this.formulario.sabor.length < 3){
        this.error = "Sabor deve ter pelo menos 3 letras.";
        return;
      }
      if(this.formulario.valor < 0 || this.formulario.valor == null){
        console.log("teste");
        this.error = "Valor deve ser maior que zero.";
        return;
      }

      this.registros.push(
        {
          "titulo_pedido": this.formulario.titulo_pedido,
          "sabor": this.formulario.sabor,
          "valor": this.formulario.valor,
          "descricao": this.formulario.descricao,
          "image": this.formulario.image,
          "bebida": this.bebida
        }
      );
      this.clearForm();
    },
    loadMock(){
      axios.get('https://62223129666291106a1fdfd3.mockapi.io/api/v1/Product').then(response => {
        for (var i = 0; i < response.data.length; i++) {
          var bebida = false;

          if(i < 5)
            bebida = true;

          this.registros.push(
            {
              "titulo_pedido": response.data[i].titulo,
              "sabor": response.data[i].sabor,
              "valor": response.data[i].valor,
              "descricao": response.data[i].descricao,
              "image": response.data[i].image,
              "bebida": bebida
            }
          );
        }
      });
      // this.registros.push(
      //   {
      //     "titulo_pedido": this.formulario.titulo_pedido,
      //     "sabor": this.formulario.sabor,
      //     "valor": this.formulario.valor,
      //     "descricao": this.formulario.descricao,
      //     "image": this.formulario.image,
      //     "bebida": this.bebida
      //   }
      // );
    }
  },
  computed: {
    tableData: function () {
      const AuxRegistros = [];

      for (var i = this.registros.length; i > 0; i--)
      {
        if(this.registros[i - 1].bebida == this.bebida
           && (this.registros[i - 1].titulo_pedido.toUpperCase().match(this.filtro.toUpperCase()) || this.registros[i - 1].sabor.toUpperCase().match(this.filtro.toUpperCase()) || this.registros[i - 1].descricao.toUpperCase().match(this.filtro.toUpperCase()))
        )
          AuxRegistros.push(this.registros[i - 1]);
      }

      return AuxRegistros;
    }
  },
}

</script>

<style>
@import './css/main.css';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 50px;
}
</style>
