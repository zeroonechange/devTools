# Cast 使用指南啊

https://learnblockchain.cn/docs/foundry/i18n/zh/index.html
https://book.getfoundry.sh/
https://github.com/foundry-rs/foundry/tree/master/config


```ssh

https://eth-mainnet.g.alchemy.com/v2/qXFbrurfVJ5Le9N3T3HwEi8Wc06v0Ud3
https://eth-goerli.g.alchemy.com/v2/ybl4NUkj7Q4_odQ1NWNL2ZyYCFcOMSd5
private key: a028b80f557f2ff8edd2b39fe749b21c10669d78381df8c6e2f2afc362efa1ee
account: 0x5E46077F3DD9462D9F559FF38F76d54F762e79fF 


Wallet Commands:
	创建一个新的随机密钥对
		cast wallet new   可以追加保存目录
	将一个私钥转换为一个地址
		cast wallet address  --private-key a028b80f557f2ff8edd2b39fe749b21c10669d78381df8c6e2f2afc362efa1ee
		cast wallet address --keystore keystore.json
	签署消息
		cast wallet sign --private-key a028b80f557f2ff8edd2b39fe749b21c10669d78381df8c6e2f2afc362efa1ee  "hello"
	生成一个虚构的地址
		创建一个以 dead 开头的新密钥对	cast wallet vanity --starts-with dead
		创建一个以 beef 结尾的新密钥对	cast wallet vanity --ends-with beef
	验证一个信息的签名
		$ cast wallet verify  --address 0x5E46077F3DD9462D9F559FF38F76d54F762e79fF "hello" 0xbaa2f9573985379afed1c94b986e28d96013e659d45a888163c7df1fe166434818032cb63b61be0b5cadac2c3ad304b5f9cb9fd5ff1226684a652644274694ff1c
		Validation succeeded. Address 0x5E46077F3DD9462D9F559FF38F76d54F762e79fF signed this message.
		
	
Utility Commands:
	获取一个函数的选择器
		$ cast sig "transfer(address,uint256)"
		0xa9059cbb
	使用 keccak-256 对任意数据进行哈希
	从给定的 nonce 和部署者地址计算合约地址
	从一个给定的 ABI 生成一个 Solidity 接口
		cast interface ./path/to/abi.json			从一个文件中生成一个接口
		cast interface -n LilENS ./path/to/abi.json 从一个文件中生成并命名一个接口
	计算集合中条目的存储插槽位置   cast index key_type key slot
		mapping (address => uint256) public mapping1;
		mapping (string => string) public mapping2;
		cast index string "hello" 1   -->  0x3556fc8e3c702d4479a1ab7928dd05d87508462a12f53307b5407c969223d1f8
	串接十六进制字符串
		$ cast --concat-hex 0xa 0xb 0xc
		0xabc
	获取 int256 最大/小值
		cast --max-int				cast --min-int
	获取 uint256 最大/小值
		cast --max-uint
	将一个地址转换为校验过的格式		例如大小写  缺少 0x 开头
		cast --to-checksum-address C02aaA39b223FE8D0A0e5C4F27eAD9083C756CC2 


Conversion Commands:
	将一个字符串转换成 bytes32 编码
		cast --format-bytes32-string "hello"     ->  0x68656c6c6f000000000000000000000000000000000000000000000000000000
	将二进制数据转换为十六进制数据
		cast --from-bin 
	将一个定点数转换成一个整数
		cast --from-fix 2 10.55
	对 RLP 编码的数据进行解码
		cast --from-rlp 0xc481f181f2     ->  ["0xf1","0xf2"] 
	将 UTF8 文本转换为十六进制
		cast --from-utf8 "hello"
	从 bytes32 编码中解析出一个字符串
		cast --parse-bytes32-string "0x68656c6c6f000000000000000000000000000000000000000000000000000000"
	将十六进制数据转换为ASCII字符串
		cast --to-ascii "0x68656c6c6f"
	将一个进制底数转换为其它底数
		$ cast --to-base 64 hex			将十进制数字 64 转换为十六进制
		0x40
		$ cast --to-base 0x100 2		将十六进制数字 100 转换为二进制
		0b100000000
	右移十六进制数据至 32 字节
		$ cast --to-bytes32 0x68
		0x6800000000000000000000000000000000000000000000000000000000000000		
	将一个整数转换成一个定点数
		cast --to-fix 2 250		  --> 2.50 	将 250 转换为带 2 位小数的定点数
	将输入规范化为小写，0x- 前缀的十六进制
		$ cast --to-hexdata "deadbeef:0xbeef"
		0xdeadbeefbeef
		$ cast --to-hexdata deadbeef
		0xdeadbeef		
	将一个数字转换为十六进制编码的 int256
		$ cast --to-int256 1001213123213123
		0x00000000000000000000000000000000000000000000000000038e99188db743		
	将十六进制数据编码为 RLP
		$ cast --to-rlp '["0xaa","0xbb","cc"]'
		0xc681aa81bb81cc
		$ cast --to-rlp f0a9
		0x82f0a9	
	将一个数字转换成十六进制编码的 uint256
		$ cast --to-uint256 1001213123213123
	将一个 eth 单位转换为另一个单位
		$ cast --to-unit 1000 gwei     转换 1000 wei 为 gwei
		0.000001000
		$ cast --to-unit 1ether gwei    转换 1 eth 为 gwei
		1000000000		
	将 eth 金额转换为 wei 单位
		cast --to-wei 1.213213123
	左移操作	
		$ cast shl --base-in 10 61 3      对数字61进行3位左移
		0x1e8
	右移操作
		$ cast shr --base-in 16 0x12 1    对 0x12 的单一右移
		0x9
	
	
ABI Commands:
	cast abi-encode "someFunc(address,uint256)" 0x39DBfDD63FD491A228A5b601e0662a4014540347 1 对给定的函数参数进行 ABI 编码，不包括选择器
	cast 4byte 0x8cc5ce99   获取指定选择器的函数签名   rewardController()
	
	对 ABI 编码的 calldata 进行解码
	$ cast 4byte-decode 0xa9059cbb000000000000000000000000e78388b4ce79068e89bf8aa7f218ef6b9ab0e9d00000000000000000000000000000000000000000000000000174b37380cea000
	1) "transfer(address,uint256)"
	0xE78388b4CE79068e89Bf8aA7f218eF6b9AB0e9d0
	104906000000000000			
	
	获取 topic 0 的事件签名
	$ cast 4byte-event 0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef
	Transfer(address,address,uint256)

	ABI 编码一个带参数的函数
	$ cast calldata "someFunc(address,uint256)" 0xE78388b4CE79068e89Bf8aA7f218eF6b9AB0e9d0 1
	0xd90a6a67000000000000000000000000e78388b4ce79068e89bf8aa7f218ef6b9ab0e9d00000000000000000000000000000000000000000000000000000000000000001
		
	重打印 Calldata
	$ cast pretty-calldata 0xa9059cbb000000000000000000000000e78388b4ce79068e89bf8aa7f218ef6b9ab0e9d00000000000000000000000000000000000000000000000000174b37380cea000

	 Possible methods:
	 - transfer(address,uint256)
	 ------------
	 [0]:  000000000000000000000000e78388b4ce79068e89bf8aa7f218ef6b9ab0e9d0
	 [1]:  0000000000000000000000000000000000000000000000000174b37380cea000
	
	解码 ABI 编码的输入或输出数据
	$ cast --abi-decode "balanceOf(address)(uint256)"   0x000000000000000000000000000000000000000000000000000000000000000a
	10	
	$ cast --abi-decode --input "transfer(address,uint256)"   0xa9059cbb000000000000000000000000e78388b4ce79068e89bf8aa7f218ef6b9ab0e9d0000000000000000000000000000000000000000000000000008a8e4b1a3d8000
	0xE78388b4CE79068e89Bf8aA7f218eF6b9AB0e9d0
	39000000000000000
	
	解码 ABI 编码的输入数据
	$ cast --calldata-decode "transfer(address,uint256)"   0xa9059cbb000000000000000000000000e78388b4ce79068e89bf8aa7f218ef6b9ab0e9d0000000000000000000000000000000000000000000000000008a8e4b1a3d8000
	0xE78388b4CE79068e89Bf8aA7f218eF6b9AB0e9d0
	39000000000000000
	
	
Etherscan Commands: 
	从 Etherscan 获取合约的源代码
	
	
ENS Commands:
	cast lookup-address 0x39DBfDD63FD491A228A5b601e0662a4014540347    ENS 反向查询
	cast resolve-name vitalik.eth --rpc-url=$ETH_RPC_URL             ENS 查询
	cast namehash  vitalik.eth   									 计算一个名字的 ENS namehash
	
	
Account Commands:
	cast balance beer.eth --rpc-url=$ETH_RPC_URL	获取一个账户的余额，单位为 Wei
	cast storage 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 0 --rpc-url=$ETH_RPC_URL 获取一个合约的存储槽的原始值
	cast proof 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 0  --rpc-url=$ETH_RPC_URL 生成存储证明  暂时不知道干啥的
	cast nonce beer.eth --rpc-url=$ETH_RPC_URL   获取一个账户的 nonce  为啥个人账户也要有 nounce  防止重入攻击
	cast code 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 --rpc-url=$ETH_RPC_URL  获取一个合约的字节码
	
Block Commands:
	cast find-block 1609459200	获取最接近 2021 年新年的区块编号   后面是时间戳   1609459200=>Friday, January 1, 2021 12:00:00 AM
	cast gas-price  --rpc-url=$ETH_RPC_URL   获取当前 GAS 价格
	cast block-number  --rpc-url=$ETH_RPC_URL  获取最新的区块号
	cast basefee -B latest  --rpc-url=$ETH_RPC_URL     获取一个区块的基础费用
	cast basefee -B  16763316  --rpc-url=$ETH_RPC_URL  获取特定区块的基础费用
	cast block --full  latest --rpc-url=$ETH_RPC_URL   获取一个区块的信息
	cast block --full  16763316 --rpc-url=$ETH_RPC_URL
	cast age -B latest --rpc-url=$ETH_RPC_URL         获取一个区块的时间戳
	

Transaction Commands:
	cast receipt 0xcae38088dc5fb0a639e07776a336a496f6d9cc423845bab5615be19694cf7fd7  --rpc-url https://eth-mainnet.g.alchemy.com/v2/qXFbrurfVJ5Le9N3T3HwEi8Wc06v0Ud3 -j 
	export ETH_RPC_URL=https://eth-mainnet.g.alchemy.com/v2/qXFbrurfVJ5Le9N3T3HwEi8Wc06v0Ud3
	cast receipt 0xcae38088dc5fb0a639e07776a336a496f6d9cc423845bab5615be19694cf7fd7  --rpc-url $ETH_RPC_URL	 
	
	发送 ether 给其他账号 
	export RPC_URL=https://eth-goerli.g.alchemy.com/v2/ybl4NUkj7Q4_odQ1NWNL2ZyYCFcOMSd5
	export PRIVATE_KEY=a028b80f557f2ff8edd2b39fe749b21c10669d78381df8c6e2f2afc362efa1ee
	cast send --rpc-url=$RPC_URL  0x6f7F3E0Ff3bd4e6eCC50d2Ee60c38D28070116bD --value 0.001ether  --private-key=$PRIVATE_KEY 
	
	将私钥转换成账号地址
	cast wallet address  --private-key a028b80f557f2ff8edd2b39fe749b21c10669d78381df8c6e2f2afc362efa1ee
	
	调用已部署合约的方法  balanceOf(address) 
	contract: 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 
	holder: 0x80acF6c7Cd6075510E0FCd4F9986c77Cd60d6253
	balanceOf:  0.142338703066365711 ETH
	cast call 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 "balanceOf(address)(uint256)" 0xFAFc8F2621fCE45334d74934cCB0c942e4784631  --rpc-url $ETH_RPC_URL
	
	调用已部署合约的方法  totalSupply()
	$ cast call 0x6b175474e89094c44da98b954eedeac495271d0f "totalSupply()(uint256)" --rpc-url https://eth-mainnet.alchemyapi.io/v2/Lc7oIGYeL_QvInzI0Wiu_pOZZDEKBrdf
	8603853182003814300330472690	
	
	原始的 JSON-RPC 请求  获取最新的 eth_getBlockByNumber 
	什么是 RPC 规范 https://ethereum.org/zh/developers/docs/apis/json-rpc/
	JSON-RPC 是一种无状态的、轻量级远程过程调用 (RPC) 协议。 它定义了一些数据结构及其处理规则。 它与传输无关，因为这些概念可以在同一进程，通过接口、超文本传输协议或许多不同的消息传递环境中使用。
	有多少方法 查看 https://ethereum.github.io/execution-apis/api-documentation/  
	$ cast rpc --rpc-url=$RPC_URL eth_getBlockByNumber "latest" "false"   
	上面的 eth_getBlockByNumber 会返回 区块信息  baseFeePerGas gasLimit gasUsed hash logsBloom nonce number parentHash 
	State 方法
		eth_getBalance
		eth_getStorageAt
		eth_getTransactionCount
		eth_getCode
		eth_call
		eth_estimateGas
	History 方法
		eth_getBlockTransactionCountByHash
		eth_getBlockTransactionCountByNumber
		eth_getUncleCountByBlockHash
		eth_getUncleCountByBlockNumber
		eth_getBlockByHash
		eth_getBlockByNumber
		eth_getTransactionByHash
		eth_getTransactionByBlockHashAndIndex
		eth_getTransactionByBlockNumberAndIndex
		eth_getTransactionReceipt
		eth_getUncleByBlockHashAndIndex
		eth_getUncleByBlockNumberAndIndex
	
	获得有关交易的信息
		export ETH_RPC_URL=https://eth-mainnet.g.alchemy.com/v2/qXFbrurfVJ5Le9N3T3HwEi8Wc06v0Ud3
		cast tx 0x2b50dd7511762dcd04d9f17102154446434aef6a8f617fb254344d98e22d06c7 --rpc-url=$ETH_RPC_URL
		查看这笔交易的发起人 from   还有其他字段 blockHash gas gasPrice  hash input nonce r s to 
		cast tx 0x2b50dd7511762dcd04d9f17102154446434aef6a8f617fb254344d98e22d06c7 from --rpc-url=$ETH_RPC_URL
	
	在本地环境运行一个已发布的交易 打印出 trace 
		运行命令后 要等好久  300s 
		$ cast run 0x2b50dd7511762dcd04d9f17102154446434aef6a8f617fb254344d98e22d06c7
		Executing previous transactions from the block.
		Traces:
		  [13940] 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2::withdraw(340000000000000000) 
			├─ [0] 0x534C6834683Ad76017B531271cEF4986D3acF623::fallback{value: 340000000000000000}() 
			│   └─ ← ()
			├─ emit Withdrawal(param0: 0x534C6834683Ad76017B531271cEF4986D3acF623, param1: 340000000000000000)
			└─ ← ()

		Transaction successfully executed.
		Gas used: 35204		
	预估交易的gas
		cast estimate 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2   --value 0.1ether "deposit()"  --rpc-url=$ETH_RPC_URL
		cast estimate 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 "balanceOf(address)(uint256)" 0xFAFc8F2621fCE45334d74934cCB0c942e4784631  --rpc-url=$ETH_RPC_URL
Chain Commands:
	$ cast chain-id --rpc-url https://eth-goerli.g.alchemy.com/v2/ybl4NUkj7Q4_odQ1NWNL2ZyYCFcOMSd5
	5
	$ cast chain-id --rpc-url https://eth-mainnet.g.alchemy.com/v2/qXFbrurfVJ5Le9N3T3HwEi8Wc06v0Ud3
	1

	$ cast chain --rpc-url https://eth-mainnet.g.alchemy.com/v2/qXFbrurfVJ5Le9N3T3HwEi8Wc06v0Ud3
	ethlive
	$ cast chain --rpc-url https://eth-goerli.g.alchemy.com/v2/ybl4NUkj7Q4_odQ1NWNL2ZyYCFcOMSd5
	goerli

	$ cast client --rpc-url https://eth-goerli.g.alchemy.com/v2/ybl4NUkj7Q4_odQ1NWNL2ZyYCFcOMSd5
	Geth/v1.10.26-stable-e5eb32ac/linux-amd64/go1.18.8
	
```


