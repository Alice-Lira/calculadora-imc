<script setup>
import { reactive, ref } from 'vue';


const dados = reactive({
  sexo: 'M',
  idade: null,
  altura: null,
  peso: null
})

const imc = ref(null)
const resultado = ref("")

const errorIdade = ref("")
const errorAltura = ref("")
const errorPeso = ref("")

function alteraSexo(sexo) {
  dados.sexo = sexo
}

function calcular() {
  errorIdade.value = ''
  errorAltura.value = ''
  errorPeso.value = ''

  if (dados.idade == null || dados.idade == '') {
    errorIdade.value = 'preenchimento obrigatório'
  }

  if (dados.altura == null || dados.altura == '') {
    errorAltura.value = 'preenchimento obrigatório'
  }
  if (dados.peso == null || dados.peso == '') {
    errorPeso.value = 'preenchimento obrigatório'
  }

  if (errorIdade.value != '' || errorAltura.value != '' || errorPeso.value != '') {
    return
  }

  let altura = dados.altura / 100
  imc.value = dados.peso / (altura * altura)

  if (imc.value >= 17 & imc.value < 18.5) {
    resultado.value = 'abaixo do peso'
  } else if (imc.value >= 18.5 & imc.value < 25) {
    resultado.value = 'peso ideal'
  } else if (imc.value >= 25 & imc.value < 30) {
    resultado.value = 'sobrepeso'
  } else if (imc.value >= 30 & imc.value < 35) {
    resultado.value = 'obesidade'
  }
}
</script>

<template>
  <div class="container">
    <div class="fundo">

      <div class="titulo">
        <h1>Calculadora de IMC</h1>
      </div>

      <p class="text-Sexo">Sexo</p>
      <div class="botao-mh">
        <button @click="alteraSexo('F')" :style="{
          backgroundColor: dados.sexo == 'F' ? '#ca71e8' : '#faf2f2',
          color: dados.sexo == 'F' ? 'white' : '#ca71e8',
        }" class="btn-mulher">
          Mulher
        </button>
        <button @click="alteraSexo('M')" :style="{
          backgroundColor: dados.sexo == 'M' ? '#0395e3' : '#edfaff',
          color: dados.sexo == 'M' ? 'white' : '#0395e3',

        }" class="btn-homem">
          Homem
        </button>
      </div>

      <form class="formulario">
        <div class="form-input">
          <label for="idade"> Idade</label>
          <input v-model="dados.idade" type="number" name="idade" placeholder="Ex: 35">
          <span>anos</span>
          <p v-if="errorIdade" class="mensagem-erro">{{ errorIdade }}</p>
        </div>

        <div class="form-input">
          <label for="altura"> Altura</label>
          <input v-model="dados.altura" type="number" name="altura" placeholder="Ex: 165">
          <span>cm</span>
          <p v-if="errorAltura" class="mensagem-erro">{{ errorAltura }}</p>
        </div>

        <div class="form-input">
          <label for="peso"> Peso</label>
          <input v-model="dados.peso" type="number" name="peso" placeholder="Ex: 62,5">
          <span>kg</span>
          <p v-if="errorPeso" class="mensagem-erro">{{ errorPeso }}</p>
        </div>

        <button @click="calcular()" class="botao-calcular" type="button">Calcular</button>

        <div class="resultado-imc" v-if="imc">
          <p>Seu IMC é de <strong>{{ imc.toFixed(2) }}</strong>. Seu IMC é considerado <strong>{{ resultado }}</strong>!
          </p>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped>
* {
  font-family: Arial, Helvetica, sans-serif
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  justify-content: center;
}

.fundo {
  border-radius: 5px;
  width: 40%;
  padding: 15px;
  border: #0395e3 solid 1px;
}

.formulario {
  display: flex;
  flex-direction: column;
  margin-bottom: 5px;
}


.titulo {
  text-align: center;
  font-size: 12px;
}

.text-Sexo {
  font-size: 15px;
  font-weight: bold;
  margin-bottom: 5px;
}

.btn-mulher {
  border-radius: 5px;
  padding: 10px;
  cursor: pointer;
  width: 49%;
  font-size: 14px;
  color: white;
  border: 1px solid #db9fef;

}

.btn-homem {
  border-radius: 5px;
  padding: 10px;
  cursor: pointer;
  width: 49%;
  font-size: 14px;
  border: 1px solid #83cdf5;

  color: white;
}

.botao-mh {
  display: flex;
  width: 100%;
  justify-content: space-between;
}

.form-input {
  margin-top: 10px;

}

.form-input input {
  width: 25%;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid rgb(197, 194, 194);
  outline: none;
  border-color: #ccc;
}

input[type=number]:focus {
  border: 2px solid #0395e3;
}

.form-input label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  font-size: 15px;
}

.form-input span {
  margin-left: 5px;
  font-size: 15px;
}

.botao-calcular {
  background-color: rgb(3, 149, 227);
  color: white;
  width: 100%;
  border-radius: 5px;
  padding: 10px;
  border: 0;
  font-size: 14px;
  cursor: pointer;
  margin-top: 20px;
}

.botao-calcular:hover {
  background-color: rgb(0, 86, 133);
}

.resultado-imc {
  margin-top: 15px;
  font-size: 14px;
  text-align: center;
}

.mensagem-erro {
  font-size: 14px;
  margin-top: 4px;
  color: #f83939;
}
</style>
