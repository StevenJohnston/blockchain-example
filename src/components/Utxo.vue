<template>
  <div class="utxo" v-on:click="click()">
    <p class="address">{{transaction.address}}</p>
    <p class="amount">{{transaction.amount}}</p>
  </div>
</template>

<script>
  export default {
    name: 'Unspent',
    props: [
      'transaction',
      'index'
    ],
    data () {
      return {
      }
    },
    methods: {
      click() {
        console.log(this.index);
        let index = this.index
        this.$emit('click', index);
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.address {
  width: 60%;
  display: inline;
}
.amount {
  width: 40%;
  display: inline;
}
.utxo {
  padding: 10px;
  box-sizing: border-box;
}

</style>
