<template>
  <div id="app">
    <app-header></app-header>
    <div class="columns">
      <div class="column"></div>
      <app-stocks class="column is-two-thirds" :stocks="stocks" :update="update"></app-stocks>
      <div class="column"></div>
    </div>
  </div>
</template>

<script>
import appHeader from './Header.vue'
import appStocks from './stocks/Home.vue'
export default {
  components : {appHeader,appStocks},
  data () {
    return {
      stocks : {},
      update: null
    }
  },
  created() {
    const stockSocket = new WebSocket("ws://stocks.mnet.website/stock"); 
    var self = this;
		stockSocket.onmessage = function handleUpdateMessage(event) {
      let data = JSON.parse(event.data);
      self.update = new Date().getTime();
		  data.forEach(([name, price]) => {
        let info = {}
        if (self.stocks[name]) {
          if(self.stocks[name].price > price)
            info.className ="is-danger";
          else
            info.className ="is-primary";
        }
        info.time = self.update;
        info.price = price.toFixed(2);
        self.$set(self.stocks, name , info);
      });
		}

  }
}
</script>

<style lang="scss">
 @import '~bulma/bulma';
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.select.is-loading.is-large:after {
  font-size: 8rem;
}
</style>
