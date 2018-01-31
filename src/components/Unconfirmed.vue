<template>
  <div class="unconfirmed">
    <h3>inputs</h3>
    <div v-for="input in unconfirmed.inputs">
      <p>{{input.address}}</p>
      <p>{{input.amount}}</p>
    </div>
    <h3>ouputs</h3>
    <div v-for="output in unconfirmed.outputs">
      <p>{{output.address}}</p>
      <p>{{output.amount}}</p>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Unconfirmed',
    props: [
      'unconfirmed'
    ],
    data () {
      return {
      }
    },
    methods: {
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 .unconfirmed p {
   display: inline;
 }
</style>
