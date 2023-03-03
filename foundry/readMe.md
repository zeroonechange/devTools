# Foundry 使用指南啊


## 基本使用

```ssh
创建新的项目
	$ forge init hello_foundry
	$ forge build
	$ forge test

导入一个项目
	$ git clone https://github.com/abigger87/femplate
	$ cd femplate
	$ forge install
	$ forge build
	$ forge test

安装依赖
	$ forge install transmissions11/solmate
	$ forge install transmissions11/solmate@v7
	remapping 依赖库  通过 remappingx.txt 
更新依赖
	$ forge update lib/solmate
移除依赖
	$ forge remove solmate
	$ forge remove lib/solmate

工程布局
.
├── foundry.toml			   配置文件 
├── lib
│   └── forge-std
│       ├── LICENSE-APACHE
│       ├── LICENSE-MIT
│       ├── README.md
│       ├── foundry.toml
│       ├── lib
│       └── src
├── script
│   └── Counter.s.sol
├── src
│   └── Counter.sol
└── test
    └── Counter.t.sol

7 directories, 8 files


------------------------------
Forge 
	tests, builds, and deploys your smart contracts.
测试
	所有测试都是由solidity写的 不是 ethers.js 
	都放在/test 目录下  文件形式 xx.t.sol
	可以单独跑一个测试   用过滤词 --match-contract  和  --match-test
		$ forge test --match-contract ComplicatedContractTest --match-test testDeposit
		还有其他过滤词  --no-match-contract and --no-match-test
	也可以单独跑一个测试文件     过滤词  --match-path   当然也有 --no-match-path
		$ forge test --match-path test/ContractB.t.sol
	打印日志  -v    分别为 -vv  -vvv -vvvv -vvvvv    v越多  打印出来的东西就越多  
		-vv    输出所有测试的 logs
		-vvv   输出失败测试的 stack trace
		-vvvv  输出 stack trace， 并输出失败用例的 setup
		-vvvvv 输出 stack trace 和 setup
	可以re-run 测试  当改了一些东西    只会再跑那些改动过的测试方法   forge test --watch 
	如果要re-run 所有测试  使用  forge test --watch --run-all 命令 

写测试
	标准库 forge-std   导入 import "forge-std/Test.sol";
	测试都部署在 0xb4c79daB8f259C7Aee6E5b2Aa729821864227e84  
	测试方法不要用 internal 或  private 修饰
	setup 方法共享  创建一个 help abstract 合约 然后继承 
cheatcodes
	作弊码能操控区块链的数据  例如改变区块 自己的地址  部署在 0x7109709ECfa91a80626fF3989D68f67F5b1DD12D 
	通过 vm 实例获取作弊码 
	testFailXXX 开头的 如果执行合约函数 revert  直接是 passed 
		如果 改成 testXXX  如果 revert 会告诉我们到底发生了什么   例如 revert Unauthorized();
		但如果要通过  那么 在测试方法最前面添加 vm.expectRevert(Unauthorized.selector); 就直接 passed 
	expectEmit 针对 事件 Event   最多三个 indexed  构成一个 topic  可以用来在区块链上进行搜索
		vm.expectEmit(true, true, false, true);  想检查 第一个 第二个的 indexed 在topic 中  第三个不检查了  第四个是data 也检查
		这里有个很奇怪的设定 在 vm.expectEmit() 后要主动去 emit Event 里面的参数要和 合约函数执行的一样才行 
		也就是 测试框架 先 emit 一个 event  然后目标合约再 emit 一次  俩个对比ok  才 passed 
标准库
	代码在 lib/forge-std/src 下
		Vm.sol: Up-to-date cheatcodes interface
		console.sol and console2.sol: Hardhat-style logging functionality
		Script.sol: Basic utilities for Solidity scripting
		Test.sol: A superset of DSTest containing standard libraries, a cheatcodes instance (vm), and Hardhat console
	分别是 作弊码的接口  打日志  脚本工具  测试类的父类  里面包含了标准库 vm实例  和 hardhat日志输出 
		vm.startPrank(alice);  // Access Hevm via the `vm` instance
		assertEq(dai.balanceOf(alice), 10000e18); // Assert and log using Dappsys Test
		console.log(alice.balance);   // Log with the Hardhat `console` (`console2`)
		deal(address(dai), alice, 10000e18);  // Use anything from the Forge Std std-libraries
	导入使用
		import "forge-std/Vm.sol";
		import "forge-std/console.sol";
		import "forge-std/console2.sol";   // 这个包含补丁 解码trace 不适用于hardhat 
	六大标准库
		Std Logs         lib/forge-std/lib/ds-test/src/test.sol
		Std Assertions   同上
		Std Cheats       lib/forge-std/src/StdCheats.sol
		Std Errors		 lib/forge-std/src/StdError.sol
		Std Storage		 lib/forge-std/src/StdStorage.sol   操控合约storage 
		Std Math		 lib/forge-std/src/StdMath.sol
理解Traces
	得到 trace for failing tests(-vvv) or all tests(-vvvv)
	trace 里面包含的信息如下: 
	  [<Gas Usage>] <Contract>::<Function>(<Parameters>)
		├─ [<Gas Usage>] <Contract>::<Function>(<Parameters>)
		│   └─ ← <Return Value>
		└─ ← <Return Value>
	trace 里面可能包含 子 trace  例如 debugging 里面的东西一样  
	颜色设定如下:
		Green: For calls that do not revert
		Red: For reverting calls
		Blue: For calls to cheat codes
		Cyan: For emitted logs
		Yellow: For contract deployments	
	一个函数的 gas usage 可能 和子函数的 gas usage 对不上 那是因为有额外的操作  forge尽可能的做到 实际不可能
Fork Testing
	俩种模式  
	Forking Mode
		在一个fork环境上跑所有的测试   forge test --fork-url <your_rpc_url>
		下面的信息就是fork时候带来的
			block_number
			chain_id
			gas_limit
			gas_price
			block_base_fee_per_gas
			block_coinbase
			block_timestamp
			block_difficulty	
		模拟真实的网络环境  可以指定区块号 例如 forge test --fork-url <your_rpc_url> --fork-block-number 1
		如果 --fork-url 和  --fork-block-number 都指定了 那么就缓存下来了 为后面的测试使用
			缓存位于 ~/.foundry/cache/rpc/<chain name>/<block number> 
			移除缓存  可以删除文件夹 或 使用命令  forge clean 
			也可以追加命令 --no-storage-caching  
			或 foundry.toml 文件 配置  no_storage_caching and rpc_storage_caching 
		trace 优化: 把区块链浏览器的  API key 传进去即可   这个东西可以设置为环境变量 
			forge test --fork-url <your_rpc_url> --etherscan-api-key <your_etherscan_api_key>   
	Forking Cheatcodes
		每个测试函数都是独立fork EVM环境    每个测试的 state 都是在 setup 后的复制 
		可以使用 createFork 创建 fork     都带有 uint256 的  forkId  在创建的时候初始化
		选择环境: selectFork(forkId)
		createSelectFork: 就是 先创建fork-createFork  然后选择当前Fork - selectFork 
		每一刻只能有一个fork处于激活状态  其他的可以通过 activeFork再次激活使用 
		运行原理:
			每个fork是独立的EVM  使用了独立的storage 唯一不同的就是 msg.sender 和 测试合约本身 
			测试合约的改动会保持  尽管fork切来切去的 因为 test contract是一个 persistent account
			persistent account 理解为本地memory  
			只有 msg.sender 和 test contract  是 persistent 
			但是其他 account 可以通过 makePersistent 使其成为 persistent  
		
```



