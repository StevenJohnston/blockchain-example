<template>
  <div class="transaction">
    <h1>inputs</h1>
    <div v-for="input in transaction.inputs">
      <input type="text" v-model="input.amount">
      <p>{{input.amount}}</p>
      <p>{{input.address}}</p>
    </div>
    <h1>outputs</h1>
    <div v-for="output in transaction.outputs">
      <p>{{output.amount}}</p>
      <p>{{output.address}}</p>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Transaction',
    props: [
      'transaction'
    ],
    data () {
      return {
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
  }
  .transaction {
    border: black solid 1px;
    background-color: rgb(179, 194, 194);
    padding: 2px;
    box-sizing: border-box;
  }
</style>
