<template>
  <div class="main-container">
    <input 
      type="text" 
      name="lookup" 
      id="lookup" 
      placeholder="Stock symbol"
      v-model="lookup"
    >
    <div class="info">
      <div v-if="loading" class="loading">
        Looking up <strong class="uppercase">{{ lookup }}</strong>
      </div>
      <div v-else>
        <stock-viewer :info="stockData"></stock-viewer>
      </div>
    </div>
    <footer>
      Data provided for free by IEX. View IEX’s <a href="https://iextrading.com/api-exhibit-a/">Terms of Use</a>.
    </footer>
  </div>
</template>

<script>
import axios from 'axios';
import _ from 'lodash';
import stockViewer from './StockViewer.vue'; 
const urlPrefix = 'https://api.iextrading.com/1.0';

export default {
  components: {
    stockViewer,
  },
  data() {
    return {
      lookup: null,
      loading: false,
      stockData: 'null',
    };
  },
  watch: {
    lookup(input) {
      if (input.length < 1) {
        return;
      }
      this.loading = true;
      this.getStockInfo(input);
    }
  },
  methods: {
    getStockInfo(input) {
      console.log(input);
      axios(`${urlPrefix}/stock/${input}/batch?types=quote,dividends`).then((res) => {
        this.loading = false;
        console.log(res);
        this.stockData = res;
      });
    }
  }
};
</script>

<style lang="scss" scoped>
p {
  font-size: 20px;
}
.loading {
  font-size: 16px;
  width: 100%;
  text-align: center;
}
.main-container {
  width: 400px;
  height: 400px;
  font-family: 'San Francisco', 'SF Pro Text', Roboto, Helvetica, sans-serif;
}
#lookup {
  font-size: 24px;
  border-radius: 2px;
  outline: none;
  margin: auto;
  width: 200px;
  border: 1px solid #ccc;
  padding: 10px;
  display: block;
}
.uppercase {
  text-transform: uppercase;
}
.info {
  height: 332px;
}
footer {
  font-size: 8px;
  text-align: center;
  width: 100%;
}
</style>
