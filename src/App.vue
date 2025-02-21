<template>
  <div id="app">  
    <header class="header">
      <section>
        <a href="#" class="logo">
            <img src="./assets/logo.png" alt="logo">
        </a>
        <label class="image-label">Ship<span>Hub</span></label>
          <!-- barra de navegação -->
        <nav class="navbar">
          <a href="#">Home</a>
          <a href="#about">About</a>
        </nav>
      </section>
    </header>
  
  <!-- lógica da imagem + botão no início-->
  <div v-if="formulario === false" class="container">
    <div class="create">
      <img src="./assets/img-main.svg">
      <button v-if="formulario === false" @click="formulario = true" class="btn-create">NEW QUOTATION</button>
    </div>
  </div>

  <div v-if="formulario === true" class="formulario">
    <div class="container-forms">
      <form @submit.prevent="enviarFormulario">
        <div class="input-group">
          <input type="text" id="origin" v-model="origin" placeholder="Origin" required />
            <img width="20" height="20" src="https://img.icons8.com/ios/50/ffffff/long-arrow-right--v1.png" alt="long-arrow-right--v1"/>
          <input type="text" id="destiny" v-model="destiny" placeholder="Destiny" required />
            <img width="20" height="20" src="https://img.icons8.com/ios-filled/30/ffffff/marker.png" alt="marker"/>
          <input type="text" id="route" v-model="route" placeholder="Route" required /><br>
        </div>

        <div class="input-group2">
          <input type="number" id="transittime" v-model="transittime" placeholder="Transit time" required />
          <input v-mask="'##/##/####'" id="expirationdate" v-model="expirationdate" placeholder="dd/mm/aaaa" required /><br>
          <label><input type="radio" v-model="cotacao" value="aerea"> Aérea</label>
          <label><input type="radio" v-model="cotacao" value="maritima"> Marítima</label>
        </div>

          <!-- se a cotação for aérea -->
        <div v-if="cotacao === 'aerea'">
          <label for="peso">Peso Taxado:</label>
          <input type="number" id="peso" v-model="pesoTaxado" placeholder="Informe o peso (Kg)">
          <input type="checkbox" id="billlading" v-model="billlading">
          <label for="billlading">Bill lading</label>
        </div>

          <!-- se a cotação for marítima -->
        <div v-if="cotacao === 'maritima'">
          <div class="radio">
            <label for="m3">M³:</label>
            <input type="number" id="m3" v-model="m3" placeholder="Informe o volume em metros cúbicos">
            <input type="checkbox" id="billlading" v-model="billlading">
            <label for="billlading">Bill lading</label>
          </div>
        </div>
      
        <br><button type="submit">Enviar</button>
      </form>
    </div>
  </div>
</div>
</template>


<script>
import VueTheMask from 'vue-the-mask';
import axios from 'axios';  // Importando o axios

export default {
  directives: {
    'mask': VueTheMask.VueTheMask
  },

  data() {
    return {
      cotacao: '',  // Valor inicial vazio ou 'aerea' se preferir que inicie com a opção aérea
      pesoTaxado: null,
      m3: null,
      formulario: false,
    };
  },
  methods: {
    async enviarFormulario() {
      try {
        // Dados que serão enviados para o backend
        const dados = {
          nome: this.nome,
          email: this.email
        };

        // Fazendo a requisição POST para a API do backend (Flask)
        const resposta = await axios.post('http://localhost:5000/api/formulario', dados);

        // Verificando a resposta do servidor
        if (resposta.status === 200) {
          this.formEnviado = true;
          this.erroEnvio = false;
          this.limparFormulario();
        }
      } catch (erro) {
        console.error('Erro ao enviar formulário:', erro);
        this.erroEnvio = true;
        this.formEnviado = false;
      }
    },

    limparFormulario() {
      this.nome = '';
      this.email = '';
    }
  }
};
</script>

<style scoped>
@import './assets/style.css';
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100..900;1,100..900&display=swap');

/* barra de navegação */
.header {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 99;
}

.header section {
    display: flex;
    align-items: center;
    padding-top: 0.5rem;
    padding-bottom: 1.5rem;
    padding-left: 1.5rem;
    gap: 2px;
}

.logo img{
  width: 3rem;
}

.image-label {
  color: var(--terciary-color);
  font-size: 1.6rem;
  font-weight: 700;
  margin-left: 0.1rem;
  margin-right: 2rem;
}

.image-label span {
  color: var(--secondary-color);
}

.navbar a {
    margin: 1.5rem;
    font-size: 1rem;
    color: #fff;
}

.navbar a:hover {
    color: var(--secondary-color);
    border-bottom: 0.1rem solid;
    padding-bottom: 0.2rem;
    font-size: 1.1rem;
}

/* container da imagem e do botão */
.container {
  display: flex;
  justify-content: center; 
  align-items: center; 
  height: 100vh;
}

.create {
  width: 25%;
}

.btn-create {
  margin-left: 7rem;
  background-color: var(--secondary-color);
  cursor: pointer;
  border: none;
  padding: 8px 8px;
  border-radius: 0.3rem;
  color: var(--terciary-color);
  font-weight: 700;
  text-align: center;
}

.btn-create:hover {
  background-color: var(--terciary-color);
  color: var(--secondary-color);
}

.container-forms { /* container para centralizar os inputs*/ 
  display: flex; 
  position: absolute;
  top: 120px;  
  left: 50%;
  transform: translateX(-50%); 
  max-width: 30rem;
  background-color: red;
}

.formulario { /* formulario em si */
  width: 100%;
  max-width: 800px;  
  display: flex;
  flex-direction: column;
  align-items: center;
  color: var(--terciary-color);
}

.input-group {
  display: flex; 
  justify-content: space-between; 
  width: 95%;
  max-width: 600px;
  margin-top: 1rem;
  margin-left: 0.7rem;  
  margin-bottom: 1.2rem;
}

.input-group input {
  width: 30%;
  padding: 6px;
  margin: 0 10px;
  border: 1px solid #ccc;
  border-radius: 7px;
  font-size: 12px;
}

.input-group img {
  margin-top: 0.3rem;
}

.input-group2 {
  display: flex;  
  width: 82%;
  margin-left: 0.7rem;
}

.input-group2 input {
  width: 48%;
  padding: 6px;
  margin: 0 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 12px;
}

.formulario input[type="radio"] {
  width: auto;
  margin-right: 0.5rem;
  margin-left: 0;
  vertical-align: middle;
}

.formulario label {
  font-size: 12px;/* Garante que o texto da label fique alinhado com o input */
  display: inline-flex;
  align-items: center;
  margin-left: 0.3rem;
}

.radio {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

</style>