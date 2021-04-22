### 文件结构
```
|———data
    |———bytecode
    |———sourcecode
    |———results
        |———res_oyente
        |———res_mythril
|———comparation
    |———oyente
    |———mythril
    |———scripts
        |———runOyente.py
        |———runMythril.py
        
```
### 1. **oyente**
docker id: fa80fcb539b3     
docker内文件结构

待做：把evm_bytecode 和 scripts移到与原始oyente目录并列

### 2. **mythril**
```
$ myth a -f bytecodefile (-o json | tee outputfile)
```
### 3. ****