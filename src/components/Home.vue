<template>
  <div class="home">
    <h1>Blocks</h1>
    <div class="main">
      <div class="blockchain">
        <block @blockUpdate="blockUpdate" v-for="block in blocks" v-bind:block="block"/>
      </div>
      <div class="unconfirmedTransactions">
        <h2>Next Block</h2>
        <unconfirmed v-for="unconfirmed in unconfirmedTransactions" v-bind:unconfirmed="unconfirmed"/>

        <h4>Difficulty <input type="text" v-model="difficulty"></h4>
        <button v-on:click="mineNextBlock">Mine Block</button>
      </div>
      <div class="createTransaction">
        <div class="maker">
          <h2>Inputs</h2>
          <div class="input" v-for="transaction in nextBlock.inputs">
            <div>
              {{transaction.address}} 
              {{transaction.amount}}
            </div>
          </div>
          <h2>Outputs</h2>
          <div class="output" v-for="transaction in nextBlock.outputs">
            <div>
              <label>Address</label>
              <input v-model="transaction.address" type="text">
            </div>
            <div>
              <label>Amount</label>
              <input v-model="transaction.amount" type="text">
            </div>
          </div>
          <button v-on:click="addOutput()">Add Output</button>
          <button v-on:click="submitTransaction()">Create Transaction</button>
        </div>
        <div class="transactionList">
          <h1>Unspent</h1>
          <utxo v-on:click="clicked" v-for="(transaction, index) in utxo" v-bind:transaction="transaction" v-bind:index="index"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Block from './Block';
  import Utxo from './Utxo';
  import Unconfirmed from './Unconfirmed';
  import md5 from 'md5';
  export default {
    name: 'Home',
    components: {
      block: Block,
      utxo: Utxo,
      unconfirmed: Unconfirmed
    },
    methods: {
      blockUpdate() {
        let lastBlock = '';
        this.blocks.forEach((block) => {
          block.head.last = lastBlock;
          lastBlock = block.head.hash;
        })
      },
      mineNextBlock() {
        let body = this.unconfirmedTransactions;
        this.unconfirmedTransactions = [];
        let last = '';
        if (this.blocks.length) {
          last = this.blocks[this.blocks.length - 1].head.hash 
        }
        let head = {
          bodyHash: md5(JSON.stringify(body)),
          time: new Date(),
          last: last,
          difficulty: parseInt(this.difficulty)
        }
        let nonce = 0;
        let hash = '';
        let difficultyMatch = Array(parseInt(this.difficulty) + 1).join('0');
        do {
          head.nonce = nonce;
          hash = md5(JSON.stringify(head));
          nonce++;
        } while (!hash.startsWith(difficultyMatch));
        head.hash = hash;
        this.blocks.push({
          head: head,
          body: body
        });
        let that = this;
        body.forEach((transaction) => {
          transaction.outputs.forEach(ouput => {
            that.utxo.push(ouput);
          })
        });
        this.utxo.push(
          {
            address: md5(Math.random()),
            amount: 50
          }
        );
        console.log(this.blocks)
      },
      submitTransaction() {
        this.unconfirmedTransactions.push({
          inputs: this.nextBlock.inputs,
          outputs: this.nextBlock.outputs
        });
        this.nextBlock = {
          inputs: [],
          outputs: []
        }
        this.nextBlock.inputs = [];
        this.nextBlock.outputs = [];
      },
      addOutput() {
        this.nextBlock.outputs.push({
          address: md5(Math.random()), 
          amount: 0
        });
      },
      clicked(index) {
        this.nextBlock.inputs.push(this.utxo[index]);
        //Remove from utxo list
        this.utxo.splice(index,1);
      },
      mine() {
        let nonce = 0;
        let hash;
        let difficultyMatch = Array(this.difficulty).join('0');
        do {
          hash = this.step(block, nonce);
        } while (hash.startsWith(difficultyMatch));

      },
      step(block, nonce) {
        return md5(nonce + JSON.stringify(block));
      },
      simpleHash(block, nonce) {
        return md5(nonce + JSON.stringify(block));
      },
    },
    data () {
      return {
        nextBlock: {
          inputs: [],
          outputs: []
        },
        difficulty: 2,
        utxo: [
          {
            address: md5(Math.random()),
            amount: '50'
          },
          {
            address: md5(Math.random()),
            amount: '50'
          }
        ],
        unconfirmedTransactions: [],
        blocks: []
      }
    },
    mounted() {
      //this.mine();
      setInterval(() => {
      }, 1000);
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .main {
    display: flex;
    flex-direction: row;
  }
  .blockchain {
    display: flex;
    flex-direction: row;
    flex-basis: 80%;
    overflow-x: scroll;
  }
  .unconfirmedTransactions {
    flex-basis: 20%;
    border: 1px solid black;
  }
  .createTransaction {
    flex-basis: 20%;
    border: 1px solid black;
    overflow: scroll;
    max-height: 80vh
  }
  .output {
    padding: 5px;
  }
</style>
