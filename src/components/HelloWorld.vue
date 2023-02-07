<template>
  <div class="hello">
    <h3>Ticker</h3>
    <input
      v-model="ticker"
      v-on:keydown.enter="add"
      id="wallet"
      name="wallet"
      type="text"
      placeholder="For example DOGE"
    />
    <button v-on:click="add">Add</button>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      ticker: "",
      tickers: ["1", "2", "3"],
    };
  },
  methods: {
    add(evt) {
      alert(this.ticker);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
h3 {
  margin: 30px 0 10px;
}
input {
  font-size: 20px;
  border: none;
  border-bottom: 2px solid rgba(57, 61, 90, 0.5);
  outline: none;
}
input::placeholder {
  color: rgba(57, 61, 90, 0.5);
}
button {
  cursor: pointer;
  margin: 10px 0 10px;
  border-radius: 15px;
  border: none;
  background-color: rgba(57, 61, 90, 0.5);
  color: whitesmoke;
  font-size: 17px;
  padding: 10px;
  width: 20%;
}
button:hover {
  background-color: whitesmoke;
  color: rgba(57, 61, 90, 0.5);
  border: 1px solid rgba(57, 61, 90, 0.5);
}
</style>