## 安装
```ssh
windows 用  WSL   
下个WSL Ubuntu  
        wsl -l -v 
        wsl --install -d Ubuntu
        wsl -l -o 
如果要卸载就用    wsl --unregister Ubuntu

 先要解决网络代理的问题

 https://zhuanlan.zhihu.com/p/451198301
 
 Clash for Windows科学上网    打开 Allow Lan 然后看看IP地址 和端口 

export http_proxy='http://172.21.64.1:7890'  
export https_proxy='http://172.21.64.1:7890'
export all_proxy='socks5://172.21.64.1:7890'
export ALL_PROXY='socks5://172.21.64.1:7890'

用 wget www.google.com  查看下是否可以了 最后

curl -L https://foundry.paradigm.xyz | bash

source /home/zeroonechange/.bashrc

foundryup


解决在 WSL terminal 中输入 code . 没有打开 VSCode的问题  
找到路径  F:\allinweb3\dev\vscode\Microsoft VS Code\bin  添加到 windows 环境变量的 path 中去  


https://github.com/foundry-rs/foundry
https://book.getfoundry.sh/getting-started/installation
https://learn.microsoft.com/zh-cn/windows/wsl/tutorials/wsl-vscode
https://www.luogu.com.cn/blog/Quank-The-OI-er/VSCode-On-Windows-10

```