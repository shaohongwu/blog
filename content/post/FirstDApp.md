---
title: "编写Dapp"
date: 2020-05-08T15:24:32+08:00
tags: [ "区块链" ,"智能合约"]
draft: false
---



基于ETH编写一个简单的Dapp,前端使用vue

## 工具环境 

- web3.js (1.0.5)

- Node (12.5.0)

- ganache-cli (6.9.1)

- vue (2.6.11)

  







## code

- 使用vue-cli 创建一个项目工程

  ```bash
  vue create mydapp && cd mydapp
  ```

  

- 然后配置项目依赖

  ```
  npm install web3
  npm install -g ganache-cli
  npm install -g npx
  ```

  

- 编写dapp界面，没有做路由直接在HelloWorld页面修改，为eth的链接方便，没有配置MetaMask，直连开发环境.

  

- 创建只能合约

  ```
  touch Contract.sol
  ```

  ```
  pragma solidity ^0.6.2;
  
  contract FirstContract {
  	function getInteger() public pure returns (uint) {
  		return 123;
  	}
  }
  ```

  

- 编译合约，编译好的合约会在 build 目录

  ```
  solc --bin --abi -o ./build Contract.sol
  ```

  

- 部署脚本

  ```
  npx ganache-cli
  touch deploy.js
  ```

  ```
  
  const fs = require('fs');
  const Web3 = require('web3');
  const web3 = new Web3('http://localhost:8545');
  const bytecode = fs.readFileSync('./build/FirstContract.bin');
  const abi = JSON.parse(fs.readFileSync('./build/FirstContract.abi'));
  
  (async function () {
    const ganacheAccounts = await web3.eth.getAccounts();
    const myWalletAddress = ganacheAccounts[0];
  
    const myContract = new web3.eth.Contract(abi);
  
    myContract.deploy({
      data: bytecode
    }).send({
      from: myWalletAddress,
      gas: 5000000
    }).then((deployment) => {
      console.log('FirstContract was successfully deployed!');
      console.log('FirstContract can be interfaced with at this address:');
      console.log(deployment.options.address);
    }).catch((err) => {
      console.error(err);
    });
  })();
  ```

  

- 然后开始合约的部署，在部署好后，会给出当前的合约地址

  ```
  node deploy.js
  ```

  ```
  ➜  mydapp git:(master) ✗ node deploy.js
  FirstContract was successfully deployed!
  FirstContract can be interfaced with at this address:
  0xFd06a23A3240D2a50b4cFBd50806f3c2819266c8
  ```

  

- 修改新建的页面,然后刷新界面，按键 start 能看到123，表示合约调用成功

  dapp.vue

  ```javascript
  <template>
  <div id="dapp">
  <p>{{ message }}</p>
  <button v-on:click="start">start</button>
  </div>
  
  </template>
  
  
  
  <script>
  const Web3 = require('web3');
  const web3 = new Web3('http://localhost:8545');
  export default {
  	name: 'dapp',
  	data(){return{
  		message:'test'
  	}},
  	methods:{
  		start:function () {
  			console.log(web3)
  			console.log('111')
  			const myContractAddress = '0xFd06a23A3240D2a50b4cFBd50806f3c2819266c8';
  			const myAbi = [{"inputs":[],"name":"getInteger","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"pure","type":"function"}];
  			const myContract = new web3.eth.Contract(myAbi, myContractAddress);
  			myContract.methods.getInteger().call().then((jsonRpcResult) => {
  				this.message = jsonRpcResult;
  				console.log(this.message)
  			});	
  		}
  	}
  }
  </script>
  ```



​		![](https://user-images.githubusercontent.com/39219403/81388623-1dfe4800-914b-11ea-9159-e9af6ea59856.png)

## 结束

这只是一个适合入门的教程，其中有几个问题，dapp 的合约是可以分开部署，然后就是浏览器的 web3 调用，其实这样是不合理的，常用的是调用浏览器的MetaMask插件 Github 上代码试用 MetaMask

教程的[Github](https://github.com/shaohongwu/mydapp.git)