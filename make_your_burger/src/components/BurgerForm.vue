<template>
  <div>
    <p>message</p>
    <div>
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
            <option value="">select your bread</option>
            <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">
              {{ bread.tipo }}
            </option>
          </select>
        </div>
        <div class="input-container">
          <label for="protein">choose protein: </label>
          <select name="protein" id="protein" v-model="protein">
            <option value="">select your protein</option>
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

      this.breads = data.paes;
      this.meats = data.carnes;
      this.optionsData = data.opcionais;

      console.log(this.optionsData);
    },

    async createBurger(e) {
      e.preventDefault();

      const data = {
        name: this.nome,
        bread: this.pao,
        meat: this.carne,
        options: Array.from(this.options),
        status: "solicitado",
      };
      console.log(data);

      const dataJson = JSON.stringify(data);

      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();
      console.log(res);
    },
  },
  mounted() {
    this.getIngredientes();
  },
};
</script>

<style scoped>
#burger-form {
  max-width: 400px;
  margin: 0 auto;
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
