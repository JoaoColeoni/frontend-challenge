<template>
  <div class="table-cell d-flex">
    <img v-if="imagem == ''" src="../assets/img/images.png" class="photo" name="teste">
    <img v-else :src="imagem" class="photo" name="teste2">
    <div class="cell cell-container">
      <div class="title-cell">
        <p class="title-tag">"{{titulo}}"</p>  <p class="price-tag">R${{valor.toLocaleString('pt-br', {minimumFractionDigits: 2})}}</p>
      </div>
      <div class="cell-infos">
        <p>
          <b>Sabor:</b> {{sabor}}
        </p>
        <p>
          <b>Descrição:</b> {{descricao}}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TableInfo',
  props: {
    titulo: String,
    sabor: String,
    valor: Number,
    descricao: String,
    imagem: String
  }
}

</script>

<style scoped>
.photo{
  width: 150px;
  height: 150px;
  border-radius: 15px;
  float: left;
  z-index: 10;
  background-color: white;
}

.title-cell{
  width: calc(100% - 110px);
  background-color: #e43636;
  border-radius: 15px 15px 0px 0px;
  text-align: left;
  padding-left: 80px;
  padding-right: 30px;
  display: flex;
  font-weight: bold;
  font-style: italic;
  color: #ffca00;
  font-size: 26px;
}

.price-tag{
  color: white;
  float: right;
  right: 0px;
  margin-left: auto;
  align-self: center;
  font-size: 22px;
  margin-bottom: 20px;
  margin-top: 20px;
}

.title-tag{
  margin-bottom: 20px;
  margin-top: 20px;
}

.cell-infos{
  width: calc(100% - 110px);
  padding-right: 30px;
  padding-left: 80px;
  text-align: left;
  color: #a03400;
  font-style: italic;
  font-size: 20px;
  margin-top: 15px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: "...";
}

.table-cell{
  width: 100%;
  float: right;
  margin-bottom: 50px;
}

.cell-container{
  width: calc(100% - 100px);
  height: 190px;
  margin-left: -50px;
  margin-top: -20px;
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
</style>
