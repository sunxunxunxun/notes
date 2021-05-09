
## 经典代码分析工具
### 文件结构
```
|———data
    |———bytecode
    |———sourcecode
    |———results
        |———res_oyente
        |———res_mythril
        |———res_smartcheck
|———comparation
    |———oyente
    |———mythril
    |———smartcheck
    |———scripts
        |———runOyente.py
        |———runMythril.py
        |———runSmartcheck.py
        
```
### 1. **oyente**
docker id: fa80fcb539b3     
docker内文件结构

待做：把evm_bytecode 和 scripts移到与原始oyente目录并列

### 2. **mythril**
```
$ myth a -f bytecodefile (-o json | tee outputfile)
```
### 3. **securify**
securify1 需要java8，而且停用了；securify2不能分析bytecode，从etherscan链接会有connection问题，分析sol编译也是个问题（只支持>0.5.8.
没做了就

### ~~4. **MAIAN**~~
web3(>=3.5)的包KeepAliveRpcProvider, RpcProvider停用了，所以本地源码安装的跑不起。
~~待做：使用python虚拟环境解决上述问题~~ 
还是不行，3.5以下的web3包很多package不能用了。

### 5. **smartcheck**
npm 安装
```
smartcheck -p <path of dir or file> 
```

### ~~6. **ZEUS**~~ 没开源

### 7. **Ethir**
https://github.com/costa-group/EthIR


### 8. **Vandal**
https://github.com/usyd-blockchain/vandal
```
$ ../bin/analyze.sh ../examples/use_of_origin.hex ../datalog/demo_analyses.dl
```
use_of_origin.hex：字节码文件；demo_analyses.dl：自行定义的6种漏洞规则（可扩展
- reentrantCall
- originUsed
- checkedCallStateUpdate
- uncheckedCall
- unsecuredValueSend
- destroyable


## deeplearning方法
