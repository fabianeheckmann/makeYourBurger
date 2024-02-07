<template>
  <div>
    <div>
      <MessageAlert :msg="msg" v-show="msg"/>
      <form id="burger-form" @submit="createBurger">
        <h1>Monte seu burger</h1>
        <div class="input-container">
          <label for="nome">client name: </label>
          <input
            type="text"
            id="name"
            v-model="nome"
            placeholder="type your name"
          />
        </div>
        <div class="input-container">
          <label for="bread">choose bread: </label>
          <select name="bread" id="bread" v-model="bread">
            <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">
             {{ bread.tipo }}
            </option>
          </select>
        </div>
        <div class="input-container">
          <label for="meat">choose meat: </label>
          <select name="meat" id="meat" v-model="meat">
            <option v-for="meat in meats" :key="meat.id" :value="meat.tipo">
              {{ meat.tipo }}
            </option>
          </select>
        </div>
        <div id="options-container" class="input-container">
          <label id="options-title" for="options">choose the options: </label>
          <div
            class="checkbox-container"
            v-for="option in optionsData"
            :key="option.id"
          >
            <input
              type="checkbox"
              name="options"
              v-model="options"
              :value="option.tipo"
            />
            <span>{{ option.tipo }}</span>
          </div>
        </div>
        <div class="input-container">
          <input type="submit" class="submit-btn" value="create burger" />
        </div>
      </form>
    </div>
  </div>
</template>

<script>

import MessageAlert from "./MessageAlert.vue";

export default {
  name: "BurgerForm",
  data() {
    return {
      breads: null,
      meats: null,
      optionsData: null,
      name: null,
      bread: null,
      meat: null,
      options: [],
      msg: null,
    };
  },
  methods: {
    async getIngredientes() {
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();

      this.optionsData = data.opcionais;
      this.breads = data.paes;
      this.meats = data.carnes;
    },

    async createBurger(e) {
      e.preventDefault();

      const data = {
        name: this.nome,
        bread: this.bread,
        meat: this.meat,
        options: Array.from(this.options),
        status: "solicitado",
      };

      const dataJson = JSON.stringify(data);

      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();
      this.msg = `Pedido número: ${res.id} realizado com sucesso!`

      setTimeout(() => this.msg = '', 3000);
    },
  },
  mounted() {
    this.getIngredientes();
  },
  components: {
    MessageAlert,
    MessageAlert
}
};
</script>

<style scoped>
#burger-form {
  max-width: 400px;
  margin: 0 auto;
}

#burger-form h1 {
  text-align: justify;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}

#options-container {
  flex-direction: row;
  flex-wrap: wrap;
}

#options-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
  width: auto;
}

.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
  transition: 0.5s;
  border-radius: 10px;
}

.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>