```ssh
获取 链 id  名字 版本 
交易部分  发布 收据  签名并发布  发送  调用一个合约函数   预估gas费 
区块链  根据时间戳找到区块号码  当前gas价格，区块号，basefee，区块信息，时间戳 
账号    获取 余额 合约的storage slot值 nonce  合约的字节码
ENS     类似DNS  
获取 etherscan 上的源码 
ABI   解码/编码   function/calldata/event
转换  string-bytes32  binary-hex  integer-(fixed point number)   UTF8-hex   hex-ASCII   hex-bytes32  lowercase 
	  number-hex-encoded-int256    ether-gwei-wei  shl <<   shr >> 
工具  获取 function的selector   给event签名 	 keccak-256  create2生成的地址  给定abi返回interface  链接hex-string  i256最大/小值  
钱包  创建新的随机对  将私钥转换成账号地址  给信息签名  创建一个靓号  信息签名校验
```

## 命令大全
```ssh
General Commands
	cast help     Display help information about Cast.
	cast completions     Generate shell autocompletions for Cast.
Chain Commands
	cast chain-id     Get the Ethereum chain ID.
	cast chain     Get the symbolic name of the current chain.
	cast client     Get the current client version.
Transaction Commands
	cast publish     Publish a raw transaction to the network.
	cast receipt     Get the transaction receipt for a transaction.
	cast send     Sign and publish a transaction.
	cast call     Perform a call on an account without publishing a transaction.
	cast rpc      Perform a raw JSON-RPC request [aliases: rp]
	cast tx     Get information about a transaction.
	cast run     Runs a published transaction in a local environment and prints the trace.
	cast estimate     Estimate the gas cost of a transaction.
	cast access-list     Create an access list for a transaction.
Block Commands
	cast find-block     Get the block number closest to the provided timestamp.
	cast gas-price     Get the current gas price.
	cast block-number     Get the latest block number.
	cast basefee     Get the basefee of a block.
	cast block     Get information about a block.
	cast age     Get the timestamp of a block.
Account Commands
	cast balance     Get the balance of an account in wei.
	cast storage     Get the raw value of a contract's storage slot.
	cast proof     Generate a storage proof for a given storage slot.
	cast nonce     Get the nonce for an account.
	cast code     Get the bytecode of a contract.
ENS Commands
	cast lookup-address     Perform an ENS reverse lookup.
	cast resolve-name     Perform an ENS lookup.
	cast namehash     Calculate the ENS namehash of a name.
Etherscan Commands
	
ABI Commands
	cast abi-encode     ABI encode the given function arguments, excluding the selector.
	cast 4byte     Get the function signatures for the given selector from https://sig.eth.samczsun.com.
	cast 4byte-decode     Decode ABI-encoded calldata using https://sig.eth.samczsun.com.
	cast 4byte-event     Get the event signature for a given topic 0 from https://sig.eth.samczsun.com.
	cast calldata     ABI-encode a function with arguments.
	cast pretty-calldata     Pretty print calldata.
	cast --abi-decode     Decode ABI-encoded input or output data.
	cast --calldata-decode     Decode ABI-encoded input data.
	cast upload-signature     Upload the given signatures to https://sig.eth.samczsun.com.
Conversion Commands
	cast --format-bytes32-string     Formats a string into bytes32 encoding.
	cast --from-bin     Convert binary data into hex data.
	cast --from-fix     Convert a fixed point number into an integer.
	cast --from-utf8     Convert UTF8 to hex.
	cast --parse-bytes32-string     Parses a string from bytes32 encoding.
	cast --to-ascii     Convert hex data to an ASCII string.
	cast --to-base     Convert a number of one base to another.
	cast --to-bytes32     Right-pads hex data to 32 bytes.
	cast --to-fix     Convert an integer into a fixed point number.
	cast --to-hexdata     Normalize the input to lowercase, 0x-prefixed hex.
	cast --to-int256     Convert a number to a hex-encoded int256.
	cast --to-uint256     Convert a number to a hex-encoded uint256.
	cast --to-unit     Convert an ETH amount into another unit (ether, gwei, wei).
	cast --to-wei     Convert an ETH amount to wei.
	cast shl     Perform a left shifting operation.
	cast shr     Perform a right shifting operation.
Utility Commands
	cast sig     Get the selector for a function.
	cast sig-event     Generate event signatures from event string.
	cast keccak     Hash arbitrary data using keccak-256.
	cast compute-address     Compute the contract address from a given nonce and deployer address.
	cast create2      Generate a deterministic contract address using CREATE2
	cast interface     Generate a Solidity interface from a given ABI.
	cast index     Compute the storage slot for an entry in a mapping.
	cast --concat-hex     Concatenate hex strings.
	cast --max-int     Get the maximum i256 value.
	cast --min-int     Get the minimum i256 value.
	cast --max-uint     Get the maximum u256 value.
	cast --to-checksum-address     Convert an address to a checksummed format (EIP-55).
Wallet Commands
	cast wallet     Wallet management utilities.
	cast wallet new     Create a new random keypair.
	cast wallet address     Convert a private key to an address.
	cast wallet sign     Sign a message.
	cast wallet vanity     Generate a vanity address.
	cast wallet verify     Verify the signature of a message.
```