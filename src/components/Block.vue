<template>
  <div class="block" v-bind:class="{error: hasError}">
    <div class="head">
      <h2>Head</h2>
      <h4>Hash</h4>
      <p>{{ block.head.hash }}</p>
      <h4>Time</h4>
      <p>{{ block.head.time }}</p>
      <h4>Prev Block</h4>
      <p>{{ block.head.last }}</p>
      <h4>Nonce</h4>
      <input type="number" v-model="block.head.nonce">
      <h4>Body Hash</h4>
      <p>{{ block.head.bodyHash }}</p>
    </div>
    <h2>Body</h2>
    <transaction v-for="transaction in block.body" v-bind:transaction="transaction"/>
    <div v-if="block.body.length == 0">
      <h1>No transactions</h1>
    </div>

  </div>
</template>

<script>
  import Transaction from './Transaction';
  import md5 from 'md5';
  export default {
    name: 'Block',
    props: [
      'block'
    ],
    computed: {
      hasError() {
        //let tempHead = Object.assign({}, this.block.head);
        let tempHead = this.block.head;
        let bodyHash = md5(JSON.stringify(this.block.body));
        tempHead.bodyHash = bodyHash;
        tempHead.nonce = parseInt(tempHead.nonce);
        tempHead.difficulty = parseInt(tempHead.difficulty);
        delete tempHead.hash;
        let difficultyMatch = Array(parseInt(tempHead.difficulty) + 1).join('0');
        let hash = md5(JSON.stringify(tempHead));
        tempHead.hash = hash;
        this.$emit('blockUpdate');
        return !hash.startsWith(difficultyMatch);
      }
    },
    components: {
      transaction: Transaction
    },
    data () {
      return {
      }
    },
    methods: {
      verify() {
        let tempHead = Object.assign({}, this.block.head);
        delete tempHead.hash;
        return md5(JSON.stringify(tempHead));
      }
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
  }
  .block {
    border: black solid 1px;
    max-height: 80vh;
  }
  .error {
    background-color: rgb(247, 185, 185);
    border: 2px red solid;
  }
  h4 {
    margin-bottom: 0;
  }
  p {
    margin-top: 0;
  }
  .head {
    padding: 5px 10px;
    background-color: rgb(219, 253, 234);
  }
</style>
