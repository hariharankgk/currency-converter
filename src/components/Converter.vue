<template>
  <div class="converter-conatainer">
    <h2 class="converter-title">Exchange Rate</h2>
    <h3 class="converter-currency"><span v-if="currencySymbol">{{currencySymbol.symbolNative}}</span> <span v-else>{{currencyTo}}</span> {{exchangePrice}}</h3>
    <div class="converter-form">
      <div class="converter-amount">
        <label class="converter-label">Amount</label>
        <input type="number" class="converter-input" min="0" v-model.number="currency">
      </div>
      <div class="converter-country">
        <div class="converter-dropdown">
          <label class="converter-label">From</label>
          <select class="converter-input" v-model="currencyFrom">
            <option value="">Please Select</option>
            <option v-for="(item, key, index) in currencyList" :key="index"  :value="key">{{key}}</option>
          </select>
        </div>
        <div class="converter-image">
          <img alt="converter-icom" src="../assets/exchange.png">
        </div>
        <div class="converter-dropdown">
          <label class="converter-label">To</label>
          <select class="converter-input" v-model="currencyTo" @change="exchangePrice = 0">
            <option value="">Please Select</option>
            <option v-for="(item, key, index) in currencyList" :key="index" :value="key">{{key}}</option>
          </select>
        </div>
      </div>
      <div class="converter-button">
        <button class="btn" @click="covert">Convert</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { currencies } from 'currencies.json';

export default {
  name: 'ConverterBlock',
  data () {
    return {
      exchangePrice: 0,
      currency: 0,
      currencyFrom: '',
      currencyTo: '',
      currencyList: []
    }
  },
  computed: {
    currencySymbol() {
      if(this.currencyTo) {
        return currencies.filter((item) => item.code === this.currencyTo)[0];
      }
      return ''
    }
  },
  methods: {
    covert() {
      if(this.currency && Number.isInteger(this.currency) && this.currency > 0) {
        if(this.currencyFrom && this.currencyTo) {
          axios.get('https://api.getgeoapi.com/v2/currency/convert?api_key=3df2183e7562c9e3436f0a91d6d9e4312ba4ab25&from='+this.currencyFrom+'&to='+this.currencyTo+'&amount='+this.currency).then((response) => {
              this.exchangePrice = this.currency * response.data.rates[this.currencyTo].rate;
          }).catch((e) => {
            console.log(e); 
            return false 
          });
        } else {
          alert('Please select the valid country');
        }
      } else {
        alert('Please enter a valid amount');
      }
    },
    getCurrencies() {
      axios.get('https://api.getgeoapi.com/v2/currency/list?api_key=3df2183e7562c9e3436f0a91d6d9e4312ba4ab25').then((response) => {
        this.currencyList = response.data.currencies;
      }).catch((e) => {
        console.log(e); 
        return false 
      });
    }
  },
  mounted() {
    this.getCurrencies();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .converter-conatainer {
    background-color: #fff;
    width: 90%;
    margin-left: auto;
    margin-right: auto;
    margin-top: 50px;
    padding: 10px;
    border-radius: 4px;
    box-shadow: 2px 2px 4px #918d8d;
    border-top: 10px solid #03a9f4;
    @media only screen and (min-width: 768px) {
      width: 60%;
      padding: 20px;
    }
    
    .converter-title {
      margin: 0;
      padding: 15px 0;
      text-align: center;
      color: #b3b3b3;
      font-weight: 200;
    }

    .converter-currency {
      font-size: 40px;
      margin: 0;
      padding: 20px 0;
      text-align: center;
    }

    .converter-form {
      padding: 20px;
      @media only screen and (min-width: 768px) {
        padding: 20px 40px;
      }

      .converter-label {
        display: inline-block;
        padding-bottom: 5px;
        color: #b3b3b3;
      }
      .converter-input {
        width: 100%;
        padding: 15px 10px;
        border: 1px solid #ddd;
        border-radius: 4px;
      }
      .converter-country {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-top: 20px;
        .converter-dropdown {
          width: 30%;
        }
        .converter-image {
          max-width: 50px;
          max-height: 50px;
          img {
            width: 100%;
          }
        }
      }

      .converter-button{
        margin-top: 25px;
        .btn {
          width: 100%;
          padding: 20px;
          color: #fff;
          font-weight: bold;
          background-color: #03a9f4;
          border:none;
          font-size: 24px;
          border-radius: 4px;
        }
      }
    }
  }
</style>
