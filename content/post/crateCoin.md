---
title: "发行token"
date: 2020-05-06T12:04:30+08:00
draft: false
---




## 工具环境 

- MAC 10.15.2 
- Ganache 2.0.2 
- Truffle v5.0.26 (core: 5.0.26)
- Solidity v0.5.0 (solc-js)
- Node v12.5.0
- Web3.js v1.0.0-beta.37
- MetaMask



## 配置

- Ganache 开启虚节点，软件给10个以太账户，每个账户100ETH方便我们的调试

  ![](https://user-images.githubusercontent.com/39219403/81184900-87fbdd80-8fe3-11ea-979d-323b549c3251.png)

- Truffle 合约开发的手脚架

- 本次使用的合约是GitHub开源的[合约](https://github.com/OpenZeppelin/openzeppelin-contracts/tree/master/contracts/token/ERC20) 

- 准备好Node环境 

  - ```
    npm install -g truffle
    ```

- 创建项目

  - ```
    mkdir meToken && cd meToken
    ```

- 初始化项目

  

  - ```console
    truffle init
    ```

![](https://user-images.githubusercontent.com/39219403/81186110-06a54a80-8fe5-11ea-9881-6d3788c132c7.png)



```
.
├── contracts
│   └── Migrations.sol
├── migrations
│   └── 1_initial_migration.js
├── test
└── truffle-config.js
```



- 修改contracts 目录文件，替换成我们的需要的合约代码

- 修改 **2_initial_migration.js**  文件配置

  

  ```javascript
  const MyToken = artifacts.require("MyToken");
  
  module.exports = function(deployer) {
    deployer.deploy(MyToken);
  };
  
  
  ```

  

- 编写自己的合约信息

  - MyToken

    ```
    pragma solidity >=0.4.21 <0.7.0;
    
    import "./token/ERC20/ERC20.sol";
    
    contract MyToken is ERC20 {
    
    	constructor() 
    	ERC20("My Custom ERC20 Token","MET")
    	public 
    	{
    	uint initialSupply = 3100000000 * 10 ** 18;
      _mint(msg.sender, initialSupply);
    	}
    }
    ```

- 修改 **truffle-config.js**  节点信息，根据Ganache的信息配置

  ![](https://user-images.githubusercontent.com/39219403/81188015-61d83c80-8fe7-11ea-9996-31b5344f2ecf.png)

  ![](https://user-images.githubusercontent.com/39219403/81188056-70beef00-8fe7-11ea-888c-142765844521.png)

- 编译合约 编译的时候注意solc版本，不然会报错,目前的版本是0.6.2 ，

  - ```bash
    truffle compile
    ```

    ![](https://user-images.githubusercontent.com/39219403/81188693-3e61c180-8fe8-11ea-8aff-ed87c1606af5.png)

    修改编译版本信息

    ```javascript
    compilers: {
        solc: {
          version: "0.6.2",    // Fetch exact version from solc-bin (default: truffle's version)
          // docker: true,        // Use "0.5.1" you've installed locally with docker (default: false)
          // settings: {          // See the solidity docs for advice about optimization and evmVersion
          //  optimizer: {
          //    enabled: false,
          //    runs: 200
          //  },
          //  evmVersion: "byzantium"
          // }
        }
      }
    ```

    

- 部署合约

  - ```
    truffle migrate
    ```

    ```
    Starting migrations...
    ======================
    > Network name:    'development'
    > Network id:      5777
    > Block gas limit: 6721975 (0x6691b7)
    
    
    1_initial_migration.js
    ======================
    
       Deploying 'Migrations'
       ----------------------
       > transaction hash:    0xf5bbe0f671b2e7d8c6770f302f5890ffe11bb7576c161b38c1de9217a4b0a3c9
       > Blocks: 0            Seconds: 0
       > contract address:    0xc16C4A9a04e8e0Fcd88AD0f96AA31651CfB45217
       > block number:        1
       > block timestamp:     1588777452
       > account:             0x907EbFF4d5969F4a43cb41B970e7014ccff53594
       > balance:             99.99622498
       > gas used:            188751 (0x2e14f)
       > gas price:           20 gwei
       > value sent:          0 ETH
       > total cost:          0.00377502 ETH
    
    
       > Saving migration to chain.
       > Saving artifacts
       -------------------------------------
       > Total cost:          0.00377502 ETH
    
    
    2_initial_migration.js
    ======================
    
       Deploying 'ERC20'
       -----------------
       > transaction hash:    0x3e0c0213f6790984fd5b46e947f3945b421d3a52786e2069c4c182e93b9989d1
       > Blocks: 0            Seconds: 0
       > contract address:    0xaF167A0a309fAFefB4356ca4Cf137b08842475d8
       > block number:        3
       > block timestamp:     1588777453
       > account:             0x907EbFF4d5969F4a43cb41B970e7014ccff53594
       > balance:             99.9692201
       > gas used:            1308243 (0x13f653)
       > gas price:           20 gwei
       > value sent:          0 ETH
       > total cost:          0.02616486 ETH
    
    
       > Saving migration to chain.
       > Saving artifacts
       -------------------------------------
       > Total cost:          0.02616486 ETH
    
    
    Summary
    =======
    > Total deployments:   2
    > Final cost:          0.02993988 ETH
    ```

    

-  Ganache 第一个账户会消费 ETH

  ![](https://user-images.githubusercontent.com/39219403/81193813-779d3000-8fee-11ea-9b05-84db063399ff.png)







- 使用chrome 打开MetaMask，配置连接到本地的节点网络

- 用Ganache 第一个账户的私钥导入到 MetaMask

  - ![](https://user-images.githubusercontent.com/39219403/81194260-0611b180-8fef-11ea-99cd-2cea8f8c5214.png)

    ![](https://user-images.githubusercontent.com/39219403/81197800-4e32d300-8ff3-11ea-97a7-b646b1f6d416.png)







## 结束

现在我们的token发行完成，你可以通过 MetaMask 转账或者查询.

代码我会放在[github](https://github.com/shaohongwu/meToken.git)

最后的目录



```
.
├── README.md
├── contracts
│   ├── GSN
│   │   ├── Context.sol
│   │   ├── GSNRecipient.sol
│   │   ├── GSNRecipientERC20Fee.sol
│   │   ├── GSNRecipientSignature.sol
│   │   ├── IRelayHub.sol
│   │   ├── IRelayRecipient.sol
│   │   └── README.adoc
│   ├── Migrations.sol
│   ├── MyToken.sol
│   ├── access
│   │   ├── AccessControl.sol
│   │   ├── Ownable.sol
│   │   └── README.adoc
│   ├── cryptography
│   │   ├── ECDSA.sol
│   │   ├── MerkleProof.sol
│   │   └── README.adoc
│   ├── drafts
│   │   └── TokenVesting.sol
│   ├── introspection
│   │   ├── ERC165.sol
│   │   ├── ERC165Checker.sol
│   │   ├── ERC1820Implementer.sol
│   │   ├── IERC165.sol
│   │   ├── IERC1820Implementer.sol
│   │   ├── IERC1820Registry.sol
│   │   └── README.adoc
│   ├── math
│   │   ├── Math.sol
│   │   ├── README.adoc
│   │   ├── SafeMath.sol
│   │   └── SignedSafeMath.sol
│   ├── mocks
│   │   ├── AccessControlMock.sol
│   │   ├── AddressImpl.sol
│   │   ├── ArraysImpl.sol
│   │   ├── ConditionalEscrowMock.sol
│   │   ├── ContextMock.sol
│   │   ├── CountersImpl.sol
│   │   ├── Create2Impl.sol
│   │   ├── ECDSAMock.sol
│   │   ├── ERC165
│   │   │   ├── ERC165InterfacesSupported.sol
│   │   │   └── ERC165NotSupported.sol
│   │   ├── ERC165CheckerMock.sol
│   │   ├── ERC165Mock.sol
│   │   ├── ERC1820ImplementerMock.sol
│   │   ├── ERC20BurnableMock.sol
│   │   ├── ERC20CappedMock.sol
│   │   ├── ERC20DecimalsMock.sol
│   │   ├── ERC20Mock.sol
│   │   ├── ERC20PausableMock.sol
│   │   ├── ERC20SnapshotMock.sol
│   │   ├── ERC721BurnableMock.sol
│   │   ├── ERC721GSNRecipientMock.sol
│   │   ├── ERC721Mock.sol
│   │   ├── ERC721PausableMock.sol
│   │   ├── ERC721ReceiverMock.sol
│   │   ├── ERC777Mock.sol
│   │   ├── ERC777SenderRecipientMock.sol
│   │   ├── EnumerableMapMock.sol
│   │   ├── EnumerableSetMock.sol
│   │   ├── EtherReceiverMock.sol
│   │   ├── GSNRecipientERC20FeeMock.sol
│   │   ├── GSNRecipientMock.sol
│   │   ├── GSNRecipientSignatureMock.sol
│   │   ├── MathMock.sol
│   │   ├── MerkleProofWrapper.sol
│   │   ├── OwnableMock.sol
│   │   ├── PausableMock.sol
│   │   ├── PullPaymentMock.sol
│   │   ├── ReentrancyAttack.sol
│   │   ├── ReentrancyMock.sol
│   │   ├── SafeCastMock.sol
│   │   ├── SafeERC20Helper.sol
│   │   ├── SafeMathMock.sol
│   │   ├── SignedSafeMathMock.sol
│   │   └── StringsMock.sol
│   ├── payment
│   │   ├── PaymentSplitter.sol
│   │   ├── PullPayment.sol
│   │   ├── README.adoc
│   │   └── escrow
│   │       ├── ConditionalEscrow.sol
│   │       ├── Escrow.sol
│   │       └── RefundEscrow.sol
│   ├── presets
│   │   ├── ERC20PresetMinterPauser.sol
│   │   ├── ERC721PresetMinterPauserAutoId.sol
│   │   └── README.adoc
│   ├── token
│   │   ├── ERC20
│   │   │   ├── ERC20.sol
│   │   │   ├── ERC20Burnable.sol
│   │   │   ├── ERC20Capped.sol
│   │   │   ├── ERC20Pausable.sol
│   │   │   ├── ERC20Snapshot.sol
│   │   │   ├── IERC20.sol
│   │   │   ├── README.adoc
│   │   │   ├── SafeERC20.sol
│   │   │   └── TokenTimelock.sol
│   │   ├── ERC721
│   │   │   ├── ERC721.sol
│   │   │   ├── ERC721Burnable.sol
│   │   │   ├── ERC721Holder.sol
│   │   │   ├── ERC721Pausable.sol
│   │   │   ├── IERC721.sol
│   │   │   ├── IERC721Enumerable.sol
│   │   │   ├── IERC721Metadata.sol
│   │   │   ├── IERC721Receiver.sol
│   │   │   └── README.adoc
│   │   └── ERC777
│   │       ├── ERC777.sol
│   │       ├── IERC777.sol
│   │       ├── IERC777Recipient.sol
│   │       ├── IERC777Sender.sol
│   │       └── README.adoc
│   └── utils
│       ├── Address.sol
│       ├── Arrays.sol
│       ├── Counters.sol
│       ├── Create2.sol
│       ├── EnumerableMap.sol
│       ├── EnumerableSet.sol
│       ├── Pausable.sol
│       ├── README.adoc
│       ├── ReentrancyGuard.sol
│       ├── SafeCast.sol
│       └── Strings.sol
├── migrations
│   ├── 1_initial_migration.js
│   └── 2_initial_migration.js
├── test
└── truffle-config.js
```

