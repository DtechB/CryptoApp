<template>
  <div class="container">
    <div class="row">
      <h1 class="mt-3">Crypto Price Coingeco API</h1>
      <div class="row">
        <div class="col-10">
          <input
              type="text"
              class="form-control rounded-1 my-4"
              placeholder="Search Coin"
              @keyup="searchCoin()"
              v-model="textSearch"
          />
        </div>
        <div class="col-2 text-end">
          <button class="btn btn-warning my-4" @click="loadData"><i class="bi bi-arrow-clockwise me-2"></i>Refresh</button>
        </div>
      </div>

      <table class="table table-striped table-responsive shadow">
        <thead class="ps-3">
        <tr>
          <th v-for="title in titles" :key="title">{{ title }}</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
          <td class="text-muted">{{ index + 1 }}</td>
          <td>
            <img :src="coin.image" style="width: 2rem" class="me-3" alt=""/>
            <span>
                {{ coin.name }}
              </span>
            <span class="ms-3 text-uppercase text-muted">
                {{ coin.symbol }}
              </span>
          </td>
          <td>{{ coin.current_price }} $</td>
          <td>{{ coin.market_cap }}</td>
          <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
              ]"
          >
            {{ coin.price_change_percentage_24h }} %
          </td>
          <td>{{ coin.total_volume.toLocaleString() }} $</td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      coins: [],
      titles: ['#', 'Coin', 'Price', 'Market Cap', 'Price Change', '24 Volume'],
      filteredCoins: [],
      textSearch: ''
    }
  },
  mounted() {
    this.loadData()
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter(
          (coin) =>
              coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
              coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      )
    },
    loadData() {
      axios
          .get('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
          .then(response => {
            console.log(response.data)
            this.coins = response.data
            this.filteredCoins = response.data
          })
    }
  }
}
</script>

<style>
</style>
