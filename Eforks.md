# 以太坊分叉改进

## [块#1,150,000](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-606.md)

> Homestead的硬分叉中包含的更改   
>
> - [EIP 2](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-2.md)（前沿硬分叉变化）
> - [EIP 7](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7.md)（DELEGATECALL）
> - [EIP 8](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-8.md)（网络层：devsp2p  Homestead的前向兼容性要求）

## [块#1,920,000](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-779.md)

> “DAO Fork”的硬叉中包含的更改。与其他硬叉不同，DAO Fork没有改变协议; 所有EVM操作码，事务格式，块结构等都保持不变。相反，DAO Fork是一种“不规则的状态变化”，它将以太币余额从一系列账户（“子DAO”合约）转移到指定账户（“withdrawDAO”合约）。  
>
> 在块1880000，以下帐户被编码到列表中`L`：
>
> - DAO（`0xbb9bc244d798123fde783fcc1c72d3bb8c189413`）
> - 它的extraBalance（`0x807640a13483f8ac783c557fcdf27be11ea4ac7a`）
> - DAO创建者的所有孩子（`0x4a574510c7014e4ae985403536074abe582adfc8`）
> - 和每个孩子的额外平衡
>
> 在块1920000开始时，所有帐户中的所有以太`L`将被转移到部署在的合同中`0xbf4ed7b27f1d666546e30d74d50d173d20bca754`。合同是根据以下Solidity代码（编译器版本`v0.3.5-2016-07-01-48238c9`）创建的：
>
> ```
> // Deployed on mainnet at 0xbf4ed7b27f1d666546e30d74d50d173d20bca754
> 
> contract DAO {
>     function balanceOf(address addr) returns (uint);
>     function transferFrom(address from, address to, uint balance) returns (bool);
>     uint public totalSupply;
> }
> 
> contract WithdrawDAO {
>     DAO constant public mainDAO = DAO(0xbb9bc244d798123fde783fcc1c72d3bb8c189413);
>     address public trustee = 0xda4a4626d3e16e094de3225a751aab7128e96526;
> 
>     function withdraw(){
>         uint balance = mainDAO.balanceOf(msg.sender);
> 
>         if (!mainDAO.transferFrom(msg.sender, this, balance) || !msg.sender.send(balance))
>             throw;
>     }
> 
>     function trusteeWithdraw() {
>         trustee.send((this.balance + mainDAO.balanceOf(this)) - mainDAO.totalSupply());
>     }
> }
> ```

## [块#2,463,000](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-608.md)

> Tangerine Whistle（EIP 150）的硬叉中包含的更改  
>
> - [EIP 150](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-150.md)（IO重型运营的天然气成本变化）

## [块#2,675,000](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-607.md)

> Spurious Dragon的硬叉中包含的更改  
>
> - [EIP 155](https://eips.ethereum.org/EIPS/eip-155)（简单的重放攻击保护）
> - [EIP 160](https://eips.ethereum.org/EIPS/eip-160)（EXP成本增加）
> - [EIP 161](https://eips.ethereum.org/EIPS/eip-161)（国家清除）
> - [EIP 170](https://eips.ethereum.org/EIPS/eip-170)（合同代码大小限制）

## [块#4,370,000](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-609.md)

> Byzantium的硬分叉中包含的更改    
>
> - [EIP 100](https://eips.ethereum.org/EIPS/eip-100)（将难度调整改为目标平均块时间，包括叔叔）
> - [EIP 140](https://eips.ethereum.org/EIPS/eip-140)（以太坊虚拟机中的REVERT指令）
> - [EIP 196](https://eips.ethereum.org/EIPS/eip-196)（椭圆曲线上的加法和标量乘法的预编译合同alt_bn128）
> - [EIP 197](https://eips.ethereum.org/EIPS/eip-197)（在椭圆曲线上进行最佳配对检查的预编译合同alt_bn128）
> - [EIP 198](https://eips.ethereum.org/EIPS/eip-198)（bigint模幂运算的预编译合同）
> - [EIP 211](https://eips.ethereum.org/EIPS/eip-211)（新操作码：RETURNDATASIZE和RETURNDATACOPY）
> - [EIP 214](https://eips.ethereum.org/EIPS/eip-214)（新操作码STATICCALL）
> - [EIP 649](https://eips.ethereum.org/EIPS/eip-649)（难度炸弹延迟和阻止奖励减少）
> - [EIP 658](https://eips.ethereum.org/EIPS/eip-658)（在收据中嵌入交易状态代码）

## [块#7,280,000](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1013.md) 

> 以太坊君士坦丁堡分叉的变化   
>
> - [EIP 145](https://eips.ethereum.org/EIPS/eip-145)：EVM中的按位移位指令
> - [EIP 1014](https://eips.ethereum.org/EIPS/eip-1014)：Skinny CREATE2
> - [EIP 1052](https://eips.ethereum.org/EIPS/eip-1052)：EXTCODEHASH操作码
> - [EIP 1234](https://eips.ethereum.org/EIPS/eip-1234)：延迟难度炸弹，调整块奖励
> - [EIP 1283](https://eips.ethereum.org/EIPS/eip-1283)：没有dirty maps的SSTORE的Gas计量

## [块#7,280,000](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1588.md)

> 替代Ethereum hardfork中包含的名为Ethereum ProgPoW的更改。
>
> - [EIP 1057](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1057.md)：ProgPoW，一种程序化的工作证明

## [待定](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1679.md)

> Istanbul的以太坊hardfork中包含的更改   