<script>
import axios from 'axios';

export default {
  data() {
    return {
      usdInput: 0,
      btcAmount: 0,
      ethAmount: 0,
      exchangeRates: {
        BTC: 0,
        ETH: 0,
      },
    };
  },
  methods: {
    async fetchExchangeRates() {
      try {
        const response = await axios.get('https://api.coinbase.com/v2/exchange-rates?currency=USD');
        this.exchangeRates.BTC = response.data.data.rates.BTC;
        this.exchangeRates.ETH = response.data.data.rates.ETH;
      } catch (error) {
        console.error('Error fetching exchange rates:', error);
      }
    },
    convertUsd() {
      const btcAllocation = this.usdInput * 0.7;
      const ethAllocation = this.usdInput * 0.3;
      
      this.btcAmount = (btcAllocation * this.exchangeRates.BTC).toFixed(6);
      this.ethAmount = (ethAllocation * this.exchangeRates.ETH).toFixed(6);
    },
  },
  mounted() {
    this.fetchExchangeRates();
  },
}
</script>


<template>
  <div id="app">
    <h1>Asset allocation calculator</h1>
    
    <form @submit.prevent="convertUsd" aria-labelledby="conversion-form">
      <div class="form-group-inline">
        <div class="input-group">
          <label for="usd">Investable assets (USD)</label>
          <input
            type="number"
            id="usd"
            v-model="usdInput"
            placeholder="Enter USD"
            aria-describedby="usd-help"
            aria-required="true"
          />
          <p id="usd-help" class="help-text">Enter the amount of holdings in USD</p>
        </div>
        <button type="submit" aria-label="Convert USD to Crypto">Calculate</button>
      </div>

      <div class="form-group">
        <label for="btc">70% in Bitcoin (BTC):</label>
        <input
          type="number"
          id="btc"
          v-model="btcAmount"
          readonly
          aria-readonly="true"
          aria-describedby="btc-help-text"
        />
        <p id="btc-help-text" class="help-text">Amount to allocate to Bitcoin</p>
      </div>
      
      <div class="form-group">
        <label for="eth">30% in Ethereum (ETH):</label>
        <input
          type="number"
          id="eth"
          v-model="ethAmount"
          readonly
          aria-readonly="true"
          aria-describedby="eth-help-text"
        />
        <p id="eth-help-text" class="help-text">Amount to allocate to Ethereum</p>
      </div>
      
    </form>
  </div>
</template>

<style scoped>
#app {
  font-family: sans-serif;
  text-align: center;
  margin-top: 20px;
  padding: 0 20px;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.form-group {
  width: 100%;
  max-width: 500px;
  margin-bottom: 20px;
  text-align: left;
}

.form-group-inline {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
  width: 100%;
  max-width: 562px;
}

.input-group {
  display: flex;
  flex-direction: column;
  width: 70%;
}

label {
  margin-bottom: 5px;
}

input {
  width: 100%;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
  box-sizing: border-box;
}

input[readonly] {
  background-color: #f7faff;
  color: #666;
}

button {
  padding: 10px 20px;
  font-size: 1rem;
  color: white;
  background-color: #0033ff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
}

button:hover {
  background-color: #0030b3;
}

.help-text {
  font-size: 0.8rem;
  color: #666;
}

/* Styles for Mobile Widths */
@media (max-width: 600px) {

  .form-group-inline {
    flex-direction: column;
  }

  .input-group {
    width: 100%;
  }

  h1 {
    font-size: 1.5rem;
  }

  .form-group {
    width: 100%;
  }

  button {
    font-size: 1rem;
    width: 100%;
  }
}
</style>
