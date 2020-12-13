<template>
  <div class="hello">
    <p>Status: {{walletStatus}}</p>
    <p>Network Id: {{networkId}}</p>
    <p>Account: {{account}}</p>
    <p>Balance: {{balance}}</p>
    <br><hr><br><br>
    <h3>TOKEN</h3>
    <p>Token Address: {{token_address}}</p>
    <p>Token Name: {{token_name}}</p>
    <p>Token Symbol: {{token_symbol}}</p>
    <p>Token Total Supply: {{token_total_supply}}</p>
    <p>Your Token Balance: {{walletTokenBalance}}</p>
    <br><br><br><br><br><br><br>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      walletStatus: "Not Connected",
      networkId: "",
      account: "",
      balance: "",
      token_address: "",
      token_name: "",
      token_symbol: "",
      token_total_supply: "",
      walletTokenBalance: "",
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
          await window.ethereum.enable();
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

      // WEB token: https://ropsten.etherscan.io/token/0xecfbed50a92c5b329c57ddcd5b89cb1c2a79372e
      var token_address = "0xEcfBed50a92c5b329C57ddcd5b89cB1C2A79372e";
      var token_abi = [{"inputs":[{"internalType":"uint256","name":"total_supply","type":"uint256"},{"internalType":"uint256","name":"initial_supply","type":"uint256"}],"payable":false,"stateMutability":"nonpayable","type":"constructor"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"owner","type":"address"},{"indexed":true,"internalType":"address","name":"spender","type":"address"},{"indexed":false,"internalType":"uint256","name":"value","type":"uint256"}],"name":"Approval","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"from","type":"address"},{"indexed":true,"internalType":"address","name":"to","type":"address"},{"indexed":false,"internalType":"uint256","name":"value","type":"uint256"}],"name":"Transfer","type":"event"},{"constant":false,"inputs":[{"internalType":"address","name":"_minter","type":"address"}],"name":"addMinter","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"internalType":"address","name":"owner","type":"address"},{"internalType":"address","name":"spender","type":"address"}],"name":"allowance","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"spender","type":"address"},{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"approve","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"internalType":"address","name":"account","type":"address"}],"name":"balanceOf","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"burn","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"account","type":"address"},{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"burnFrom","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"decimals","outputs":[{"internalType":"uint8","name":"","type":"uint8"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"spender","type":"address"},{"internalType":"uint256","name":"subtractedValue","type":"uint256"}],"name":"decreaseAllowance","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"governance","outputs":[{"internalType":"address","name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"spender","type":"address"},{"internalType":"uint256","name":"addedValue","type":"uint256"}],"name":"increaseAllowance","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"account","type":"address"},{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"mint","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"internalType":"address","name":"","type":"address"}],"name":"minters","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"name","outputs":[{"internalType":"string","name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"_minter","type":"address"}],"name":"removeMinter","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"_governance","type":"address"}],"name":"setGovernance","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"setTotalSupply","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"symbol","outputs":[{"internalType":"string","name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"totalSupply","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"recipient","type":"address"},{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"transfer","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"sender","type":"address"},{"internalType":"address","name":"recipient","type":"address"},{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"transferFrom","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"}];
      var contract = new web3.eth.Contract(token_abi, token_address);
      
      // read name function
      contract.methods.name().call((err, res) => {
        this.token_name = res;
        if(res != null) {
          this.token_address = token_address;
        }
      });

      // read symbol function
      contract.methods.symbol().call((err, res) => {
        this.token_symbol = res;
      });

      // read totalSupply function
      contract.methods.totalSupply().call((err, res) => {
        this.token_total_supply = web3.utils.fromWei(res, 'ether');
      });

      // read balanceOf function
      contract.methods.balanceOf(this.account).call((err, res) => {
        this.walletTokenBalance = web3.utils.fromWei(res, 'ether');
      });
    },
  }
}
</script>