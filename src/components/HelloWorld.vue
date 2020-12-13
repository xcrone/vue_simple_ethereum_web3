<template>
  <div class="hello">
    <p>Status: {{walletStatus}}</p>
    <p>Network Id: {{networkId}}</p>
    <p>Account: {{account}}</p>
    <p>Balance: {{balance}}</p>
    <br><br><br>
    <p>{{output}}</p>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  data() {
    return {
      walletStatus: "Not Connected",
      account: "-",
      balance: "-",
      networkId: "-",
      output: "",
    }
  },
  created() {
    //detect wallet
    this.walletInstalled();
  },
  methods: {
    async walletInstalled() {
      const metamaskInstalled = typeof window.web3 !== 'undefined'; //boolean value return
      if(metamaskInstalled) {
        //connect to wallet
        await this.connectWallet();
        //load data if wallet connected
        await this.loadData();
      }else {
        alert("Please install metamask");
      }
    },
    async connectWallet() {
      var Web3 = require("web3");
      if(window.ethereum) {
        window.web3 = new Web3(window.ethereum);
        // ask permission to connect
        try {
          await window.eth_requestAccounts();
          this.walletStatus = "Connected";
        }catch(err) {
          this.walletStatus = "Failed";
        }
      }else if(window.web3) {
        //create a new connection to blockchain
        window.web3 = new Web3(window.web3.currentProvider);
      }
    },
    async loadData() {
      const web3 = window.web3;
      var account = await web3.eth.getAccounts();

      // get wallet address
      this.account = account[0];

      // get network ID
      this.networkId = await web3.eth.net.getId();

      // get wallet balance
      await web3.eth.getBalance(this.account, (err, bal) => {
          this.balance = web3.utils.fromWei(bal, 'ether')
        },
      );

    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
