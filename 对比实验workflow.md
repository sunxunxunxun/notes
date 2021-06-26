## 经典代码分析工具
### 文件结构
```
|———data
    |———bytecode
    |———sourcecode
    |———results
        |———res_oyente
        |———res_mythril
        |———res_osiris
        |———res_vandal
        |———res_smartcheck
|———comparation
    |———oyente
    |———mythril
    |———smartcheck
    |———osiris
    |———scripts
        |———runOyente.py
        |———runMythril.py
        |———runSmartcheck.py
        ...
```
### 1. **oyente**
https://github.com/enzymefinance/oyente     
docker id: fa80fcb539b3     
docker内文件结构

### 2. **mythril**
https://github.com/ConsenSys/mythril
```
$ pip3 install mythril
$ myth a -f <bytecodefile> (-o json | tee <outputfile>)
```
### 3. ~~**securify**~~
securify1 需要java8，而且停用了；securify2不能分析bytecode，从etherscan链接会有connection问题，分析sol编译也是个问题（只支持>0.5.8.
没做了就

### ~~4. **MAIAN**~~
web3(>=3.5)的包KeepAliveRpcProvider, RpcProvider停用了，所以本地源码安装的跑不起。
~~待做：使用python虚拟环境解决上述问题~~ 
还是不行，3.5以下的web3包很多package不能用了。

### 5. **smartcheck**
https://github.com/smartdec/smartcheck      
npm 安装
```
npm install @smartdec/smartcheck -g
```
```
smartcheck -p <path of dir or file> 
```

### ~~6. **ZEUS**~~ 没开源

### ~~7. **Ethir**~~ 没有漏洞检测结果
https://github.com/costa-group/EthIR        
python2的环境
安装_solc0.4.25, evm_1.8.18, z3-solver_4.5.1.0, six
```shell
##在ethir文件夹下
$ python2 ethir.py -s xx.sol
$ python2 ethir.py -s xx.evm -b
```
生成几个分析文件到/tmp/costa/，但没有漏洞检测结果。

### 8. **Vandal**
https://github.com/usyd-blockchain/vandal
git clone 下来源码安装，[官方文档](https://github.com/usyd-blockchain/vandal/wiki/Getting-Started-with-Vandal)
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

### 9. **osiris**
https://github.com/christoftorres/Osiris
- docker
```shell
$ docker pull christoftorres/osiris && docker run -i -t christoftorres/osiris
$ python osiris/osiris.py -s sourcecode/xxx.sol [-j]  # 标准输出 or 输出json文件到results文件夹
$ python osiris/osiris.py -s bytecode/xxx -b [-j]
```
### 10. ~~**manticore**~~
https://github.com/trailofbits/manticore    
源码分析会出错，字节码分析是把字节码写死在代码里面的. ————后续再看要不要做

## deeplearning方法
是否需要做？

### 检测结果分析
1. 每个工具的检测结果形成一个txt文件    
完成
- mythril   
0x9dc971efd1774d442d2f945152ef917ba04a3e4e
0xadc5c669aa3abcbaf54d703c03625d6b3a30ec55#1#0#0#0#0#1#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0
- osiris    
0x5ab777e1e73477a9620af40809bb84779385a143#80.4#0#0#1#1#0#0#1#0#50.0347781181#0#0#0#72#0
- oyente    
0xc56efab08d635f8fe0831319fe88539c5bdc3490#18.5#1#0#0#0#0#0
- smartcheck    
0xf32c56d211179fbc6d95de7630ca0eed1c55f682#SOLIDITY_VISIBILITY :14#SOLIDITY_SAFEMATH :3#SOLIDITY_DEPRECATED_CONSTRUCTIONS :3#SOLIDITY_PRAGMAS_VERSION :1#SOLIDITY_EXTRA_GAS_IN_LOOPS :3#SOLIDITY_ADDRESS_HARDCODED :1#SOLIDITY_UPGRADE_TO_050 :6#SOLIDITY_GAS_LIMIT_IN_LOOPS :3
- vandal    
0xdd6433e9fa990c232f6a056db1bb41726300c8a7#1#0#0#1#0#0

解决**每个检测工具漏洞表示差异**的问题
2. 分类     
先拿到检测交集的合约addr, 再分类，最好用deeplearning
- common addr   
smartcheck用的sourcecode, 其它用的bytecode, 提取数据集的时候没有配套~~
已解决, 但最后有4743个, 存在commonAdrrs.txt
- 对common addrs分类    


3. 汇总每个合约的漏洞检测结果
4. 分析
