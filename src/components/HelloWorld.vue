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
  <div v-if="tickers.length" class="cards">
    <div v-for="t in tickers" v-bind:key="t.name" class="card">
      <div class="name">{{ t.name }} - USD</div>
      <div class="price">{{ t.price }}</div>
      <button v-on:click="handleDelete(t)">Delete</button>
    </div>
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
      tickers: [
        { name: "ffff", price: "3423" },
        { name: "daddad", price: "64563453" },
        { name: "baaaaa", price: "34535324" },
      ],
    };
  },
  methods: {
    add() {
      const newTicker = {
        name: this.ticker,
        price: "-",
      };
      this.tickers.push(newTicker);
      this.ticker = "";
    },
    handleDelete(tickerToRemove) {
      this.tickers = this.tickers.filter((t) => t !== tickerToRemove);
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
  max-width: 200px;
  width: 100%;
}
button:hover {
  background-color: whitesmoke;
  color: rgba(57, 61, 90, 0.5);
  border: 1px solid rgba(57, 61, 90, 0.5);
}
.cards {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  border-bottom: 1px solid rgba(57, 61, 90, 0.5);
  border-top: 1px solid rgba(57, 61, 90, 0.5);
  width: fit-content;
  padding: 20px;
}
.card {
  display: flex;
  align-items: center;
  flex-direction: column;
  margin: 10px;
  padding: 10px;
  border-radius: 15px;
  box-shadow: 0px 2px 5px 0px rgb(57 61 90 / 25%),
    0px 5px 10px rgb(57 61 90 / 22%);
}
.name {
  font-size: 20px;
  color: rgba(57, 61, 90, 0.7);
  margin-bottom: 20px;
}
.price {
  margin-bottom: 10px;
  font-weight: 550;
  font-size: 18px;
  color: rgba(57, 61, 90, 0.9);
}
</style>
