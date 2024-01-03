<template>
  <div class="app" style="margin-bottom: 10rem">
    <div>
      <label for="bolsa">Bolsa:</label><br />
      <input
        type="text"
        name="bolsa"
        id="bolsa"
        v-model="bolsa"
        v-on:keypress="searchbolsas(bolsa)"
      /><br />

      <div v-if="val == 1">
        <li v-for="(value, key) in bolsas" v-bind:key="key">
          <img
            class="image"
            :src="`https://api.socia.ao:44307${value.imagem}`"
            alt="API"
          />
          <br />
          {{ key }} : {{ value.modelo }} : {{ value.cor }}
          <div
            class="color-circle"
            :style="{ 'background-color': value.cor }"
          ></div>
        </li>
      </div>
      <div v-else-if="val == 2">Pesquisando....</div>
      <div v-else-if="val == 3">Nenhuma Dado Pesquisado</div>
    </div>
    <hr />

    <div>
      <img class="logo" :alt="imgAlt" src="./assets/logo.png" />
    </div>
    <hr />
    <div>
      <label for="name">Nome:</label><br />
      <input type="text" name="name" id="name" v-model="nome" /><br />
      {{ nome }}
    </div>
    <br />
    <div>
      <select name="sport" id="sport" v-model="sport">
        <option value="">Faça uma escolha</option>
        <option value="Futebol">Futebol</option>
        <option value="Basketbol">Basketbol</option>
        <option value="Skate">Skate</option>
      </select>
      <br />
      {{ sport }}
    </div>
    <br />
    <div>
      <label for="contract">Contrato</label><br />

      <input
        type="radio"
        name="contract"
        id="contract"
        v-model="contract"
        value="Sim"
      />Sim <br />
      <input
        type="radio"
        name="contract"
        id="contract"
        v-model="contract"
        value="Não"
      />Não

      <br />

      {{ contract }}
    </div>
    <div>
      <br />
      <label for="newsletter">Termos</label><br />

      <input
        v-model="newsletter"
        type="checkbox"
        name="newsletter"
        id="newsletter"
      />Aceita os termos <br />
      {{ newsletter }}
    </div>
    <br />
    <div>
      <label for="colors">Cores</label><br />

      <input
        v-model="colors"
        type="checkbox"
        name="colors"
        id="colors"
        value="Azul"
      />Azul <br />
      <input
        v-model="colors"
        type="checkbox"
        name="colors"
        id="colors"
        value="Verde"
      />Verde <br />
      <input
        v-model="colors"
        type="checkbox"
        name="colors"
        id="colors"
        value="Vermelho"
      />Vermelho <br />
      {{ colors }}
    </div>
    <br />
    <hr />
    <br />
    <div>
      <h1>Outro assunto</h1>
      <button @click="enviar">Enviar</button>
      <br />
      <div
        style="font-size: 12rem"
        @mouseover.once="enviar"
        @mouseout.once="enviar"
      >
        Ola
      </div>
      <br />
      <div>
        <form action="http://google.com" @submit.prevent="onSubmit">
          <input type="text" name="" id="" @keyup="onKeyUp" />
          <button type="submit">Enviar</button>
        </form>
      </div>
    </div>
    <hr />
    <br />
    <div>
      <h1>Nome Completo</h1>
      {{ user.firstName }} {{ user.lastName }}
      <FullName />
    </div>

    <div class="watch">
      <select name="" id="" v-model="dado">
        <option value="5">5</option>
        <option value="10">10</option>
        <option value="15">15</option>
      </select>
      <br />
      {{ dado }}
    </div>
    <hr />
    <div class="input">
      <input type="text" name="dbind" id="dbind" v-model="dbind" />
      <br />
      {{ dbind }}
    </div>
    <br />
    <hr />
    <div class="input">
      <input
        type="text"
        name="firstName"
        id="firstName"
        v-model="user.firstName"
      />
      <br />
      <br />
      <input
        type="text"
        name="lastName"
        id="lastName"
        v-model="user.lastName"
      />
      <br />
    </div>

    <br />
    <hr />
    <br />

    <TheHead>
      <template v-slot:title>
        <h1>Titulo</h1>
      </template>

      Conteudo normal
    </TheHead>

    <br />
    <hr />
    <br />

    <div class="filho">
      <FilhoComponent v-if="stage" styleprop="success">
        <template v-slot:paragraph>
          <h3>Isto é um alert com background</h3>

          <button @click="onClosed" class="btn-primary">X</button>
        </template>
      </FilhoComponent>
    </div>
  </div>
</template>

<script>
import FilhoComponent from "./components/FilhoComponent.vue";
import FullName from "./components/FullName.vue";
import TheHead from "./components/TheHead.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    FullName,
    TheHead,
    FilhoComponent,
  },
  data() {
    return {
      dado: "5",
      imgAlt: "Imagem",
      dbind: "",
      nome: "Eliude Francisco",
      bolsa: "",
      bolsas: {},
      sport: "Futebol",
      contract: "Sim",
      newsletter: true,
      val: 3,
      colors: [],
      user: {
        firstName: "",
        lastName: "",
      },
      stage: true,
    };
  },

  // Observador
  watch: {
    dado(newValue, oldValue) {
      console.log(newValue, oldValue);
    },
    dbind(vl) {
      console.log("Letra: " + vl);
    },
    user: {
      handler() {
        console.log("Alterou o Nome");
      },
      deep: true,
    },
  },

  // Propridade Computada
  computed: {
    fullname() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
  },

  //Metodos
  methods: {
    async searchbolsas(bolsadata) {
      try {
        this.val = 2;
        const response = await axios.get(
          "http://localhost:5283/api/dvj/Pesquisar",
          {
            params: {
              bolsa: bolsadata,
            },
          }
        );

        this.bolsas = response.data.resposta;
        this.val = 1;
      } catch (error) {
        this.val = 3;
        console.error("API Error: ", error);
      }
    },

    enviar($e) {
      console.log("Clicou no botão", $e);
    },
    onSubmit($e) {
      console.log("Enviou ao navegador", $e);
    },
    onKeyUp($e) {
      console.log("Input seleccionado", $e);
    },
    nomeCompleto() {
      console.log(this.fullname);
    },
    onClosed() {
      console.log("clicou");
    },
  },

  //Ciclo de Vida de um Componente
  // Criação
  beforeCreate() {}, // Antes de ser criado
  created() {}, // depois de ser criado

  // Montagem
  beforeMount() {}, // Antes de ser montado
  mounted() {}, // depois de ser montado

  // Atualização
  beforeUpdate() {}, // Antes de ser Atualizado
  updated() {}, // depois de ser Atualizado

  // Desmontagem
  beforeUnmount() {}, // Antes de ser desmontado
  unmounted() {}, // depois de ser desmontado
};
</script>

<style>
body {
  background-color: #1b2834;
  color: #fff;
}
.logo {
  transition: 0.3s;
  transform: rotate(0deg);
  cursor: pointer;
}
.logo:hover {
  transition: 0.3s;
  transform: rotate(90deg);
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}
.image {
  width: 80px;
}
.color-circle {
  width: 40px;
  height: 40px;
  border-radius: 50%; /* Torna o div um círculo */
  position: center;
  text-align: center;
}
</style>
