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
    <div
      v-for="t in tickers"
      v-bind:key="t.name"
      @click="selectTicker(t)"
      :class="{ 'border-4': select === t }"
      class="card"
    >
      <div class="name">{{ t.name }} - USD</div>
      <div class="price">{{ t.price }}</div>
      <button v-on:click.stop="handleDelete(t)">Delete</button>
    </div>
  </div>
  <div v-if="select" class="dopInfo">
    <h3>{{ select.name }} - USD</h3>
    <div class="items">
      <div
        v-for="(bar, idx) in normalizeGraph()"
        :key="idx"
        :style="{ height: `${bar}%` }"
        class="item"
      ></div>
    </div>
    <svg
      @click="select = null"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      xmlns:svgjs="http://svgjs.com/svgjs"
      version="1.1"
      width="30"
      height="30"
      x="0"
      y="0"
      viewBox="0 0 511.76 511.76"
      style="enable-background: new 0 0 512 512"
      xml:space="preserve"
    >
      <g>
        <path
          d="M436.896,74.869c-99.84-99.819-262.208-99.819-362.048,0c-99.797,99.819-99.797,262.229,0,362.048    c49.92,49.899,115.477,74.837,181.035,74.837s131.093-24.939,181.013-74.837C536.715,337.099,536.715,174.688,436.896,74.869z     M361.461,331.317c8.341,8.341,8.341,21.824,0,30.165c-4.16,4.16-9.621,6.251-15.083,6.251c-5.461,0-10.923-2.091-15.083-6.251    l-75.413-75.435l-75.392,75.413c-4.181,4.16-9.643,6.251-15.083,6.251c-5.461,0-10.923-2.091-15.083-6.251    c-8.341-8.341-8.341-21.845,0-30.165l75.392-75.413l-75.413-75.413c-8.341-8.341-8.341-21.845,0-30.165    c8.32-8.341,21.824-8.341,30.165,0l75.413,75.413l75.413-75.413c8.341-8.341,21.824-8.341,30.165,0    c8.341,8.32,8.341,21.824,0,30.165l-75.413,75.413L361.461,331.317z"
          fill="#393d5a80"
          data-original="#000000"
        ></path>
      </g>
    </svg>
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
      tickers: [],
      select: null,
      graph: [],
    };
  },
  methods: {
    add() {
      const newTicker = {
        name: this.ticker,
        price: "-",
      };
      this.tickers.push(newTicker);
      setInterval(async () => {
        const f = await fetch(
          `https://min-api.cryptocompare.com/data/price?fsym=${newTicker.name}&tsyms=USD`
        );
        const data = await f.json();
        this.tickers.find((t) => t?.name === newTicker?.name).price = data.USD;
        if (this.select?.name === newTicker.name) {
          this.graph.push(data.USD);
        }
      }, 5000);
      this.ticker = "";
    },
    selectTicker(ticker) {
      this.select = ticker;
      this.graph = [];
    },
    handleDelete(tickerToRemove) {
      if (this.select === tickerToRemove) {
        this.select = null;
      }
      this.tickers = this.tickers.filter((t) => t !== tickerToRemove);
    },
    normalizeGraph() {
      const maxValue = Math.max(...this.graph);
      const minValue = Math.min(...this.graph);
      return this.graph.map(
        (price) => 5 + ((price - minValue) * 95) / (maxValue - minValue)
      );
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
  width: auto;
  padding: 20px;
  justify-content: center;
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
svg {
  position: absolute;
  top: 0;
  right: 0;
}
.dopInfo {
  position: relative;
}
.border-4 {
  border: 2px solid rgba(57, 61, 90, 0.5);
}
.item {
  background-color: rgba(57, 61, 90);
  width: 2.5rem;
  border: 1px solid whitesmoke;
}
.items {
  display: flex;
  align-items: flex-end;
  border-bottom: 1px solid #718096;
  border-left: 1px solid #718096;
  height: 200px;
}
</style>
