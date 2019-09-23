# 以太坊改进建议摘选

## [#8@devp2p Homestead的前向兼容性要求!final](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-8.md)

> 此EIP为devp2p有线协议，RLPx发现协议和RLPx TCP传输协议的实现了引入新的前向兼容性要求。实施EIP-8的客户端根据Postel定律行事：
>
> > 保守你的所作所为，要接受别人的自由。
>
> 对devp2p协议的更改很难部署，因为如果hello（发现ping，RLPx握手）包的版本号或结构与本地期望不匹配，则运行旧版本的客户端将拒绝通信。
>
> 作为Homestead共识升级的一部分，引入前向兼容性要求，将确保在以太坊网络上使用的所有客户端软件可以应对未来的网络协议升级（只要保持向后兼容性）。

## [#55@混合大小写校验和地址编码!final](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-55.md)

> 在英语中，将地址转换为十六进制，但如果第i位是一个字母(即。如果小写十六进制地址的哈希值的第4*i位是1，则用大写，否则用小写。
>
> - 向后兼容许多接受混合大小写的十六进制解析器，允许它随着时间的推移轻松引入
> - 保持长度为40个字符
> - 平均每个地址将有15个校验位，如果输入错误，随机生成的地址将意外通过检查的净概率为0.0247％。这比ICAP提高了约50倍，但不如4字节检查代码好。

## [#137@以太坊域名服务规范!final](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-137.md)

> 本EIP草案描述了以太坊名称服务的细节，这是一种提议的协议和ABI定义，可以灵活地解析服务和资源标识符的简短，易读的名称。这允许用户和开发人员引用易读且易记的名称，并允许在底层资源（合约，内容寻址数据等）发生变化时根据需要更新这些名称。
>
> 域名的目标是提供稳定的，人类可读的标识符，可用于指定网络资源。通过这种方式，用户可以输入令人难忘的字符串，例如'vitalik.wallet'或' [www.mysite.swarm](http://www.mysite.swarm/)'，并被引导到适当的资源。名称和资源之间的映射可能会随着时间的推移而发生变化，因此用户可能会更改钱包，网站可能会更改主机，或者群集文档可能会更新为新版本，而域名不会更改。此外，域不需要指定单个资源; 不同的记录类型允许相同的域引用不同的资源。例如，浏览器可以通过获取其A（地址）记录将“mysite.swarm”解析为其服务器的IP地址，而邮件客户端可以通过获取其MX（邮件交换器）记录将相同的地址解析为邮件服务器。

## [#145@EVM中的按位移位指令!final](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-145.md)

> 提供本机按位移位，其成本与其他算术运算相同。引入了本机按位移位指令，这些指令在主机上处理更有效，并且合约使用更便宜。EVM缺少按位移位运算符，但支持其他逻辑和算术运算符。移位操作可以通过算术运算符实现，但是成本较高并且需要更多来自主机的处理时间。实施`SHL`（逻辑左移）和`SHR`（逻辑右移）使用算术成本每35个Gas，而建议的指令需要3个Gas。

## [#155@简单的重放攻击保护!final](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-155.md)

> 如果`block.number >= FORK_BLKNUM`和`v = CHAIN_ID * 2 + 35`或者`v = CHAIN_ID * 2 + 36`，然后在为了签名或恢复而计算事务的哈希时，而不是仅对前六个元素（即nonce，gasprice，startgas，to，value，data）进行哈希处理，哈希九个元素，`v`替换为`CHAIN_ID`，`r = 0`和`s = 0`。当前存在的签名方案使用`v = 27`并`v = 28`保持有效，并继续按照与现在相同的规则运行。

## [#158@状态清理!Replaced](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-158.md)

> 对于所有块`block.number >= FORK_BLKNUM`（TBA）：
>
> 1. 在所有对帐户进行状态更改的情况下，如果该状态更改导致帐户状态被保存为nonce = 0、balance = 0、code empty、storage empty(以下简称“empty account”)，则删除该帐户。
> 2. 如果一个地址被“触摸”，并且该地址包含一个空帐户，那么该地址将被删除。“touch”定义为如果给定地址上的帐户不存在，将创建该帐户的任何情况。
> 3. 每当EVM检查帐户是否存在时，空被视为等同于不存在。特别需要注意的是，这意味着，一旦启用了此更改，从EVM执行的角度来看，空性和不存在性之间就不再存在有意义的区别。
> 4. 在任何情况下，零值调用和零值自销毁不再消耗25000个帐户创建Gas成本
>
> 发生“触摸”的情况可以列举如下：
>
> - 零价值CALL
> - CREATE（如果最终保存的代码为空，并且保存时帐户中没有剩余的以太）
> - 零价值SUICIDE
> - 交易收件人
> - 合约创建交易中创建的合约
> - 矿工收取交易费用（注意gasprice为零的情况，并且该帐户尚不存在，因为它只*在*处理每笔交易*后*收到冻结/叔叔/侄子奖励）

## [#162@ENS Hash 注册机初始化!final](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-162.md)

> 该ERC描述了部署在2017-05-04主要以太坊网络上的注册商合约的实施，该合约用于管理以太坊名称服务（ENS）中的名称分配。相应的源代码在[这里](https://github.com/ethereum/ens/blob/mainnet/contracts/HashRegistrarSimplified.sol)。
>
> 有关更多背景信息，请参阅[EIP 137](https://github.com/ethereum/EIPs/issues/137)。
>
> > 注册商负责为系统用户分配域名，并且是唯一能够更新ENS的实体; ENS注册中心节点的所有者是其注册商。注册商可能是合约或外部拥有的账户，但预计根和最高级注册商至少将作为合约实施。
> >
> > \- EIP 137
>
> 精心设计和管理的注册商对EIP 137中描述的ENS的成功至关重要，但在本文档中单独描述，因为它是核心ENS协议的外部。
>
> 为了最大化新命名空间的实用性和采用率，注册商应该减少推测和“命名蹲”，但是最好的缓解方法还不清楚。因此，提出了“初始”注册器，其实现了名称分配的简单方法。在初始阶段，可用命名空间将明显限制在`.eth`顶级域中，并且不允许长度小于7个字符的子域。该规范主要描述了@alexvandesande和@ arachnid的[散列注册器实现](https://github.com/ethereum/ens/blob/mainnet/contracts/HashRegistrarSimplified.sol)，以便于讨论。
>
> 目的是用永久注册商合约取代初始注册商合约。常任书记官长将增加可用的名称空间，并纳入从初始书记官长的表现中汲取的经验教训。此升级预计将在初始部署的大约2年内进行。
>
> 应考虑以下因素，以优化ENS的采用，以及初始注册商名称空间的良好治理。
>
> **可升级性：**初始注册商应该可以安全地升级，以便在部署期间获得的知识可以用来代替改进的永久注册商。
>
> **有效分配：**新发布的名称空间通常会产生抢占状态，导致许多具有潜在价值的名称被购买但尚未使用，希望以盈利方式进行转售。这会降低最有用名称的可用性，从而降低名称服务对最终用户的效用。
>
> 实现有效分配可能需要也可能不需要人为干预以解决争议和其他形式的策展。初始注册服务商的目标不应是创建最有效的可能分配，而是限制长期错配的成本。
>
> **安全性：**注册商将保持以太的余额而没有明确的限制。它必须安全设计。
>
> **简单性：** ENS规范本身强调关注点的分离，允许最基本的元素，注册表尽可能简单。临时登记员应该尽可能简单，同时仍然符合其他设计目标。
>
> **采用：**由于网络效应，成功的标准变得更加成功。注册商应考虑哪些策略将鼓励采用ENS，尤其是它所控制的命名空间。

## [#165@标准接口检测!final](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-165.md)

> 创建标准方法以发布和检测智能合约实现的接口,对于某些“标准接口”，如[ERC-20token接口](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md)，有时查询合约是否支持接口，如果是，接口的哪个版本，以便调整合约的交互方式用。特别是对于ERC-20，已经提出了版本标识符。该提议标准化了接口的概念，并标准化了接口的标识（命名）。

## [#190以太坊智能合约打包规范!final](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-190.md)

> 该ERC提出了以太坊智能合约包的规范。
>
> 打包是现代软件开发的核心部分，在以太坊生态系统中缺乏包管理限制了开发人员重用代码的能力，这会对生产力和安全性产生负面影响。
>
> 一个关键的例子是ERC20标准。有一些经过良好审计的token合约是可复用的，但是由于难以找到和重用现有代码，大多数开发人员最终编写了自己的token合约
>
> 打包标准应对生态系统产生以下积极影响：
>
> - 复用现有代码的能力导致整体生产力提高。
> - 通过复用现有的经过良好审计的常见模式（ERC20，众筹等）也可以提高了安全性。
>
> 智能合约打包也应对最终用户产生直接的积极影响。电子钱包软件将能够使用已发布的软件包并生成用于与该软件包中包含的任何已部署合约进行交互的界面。随着[ENS](https://github.com/ethereum/EIPs/issues/137)的出现，所有部件都将用于钱包以获取人类可读的名称并向用户呈现用于与底层应用程序交互的界面。

## [#191@签署数据标准!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-191.md) *

> 该ERC提出了关于如何处理以太坊合约中的签名数据的规范
>
> 已经创建了几个多签名钱包实现，它们接受预签名的事务。预签名事务是一个带有签名的二进制signed_data块(r、s、v)，没有指定对signed_data的解释，导致了以下几个问题:：
>
> - 标准以太坊交易可以提交为`signed_data`。可以将以太坊交易解压缩到以下组件中:( `RLP<nonce, gasPrice, startGas, to, value, data>`特此称为`RLPdata`）`r`，`s`和`v`。如果没有语法约束`signed_data`，这意味着`RLPdata`可以用作语法上有效的`预签名`事务。
> - 多重签名钱包也存在`预签名`交易未与`validator`特定钱包相关联的问题，即特定钱包。例：
>   1. 用户`A`，`B`和`C`拥有`2/3`-wallet`X`
>   2. 用户`A`，`B`和`D`拥有`2/3`-wallet`Y`
>   3. 用户`A`和`B` 提交 `presigned`交易`X`。
>   4. 攻击者现在可以重复使用他们的预先签署的交易`X`，并提交给`Y`。

## [#225@集中的权力证明一致协议!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-225.md)

> Clique是一种权威证明共识协议。它会影响以太坊主网的设计，因此可以轻松地添加到任何客户端。
>
> 以太坊的第一个官方测试网络是Morden。它从2015年7月到2016年11月左右，由于Geth和Parity之间累积的垃圾和一些测试网络共识问题，它最终被搁置以支持testnet重启。
>
> 因此，Ropsten出生了，清除了所有的垃圾，从一个干净的石板开始。这种情况一直持续到2017年2月底，当时恶意行为者决定滥用低PoW，并逐渐将封闭气体限制扩大到90亿（从正常的470万），此时发送巨大的交易使整个网络陷入瘫痪。甚至在此之前，攻击者就尝试了多次极长的重组，导致不同客户端甚至不同版本之间的网络分裂。
>
> 这些攻击的根本原因是PoW网络仅与其背后的计算能力一样安全。从零开始重新启动一个新的测试网络无法解决任何问题，因为攻击者可以一遍又一遍地安装相同的攻击。Parity团队决定采用紧急解决方案来回滚大量的区块，并制定了一个不允许气体限制超过某个阈值的软分叉规则。
>
> 虽然这个解决方案可能在短期内有效：
>
> - 它不优雅：以太坊应该有动态块限制
> - 它不可移植：其他客户端需要自己实现新的fork逻辑
> - 它与同步模式不兼容：快速和轻便的客户都不走运
> - 它只会延长攻击时间：垃圾仍然可以无限制地推进
>
> Parity的解决方案尽管不完美，但仍然可行。我想提出一个更长期的替代解决方案，它更复杂，但应该足够简单，以便在合理的时间内推出。

## [#601@确定性钱包的以太坊层次结构!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-601.md)

> 此EIP定义了基于确定性钱包逻辑层次结构[BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)，在限定的目的方案[BIP43](https://github.com/bitcoin/bips/blob/master/bip-0043.mediawiki)和EIP-草案-复仇用途。
>
> 该EIP是eip-draft-ethereum目的的特定应用。
>
> 目前，不同的以太坊客户和钱包使用不同的派生路径; 可在[此处](https://github.com/ethereum/EIPs/issues/84#issuecomment-292324521)找到它们的摘要。其中一些路径违反了BIP44，这是标准定义派生路径的开头`m/44'/`。这会在钱包实施之间产生混淆和不兼容，在某些情况下，从一个钱包中获取资金在另一个钱包上不可访问，而在其他情况下需要手动提示用户获取推导路径，这会妨碍可用性。
>
> 此外，BIP44在设计时考虑了基于UTXO的区块链，并且不适合使用帐户抽象的以太坊。
>
> 作为替代方案，我们提出了一个更确定的钱包层次结构，更好地适应以太坊的独特需求。

## [#615@EVM的子程序和静态跳转!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-615.md) *

> 在21世纪，在区块链上传播数十亿的ETH，正式的规范和验证是防止损失的重要工具。然而，EVM的设计使得这不必要地变得困难。此外，EVM的设计使得低Gas成本，高性能执行变得困难。我们建议通过收紧安全保障和推动EVM的性能限制来推进解决这些问题的建议。
>
> 目前，EVM支持动态跳转，其中跳转到的地址是堆栈中的参数。这些动态跳跃模糊了代码的结构，因此主要抑制了控制和数据流分析。这使得优化编译的质量和速度从根本上说是不一致的。此外，由于许多跳转可能是代码中的任何跳转目的地，因此通过代码的可能路径的数量可以作为跳转次数与目的地数量的乘积，静态分析的时间复杂度也是如此。其中许多情况在部署时是不可判定的，进一步抑制了静态和正式分析。
>
> 缺少动态跳转代码可以在线性时间内静态分析。静态分析包括验证，以及大部分优化，编译和形式分析; 当线性时间分析可用时，工具链的每个部分都会受益。并且没有动态跳转，并且通过适当的子程序，EVM是从其他语言生成代码的更好目标，包括
>
> - Solidity
> - Vyper
> - LLVM IR
>   - 前端包括C，C ++，Common Lisp，D，Fortran，Haskell，Java，Javascript，Kotlin，Lua，Objective-C，Pony，Pure，Python，Ruby，Rust，Scala，Scheme和Swift
>
> 结果是，以下所有验证和优化都可以在部署时执行，其时间复杂度为线性(n)或近似线性(n log n)
>
> - 缺乏最特殊的停止状态可以得到验证。
> - 有时可以计算资源的最大使用量。
> - 字节码可以编译为机器代码。
> - 编译可以优化较小寄存器的使用。
> - 编译可以优化燃气计量的注入。
>
> 请注意，近线性`(n log n)`时间复杂度至关重要。否则，特制合约可用作针对任何验证和优化的攻击向量。

## [#616@支持EVM的SIMD(单指令多数据流)操作!Deferred](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-616.md)

> 提议为以太坊虚拟机提供单指令多数据类型和操作，充分利用256位宽EVM堆栈项，并为向量和标量操作提供显着的性能提升。
>
> 大多数现代CPU都包括在宽数据寄存器上运行的SIMD硬件，并行地将单指令应用于多个数据通道，其中通道将寄存器划分为相同大小的标量元素向量。该模型非常适合EVM的宽堆栈项目，为可以表示为标量向量的并行操作的操作提供显着的性能提升。对于一些示例，简短的文献检索发现SIMD加速
>
> - [SHA-512](http://keccak.noekeon.org/sw_performance.html)最高可达7X
> - 4X用于[椭圆曲线标量乘法](https://link.springer.com/chapter/10.1007/3-540-45439-X_16)
> - [BLAKE2b](https://github.com/minio/blake2b-simd)为3X至4X
> - [OpenSSL](https://software.intel.com/en-us/articles/improving-openssl-performance)最高可达3倍
> - [椭圆曲线模乘的](http://ieee-hpec.org/2013/index_htm_files/24-Simd-acceleration-Pabbuleti-2886999.pdf) 2X至3X
> - [SHA-256](https://github.com/minio/sha256-simd)为1.7X至1.9X
> - 1.3X用于[RSA加密](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.738.1218&rep=rep1&type=pdf)

## [#663@无限SWAP和DUP指令!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-663.md)

> `SWAP`并且`DUP`指令限制为堆栈深度为16.引入两条新指令，`SWAPn`并`DUPn`解除此限制并允许访问堆栈达到1024个项目的全部深度
>
> 在EVM之上实现更高级别的构造（例如函数）将导致输入和输出参数列表以及要返回的指令偏移量。
>
> 这些参数（或堆栈项）的数量很容易超过16，因此需要编译器额外注意，以便所有这些参数仍可访问。
>
> 介绍`SWAPn`并将`DUPn`为编译器提供一个选项，以便以可能增加的燃气成本为代价简化深度堆栈项目的访问。

## [#665@为Ed25519签名验证添加预编译合约!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-665.md) ？

> 通过向EVM添加Ed25519签名验证的预编译合约，支持在智能合约中对Ed25519加密签名进行高性能和低成本验证。

## [#698@OPCODE 0x46 BLOCKREWARD!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-698.md)

> 此EIP为EVM添加了一个额外的操作码，它将返回最终的块奖励值。
>
> 根据EIP-649（＃669），路线图中现在计划了周期性块奖励减少，但是，此EIP与共识系统无关，并且对去中心化的矿池操作以及任何通过挖块奖励支付而受益的合约最有价值（即合并采矿代币）

## [#712@以太坊类型的结构化数据哈希和签名!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-712.md) *

> 如果我们只关心字节串，那么签名数据就是一个解决了的问题。不幸的是，在现实世界中，我们关心的是复杂而有意义的信息。哈希结构化数据是非平凡的，错误会导致系统安全属性的丢失。
>
>
> 因此，有句谚语“不要使用自己的密码”。相反，需要使用经过同行评审和充分测试的标准方法。这个EIP的目标就是成为这个标准。
>
> 该EIP旨在提高离线消息签名在链上使用的可用性。我们看到越来越多的人采用脱机消息签名，因为它节省了算力，减少了区块链上的事务数量。当前已签名的消息是一个不透明的十六进制字符串，显示给用户，几乎没有关于组成消息的项的上下文。

## [#725@代理身份!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-725.md) *

> 密钥管理和执行的代理合约，用于建立区块链身份。
>
> 这个标准化的身份接口将允许Dapps、智能合约和第三方通过功能XXX中描述的两个步骤来检查一个人、组织、对象或机器的有效性。在这种情况下，信托被转交给债权的发行者。
>
> 验证身份的最重要的功能是： `XXX`
>
> 管理身份的最重要的功能是： `XXX`

## [#777 @新的更高级的代币标准!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-777.md) *

> 此EIP定义了token合约的标准接口和行为。
>
> 本标准试图改进广泛使用的[ERC20 ](https://eips.ethereum.org/EIPS/eip-20)token标准。该标准的主要优点是：
>
> 1. 使用与以太相同的理念，即token发送`send(dest, value, data)`。
> 2. 合约和常规地址都可以通过注册`tokensToSend`挂钩来控制和拒绝它们发送的token。（拒绝是通过`revert`钩子函数完成的。）
> 3. 合约和常规地址都可以通过注册`tokensReceived`钩子来控制和拒绝他们收到的token。（拒绝是通过`revert`钩子函数完成的。）
> 4. 该`tokensReceived`挂钩允许token发送到合约，并在一个单一的交易通知了，不像[ERC20](https://eips.ethereum.org/EIPS/eip-20)需要两个人通话（`approve`/ `transferFrom`）来实现这一目标。
> 5. token持有者可以“授权”和“撤销”可以代表他们发送token的运营商。这些运营商旨在成为经过验证的合约，例如交换机，支票处理器或自动收费系统。
> 6. 每个token事务包含`data`和`operatorData`字节字段，分别用于自由地从token持有者和操作员传递数据。
> 7. 它`tokensReceived`通过部署实现`tokensReceived`钱包挂钩的代理协议向后兼容不包含挂钩功能的钱包。

## [#902@token验证!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-902.md) *

> 用于提供token所有权和转移验证的服务的协议。
>
> 资产的代币化具有广泛的应用，尤其是证券和证券代币等金融工具。大多数司法管辖区对可交易的东西设置了法律限制，谁可以持有被视为证券的代币。从广义上讲，这包括KYC和AML验证，但也可能包括基于时间的支出限额，交易总量等。
>
> 监管机构和受制裁的第三方合规机构需要某种方式将链外合规信息（如身份和居住权）与链式服务联系起来。此设计的应用范围比法律规定更广泛，包括用于创建，管理和交易token的所有方式的业务逻辑权限。
>
> 不是每个token保持其自己的白名单（或其他机制），而是优选地共享链上资源，规则，列表等。还希望聚合跨多个验证器的数据和规则，或者应用复杂的行为（例如，切换逻辑，门，状态机）以将分布式数据应用于应用程序。

## [#908@奖励客户建立可持续发展的网络!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-908.md)

> 虽然Casper验证器受到激励以验证交易，但仍然没有激励中继块和存储数据（包括状态）。本文更多的是高层次的分析和讨论，而不是试图提供具体的解决方案。[Pocket Network](https://www.pokt.network/)是一个独立的区块链，设计于2018年9月，用于激励中继交易，旨在与其他区块链兼容。另请注意，[Rocket Pool](https://github.com/rocket-pool/rocketpool)正在开发中，并计划成为Casper的池，这将有助于激励运行完整节点。另一种选择是[VIPnode](https://github.com/vipnode/vipnode.org)它为轻客户收取服务他们的完整节点的费用。鉴于正在开发的这些解决方案，通常奖励客户的一种更合适的方法可能是激励带宽（中继和下载），存储和I / O（而计算已经被矿工用气体激励，并且将用于分片下的提议者）和卡斯帕）。另请注意，将激励公证人在分片下下载排序规则。过时（Casper FFG将使用带有分片的[以太](https://notes.ethereum.org/SCIg8AH5SA-O4C1G1LYZHQ#)坊2.0实现：[shasper](https://notes.ethereum.org/SCIg8AH5SA-O4C1G1LYZHQ#)）：鉴于Casper FFG看起来很快就会实施，为了尽量减少协议的过度复杂性，同时激励验证可能被认为是不值得的。对于包含奖励完整节点的提案的先前版本的提案，请参阅[此处](https://github.com/ethereum/EIPs/commit/97e235d0ba4a88b4ce29834aa2b94107b8d91e12#diff-9a43a8739b5a9e1dec427324cb264921)。

## [#926@地址元数据的注册表!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-926.md)

> 此EIP指定地址元数据的注册表，允许合约和外部帐户向onchain和offchain调用者提供有关他们自己的元数据。这允许使用案例，例如通用授权，提供token接受设置和声明注册表。
>
> 越来越多的用例需要存储与地址相关联的元数据; 例如，参见EIP 777和EIP 780，以及EIP 181中的ENS反向注册表。目前，每个用例都定义了自己的专用注册表。为了防止特殊用途注册管理机构合约的扩散，我们提出了一个使用可扩展架构的单一标准化注册机构，该架构允许未来的标准实施自己的元数据标准。

## [#927@授权!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-927.md) *

> 此EIP指定了一种通用授权机制，可用于实现各种授权模式，替换ERC20中的批准，ERC777中的操作员以及各种其他类型合约中的定制授权模式。
>
> 智能合约通常需要提供允许第三方呼叫者代表用户执行操作的界面。最常见的例子是token授权/运营商，但整个生态系统中存在其他类似情况，包括例如授权ENS域上的操作。通常，每个标准都会为自己重新发明这个系统，从而导致相同基本模式的大量不兼容的实现。在这里，我们提出了一种可用于所有此类合约的通用方法。
>
> 这里实现的模式受到[ds-auth](https://github.com/dapphub/ds-auth)和OAuth的启发。

## [#969@修改ethash使现有的专用硬件实现失效!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-969.md)

> 该EIP修改了ethash，以打破专门针对当前ethash挖掘算法的ASIC矿工。
>
> 基于ASIC的矿工将比基于GPU的矿工具有更低的运营成本，这将导致基于GPU的采矿迅速变得无利可图。鉴于基于ASIC的矿工的生产具有很高的进入壁垒和很少的市场参与者，这将导致采矿集权的集中化趋势。
>
> 风险包括单一制造商的市场支配地位，可能利用生产库存来挖掘自己，在其硬件中引入后门，或促进51％的攻击，否则这些攻击是不可行的。
>
> 这种集中化趋势对网络安全产生了负面影响，将网络的重要控制权交给了少数实体。
>
> Ethash仍然具有ASIC抗性，但ASIC制造商的技术正在不断发展，并且可能需要进一步改进以保持对不可预见的设计技术的抵抗力。该EIP明确寻求购买新开发的ASIC技术将面临障碍的时间，同时可以探索更多长期机制以确保持续的ASIC抗性。

## [#1014@Skinny CREATE2!final](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1014.md)  

> 在0xf5添加一个新的opcode，它需要4个堆栈参数：endowment，memory_start，memory_length，salt。除了使用`keccak256( 0xff ++ address ++ salt ++ keccak256(init_code))[12:]`而不是通常的sender-and-nonce-hash作为初始化合约的地址之外，其他行为与CREATE相同。
>
> CREATE2具有与CREATE相同的gas模式，但也有GSHA3WORD * ceil(len(init_code) / 32)的额外hashcost，用于解释必须执行的散列。在计算结果地址和执行init_code之前，hashcost与内存扩展Gas和CreateGas同时被扣除。
>
> * 0xff是一个单字节
>
> * 地址总是20字节
>
> * salt总是32字节(堆栈项)
>
> 因此，最后一轮哈希的预映像总是恰好85字节长。允许使用尚未在链上存在的地址(实际上或在通道中)进行交互，但是可以依赖这些地址，最终只可能包含由特定init代码段创建的代码。对于涉及与契约的反事实交互的状态通道用例非常重要。
>

## [#1015@可配置的链上发布!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1015.md) 

> 此EIP更改块奖励步骤，而不是将其设置为在客户端上进行硬编码并将其提供给矿工/验证器etherbase，而应该转到由链上有限制的合约地址来决定其发布（六个月内锁定;发行只能减少或被保留，但不会增加;）。建议采用一种决策方法，但这并不是本EIP概念的必要条件。这**不是一个通用的治理解决方案**，这是一个更广泛和更难的主题，**不会**影响技术升级决策或其他硬分叉

## [#1052@EXTCODEHASH 操作码!final](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1052.md) 

> 此EIP指定一个新的操作码，它返回合约代码的keccak256哈希值。
>
> 许多合约需要对合约的字节码执行检查，但不一定需要是字节码本身。例如，合约可能想要检查另一个合约的字节码是否是一组允许的操作，如果通过，它可以对代码执行分析并将任何具有匹配字节码的合约列入白名单。
>
> 目前的合约可以使用`EXTCODECOPY`操作码执行此操作，但这样的成本很高，特别是对于大型合约在只需要校验哈希的情况下。因此，我们提出了一个新的操作码`EXTCODEHASH`，它返回合约字节码的keccak256哈希值。

## [#1057@ProgPoW，一种程序化的工作量证明!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1057.md)

> 一种新的Proof-of-Work算法（ProgPow）取代了Ethash，它几乎可以利用商品GPU的所有部分。ProgPoW是一种工作量证明算法，旨在缩小专用ASIC可用的效率差距。它几乎利用了商用硬件（GPU）的所有部分，并针对以太坊网络中使用的最常见硬件进行了预调谐。

## [#1066@Status Codes!lastcall](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1066.md) *

> 此EIP提议了一种广泛适用的智能合约Status codes。该标准概述了一组与HTTP状态类似的通用状态代码。这提供了一组共享信号，以允许智能合约自动响应情况，向用户公开本地化错误消息等。
>
> 目前的技术水平要么除非已经明显的执行成功了，否则不回复（即：需要人为干预），要么返回很少有上下文的`true`或者`false`。Status codes与revert-with-reason类似，但是更加针对自动化的调试和最终用户反馈，它们完全兼容revert和revert-with-reason。
>
> 与HTTP的情况一样，拥有一组标准的已知codes对开发人员有很多好处。它们消除了需要为每个合约开发自己的方案的摩擦，使合约间自动化的调用执行更容易，并且更容易广泛地了解您的请求产生的有限状态。重要的是，它可以更容易区分预期的错误状态，需要暂停执行的真正异常条件，正常状态转换和各种成功案例。

## [#1081@标准化赏金!draft ](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1081.md)

> 为了鼓励Ethereum上奖金的跨平台互操作性，并为了更容易地跟踪声誉，标准化赏金可以以公开可审计和不可变的方式，促进基金管理，以换取与已完成任务相对应的可交付成果。
>
> 在Ethereum上缺少奖金标准的情况下，平台将很难协作并共享用户创建的奖金(从而重新创建Web2.0任务外包平台上现有的围墙花园)。对这些跨任务类型的交互进行标准化，还可以更容易地跟踪各种声誉指标(例如，您为完成的提交支付的频率，或者您的工作被接受的频率)。

## [#1123@修订了以太坊智能合约的打包标准!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1123.md)

> 描述智能合约软件包的数据格式。该EIP定义*包清单*文档的数据格式，表示一个或多个智能合约的包，可选地包括源代码和跨多个网络的任何/所有部署的实例。包清单是缩小的JSON对象，通过内容可寻址存储网络（如IPFS）进行分发。
>
> * 概括存储URI，以表示任何内容可寻址URI方案，而不仅仅是IPFS。
>
> * 重命名释放锁定文件到包清单。
>
> * 通过对编译器信息格式进行泛型，增加了对除可靠性之外的其他语言的支持。
>
> * 重新定义链接引用，使其更加灵活，以更直接的方式表示字节码中的任意间隙(除了地址)。
>
> * 强制格式严格，要求包清单不包含多余的空格，并按字母顺序对对象键排序，以防止散列不匹配。

## [#1155@ERC-1155多token标准!lastcall ](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md)

> 该标准概述了一个智能合约界面，可以代表任意数量的伪造和非伪造的token类型。诸如ERC-20的现有标准要求每个token类型部署单独的合约。ERC-721标准的tokenID是单个不可替代的索引，这些不可替换的组合作为单个合约进行部署，并设置整个集合。相比之下，ERC-1155多token标准允许每个tokenID表示新的可配置token类型，其可具有其自己的元数据，供应和其他属性。
>
> 该`_id`参数包含在每个函数的参数中，并指示事务中的特定标记或标记类型。

## [#1167@最小代理合约!lastcall](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1167.md)

> 通过标准化一个已知最小的字节码重定向的实现,本标准允许用户和第三方工具(例如Etherscan) (a)简单地发现合约，并总是以一个已知的方式重定向和(b)通过代码的方式，在重定向地址查询字节码，以确定将要运行的代码的位置——并且可以依赖于关于该代码的表示(经过验证的源代码、第三方审计等)。此实现将所有调用和100%的gas转发到实现合约，然后将返回值传递回调用者。在实现恢复的情况下，将把恢复连同负载数据一起传递回去(用于消息恢复)。

## [#1193@以太坊提供JavaScript API!draft ](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1193.md)

> 此EIP正式化了以太坊提供程序JavaScript API，以实现客户端和应用程序之间的一致性。该提供程序旨在最小化，旨在提供`window.ethereum`跨环境兼容性。
>
> ```
> const ethereum = window.ethereum;
> 
> // A) Set provider in web3.js
> var web3 = new Web3(ethereum);
> 
> 
> // B) Use provider object directly
> // Example 1: Log last block
> ethereum
>   .send('eth_getBlockByNumber', ['latest', 'true'])
>   .then(block => {
>     console.log(`Block ${block.number}:`, block);
>   })
>   .catch(error => {
>     console.error(
>       `Error fetching last block: ${error.message}.
>        Code: ${error.code}. Data: ${error.data}`
>     );
>   });
> 
> 
> // Example 2: Request accounts
> ethereum
>   .send('eth_requestAccounts')
>   .then(accounts => {
>     if (accounts.length > 0) {
>       console.log(`Accounts enabled:\n${accounts.join('\n')}`);
>     } else {
>       console.error(`No accounts enabled.`);
>     }
>   })
>   .catch(error => {
>     console.error(
>       `Error requesting accounts: ${error.message}.
>       Code: ${error.code}. Data: ${error.data}`
>     );
>   });
> 
> 
> // Example 3: Log available accounts
> ethereum
>   .send('eth_accounts')
>   .then(accounts => {
>     console.log(`Accounts:\n${accounts.join('\n')}`);
>   })
>   .catch(error => {
>     console.error(
>       `Error fetching accounts: ${error.message}.
>     Code: ${error.code}. Data: ${error.data}`
>     );
>   });
> 
> 
> // Example 4: Log new blocks
> let subId;
> ethereum
>   .send('eth_subscribe', ['newHeads'])
>   .then(subscriptionId => {
>     subId = subscriptionId;
>     ethereum.on('eth_subscription', result => {
>       if (result.subscription === subscriptionId) {
>         if (result.result instanceof Error) {
>           const error = result.result;
>           console.error(
>             `Error from newHeads subscription: ${error.message}.
>              Code: ${error.code}. Data: ${error.data}`
>           );
>         } else {
>           const block = result.result;
>           console.log(`New block ${block.number}:`, block);
>         }
>       }
>     });
>   })
>   .catch(error => {
>     console.error(
>       `Error making newHeads subscription: ${error.message}.
>        Code: ${error.code}. Data: ${error.data}`
>     );
>   });
>   
> // to unsubscribe
> ethereum
>   .send('eth_unsubscribe', [subId])
>   .then(result => {
>     console.log(`Unsubscribed newHeads subscription ${subId}`);
>   })
>   .catch(error => {
>     console.error(
>       `Error unsubscribing newHeads subscription: ${error.message}.
>        Code: ${error.code}. Data: ${error.data}`
>     );
>   });
> 
> 
> // Example 5: Log when accounts change
> const logAccounts = accounts => {
>   console.log(`Accounts:\n${accounts.join('\n')}`);
> };
> ethereum.on('accountsChanged', logAccounts);
> // to unsubscribe
> ethereum.removeListener('accountsChanged', logAccounts);
> 
> // Example 6: Log if connection ends
> ethereum.on('close', (code, reason) => {
>   console.log(`Ethereum provider connection closed: ${reason}. Code: ${code}`);
> });
> ```

## [#1261@会员资格验证token（MVT）!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1261.md)

> 以下标准允许在智能合约（称为实体）内实现会员验证token的标准API。该标准提供了跟踪某些链上“组织”中个人成员资格的基本功能。这允许几个用例，如自动化合规性，以及几种形式的治理和成员结构。
>
> 我们认为MVT的使用案例被分配给个人，这些个人不可转让并且可由业主撤销。MVT可以代表承认证明，成员证明，投票权证明以及区块链上的一些其他抽象概念。以下是这些用例的一些示例，可以提出其他几个用例：
>
> - 投票：投票本质上应该是一种允许的活动。到目前为止，onchain投票系统只能通过基于硬币平衡的民意调查进行投票。现在可以改变并采取各种形状和形式。
> - 护照签发，社会福利分配，旅行证发放，驾驶执照签发都是可以抽象为会员资格的申请，即属于个人的小型集合，被某些权威机构认定为具有某些权利，不需要任何个人特定信息（福利权，行动自由权，经营车辆的授权，移民）
> - 投资者许可：使监管合规成为一个简单的连锁过程。证券的特征化，简化为仅流向认可的地址，追踪和证明链地址用于反洗钱的目的。
> - 软件许可：游戏开发商等软件公司可以使用该协议授权某些硬件单元（控制台）下载和使用特定软件（游戏）
>
> 一般而言，个人在日常生活中可以拥有不同的会员资格。该协议允许创建将所有内容放在一个地方的软件。他的身份可以立即验证。想象一个世界，你不需要携带充满身份证的钱包（护照，健身房会员，SSN，公司ID等），组织可以轻松跟踪其所有成员。组织可以轻松识别和禁止虚假身份。
>
> 属性是ERC-1261的重要组成部分，它有助于存储有关其成员的可识别信息。民意调查可以利用属性来计算选民群。例如：用户应该属于美国实体而不属于华盛顿州属性作为民意调查的一部分。
>
> 将存在将属性标头映射到所有可能属性的数组的映射表。这样做是为了将实体细分为排他性和详尽的子组。例如，标题：血型字母表数组：[o，a，b，ab]标题：血型组符号数组：[+， - ]
>
> 不是专属详尽的例子：标题：视频订阅数组：[Netflix，HBO，亚马逊]因为一个人没有必要完全拥有其中一个元素。他或她可能没有或多于一个。

## [#1283@没有dirty maps的SSTORE净gas计量 !final](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1283.md)

> 该EIP建议对SSTORE操作码进行净Gas计量更改，为合约存储提供新的用途，并在不符合大多数实施方案的情况下减少过多的Gas成本。
>
> 该EIP提出了一种在SSTORE上进行Gas计量的方法（作为EIP-1087和EIP-1153的替代方案），使用大多数实施更普遍可用的信息，并且要求尽可能少地改变实施结构。
>
> - **Storage* 的原始值*。
> - **Storage* 的当前值*。
> - 退款账户。
>
> 受益于此EIP Gas减排计划的用途包括：
>
> - 后续存储在同一个调用帧内写入操作。这包括再入锁，同签多发等。
> - 在子调用帧和父调用帧之间交换存储信息，其中此信息不需要在事务之外持久化。这包括子帧错误代码和消息传递等。

## [#1319@智能合约包注册接口!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1319.md)

> 智能合约包注册的标准接口。其目标是建立一个框架，允许智能合约发布者使用任意业务逻辑设计和部署代码注册中心，同时公开一组公共端点可以为合约使用者检索资产。
>
>
> 一个清晰的标准将帮助现有的EthPM包注册中心从一个集中的、单一项目的社区资源发展成为一个分散的多注册中心系统，其组成部分被建议的接口绑定在一起。反过来，这些注册中心可以是ENS的名称空间，从而支持npm和其他包管理器用户熟悉的安装约定。

## [#1328@WalletConnect标准URI格式!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1328.md)

> 创建WalletConnect URI以启动应用程序和钱包之间连接的标准。该标准定义了如何使用URI对连接某些应用程序和钱包的数据进行编码。然后，该URI可以作为QR码显示，也可以作为链接显示给移动设备。

## [#1355@Ethash 1a!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1355.md)

> 为Ethash算法提供最小的更改集，以阻止和延迟采用基于ASIC的挖掘

## [#1418@区块链存储租金支付!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1418.md)

> 在每个块中，根据该帐户使用的存储量从每个帐户中扣除一定数量的值。最天真的实现是简单地遍历每个块上的每个帐户并扣除一定的费用。我们表明，更好的实施可以实现合理的性能。我们还审查了转向收费租金制度的实际考虑因素。

## [#1444@具有信号到文本的本地化消息传递!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1444.md)

> 一种将机器代码转换为任何语言和措辞的人类可读文本的方法。在许多情况下，最终用户需要来自智能联系人的反馈或指令。直接暴露数字代码不适用于良好的UX或DX。如果以太坊要成为一个真正的全球系统，专家和非专业人士都可以使用，那么在交易过程中发生的事情提供反馈的系统需要尽可能多的语言。
>
> 返回硬编码字符串（通常为英文）仅服务于全球人口中的一小部分。该标准提出了一种方法，允许用户创建，注册，共享和使用分散的翻译集合，从而实现更加文化和语言多样化的更丰富的消息传递。
>
> 有几种机器有效的方式来表示意图，状态，状态转换和其他语义信号，包括布尔值，枚举和[ERC-1066代码](https://eips.ethereum.org/EIPS/eip-1066)。通过为这些信号提供人类可读的消息，通过使用更多上下文返回更容易使用的信息来增强开发人员体验（例如`revert`）。通过提供可以传播到UI的文本来增强最终用户体验。

## [#1470@智能合约弱点分类（SWC）!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1470.md) *

>该EIP提出了以太坊智能合约中安全漏洞的分类方案。
>
>SWC计划的目标如下：
>
>- 提供一种直接的方法来分类智能合约系统中的弱点。
>
>- 提供一种直接的方法来识别导致智能合约系统中的漏洞的弱点。
>
>- 定义一种通用语言来描述智能合约系统的架构，设计和代码中的弱点。
>
>- 培训并提高智能合约安全分析工具的性能。
>
>  目前，对于以太坊智能合约特有的弱点，不存在这种弱点分类方案。安全弱点的共同语言和意识主要来自学术论文，最佳实践指南和发表的文章。审计报告和安全工具分析的结果增加了用于描述已发现的弱点的广泛术语。即使对于安全专家来说，理解技术根本原因和与不同来源的发现相关的风险通常也很耗时。

## [#1474@远程过程调用规范!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1474.md)

> 该提议定义了以太坊节点应实现的一组标准远程过程调用方法。由当前一代以太坊客户端创建的节点暴露出不一致且不兼容的远程过程调用（RPC）方法，因为不存在正式的以太坊RPC规范。该提案标准化了这样的规范，以便为开发人员提供可预测的以太网RPC接口，而不管底层节点的实现如何。

## [#1485@TEthashV1!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1485.md)

> 该EIP修改了ethash，以打破专门针对当前ethash挖掘算法的ASIC矿工。该EIP通过以非常低风险的方式修改PoW算法来推进“淘汰目前的ASIC矿工”提案，并且从弃用的FNV哈希算法更新到最新的哈希算法。
>
> 遵循TEthashV1算法建议PoW算法的安全转换并在MIX部分中保护FNV算法。通过更新FNV0算法，以最小的更改集提供原始的Ethash工作验证证明

## [#1491@人力花费计量标准（与之前的GAS计量类似）!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1491.md)

> 人力资本会计token的标准接口。以下标准允许在智能合约中实施HUCAPtoken的标准API。该标准提供了发现，跟踪和转移人力资源激励等级的基本功能。虽然区块链架构通过透明度在完整性金融化方面取得了成功; 相应地，现实世界的结果将与知识的资本个体化程度成正比。

## [#1577@ENS的内容哈希字段!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1577.md) *

> 该EIP为ENS解析器引入了新的contenthash字段，允许更好地定义将名称映射到网络和内容地址的系统。此外，不推荐使用`content`和`multihash`字段。包括[Metamask](https://metamask.io/)和移动客户端（如[Status](https://status.im/)）在内的多个应用程序已开始将ENS名称解析为托管在分布式内容存储系统（如[IPFS](https://ipfs.io/)和[Swarm](https://swarm-guide.readthedocs.io/)）。由于存储和寻址内容的方式多种多样，因此需要一个标准，以便这些应用程序知道如何解析名称，并且域所有者知道如何解析它们的内容。该`contenthash`字段允许在ENS中轻松指定网络和内容地址。
>

## [#1581@来自BIP-32树的非钱包使用的密钥!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1581.md)

> 该EIP描述了用于非钱包密钥对的[BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)钱包的派生路径结构。BIP32定义了一种生成密钥的分层树的方法，这些密钥树可以从公共主密钥导出。BIP32和[BIP44](https://https//github.com/bitcoin/bips/blob/master/bip-0044.mediawiki)将这些密钥的用法定义为钱包。在本EIP中，我们描述了在区块链范围之外的这些密钥的使用，这些密钥定义了密钥使用的逻辑树，该逻辑树可以与现有的BIP44兼容钱包共存（并因此共享相同的主服务器）。与区块链交互的应用程序通常使用额外的非区块链技术来执行它们所设计的任务。对于隐私和安全敏感机制，需要密钥集。重用用于钱包的密钥可能被证明是不安全的，而保持完全独立的密钥使得整套凭证的备份和迁移更加复杂。定义单独的（来自BIP44兼容的钱包）派生分支允许将独立密钥的安全性与具有需要备份或迁移的单条信息的便利性相结合。

## [#1484@数字身份聚合器!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1484.md)*

> 用于聚合数字身份信息的协议，该协议可与现有的，提议的和假设的未来数字身份标准广泛互操作。该EIP在以太坊区块链上提出了身份管理和聚合框架。它允许实体`Identity`通过单一的`Identity Registry`智能合约声明，通过各种有意义的方式将其与以太坊地址相关联，并使用它与智能合约进行交互。这使得任意复杂的身份相关功能成为可能。值得注意的是（在其他功能中）ERC-1484 `Identities`：是自我主权的，可以原生支持[ERC-725](https://github.com/NoahZinsmeister/ERC-1484/tree/master/contracts/examples/Resolvers/ERC725)和[ERC-1056](https://github.com/NoahZinsmeister/ERC-1484/tree/master/contracts/examples/Resolvers/ERC1056)身份，符合[DID标准](https://github.com/NoahZinsmeister/ERC-1484/blob/master/best-practices/DID-Method.md)，并且可以完全由[元交易](https://github.com/NoahZinsmeister/ERC-1484/tree/master/contracts/examples/Providers/MetaTransactions)提供动力。
>
> 新兴的标准，或由以太坊社区（包括EIP审查委员会提出的相关框架[725](https://github.com/ethereum/EIPs/issues/725)，[735](https://github.com/ethereum/EIPs/issues/735)，[780](https://github.com/ethereum/EIPs/issues/780)，[1056](https://github.com/ethereum/EIPs/issues/1056)，等等）定义和各种方式工具化的数字身份。随着现有方法的成熟，出现了新的标准，孤立的，非标准的身份开发方法，协调身份将对区块链用户和开发人员变得越来越繁重，并且涉及不必要的重复工作。
>
> 链上身份解决方案的激增可以追溯到这样一个事实：每个解释身份概念并将其与以太坊的特定方面联系起来（声明协议，每身份智能合约，签名验证方案等）。该提议避开了这种方法，而是在以太坊网络和个人身份应用程序之间引入协议层。这通过使任何身份驱动的应用程序能够利用不同意见的身份管理协议来解决身份管理和互操作性挑战。

## [#1613@加油站网络!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1613.md)

> 让非以太用户可以访问智能合约(例如dapps)，允许合约接受一种“调用集（[collect-calls](https://en.wikipedia.org/wiki/Collect_call)）”，支付调用费。让合约在公共可访问的通道上“监听”(例如web URL或耳语地址)。鼓励节点运行“加油站”来促进这一点。不需要网络更改，只需要最小的契约更改。
>
> - 通过以下方式提高用户对智能合约的调用
>   - 消除了用户获取ETH的麻烦。交易仍然由ETH支付，但费用可以由dapp承担，也可以由用户通过其他方式支付。
>   - 消除与区块链直接交互的必要，同时保持去中心化和抗审查。合约可以在多个公共通道上“监听”，用户可以通过通用协议与合约进行交互，即使在受限的环境中，一般也允许使用通用协议。
> - 以太坊节点无需采矿设备即可获得收入来源。整个网络受益于拥有更多节点。
> - 无需更改协议。加油站网络通过智能合约自组织，dapps通过实现接口与网络交互。

## [#1616@ERC-1616属性注册标准!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1616.md)

> EIP-1616提供了一个基本接口，用于查询注册表以获取分配给以太坊帐户的属性元数据。
>
> 该EIP包含以下核心思想：
>
> 1. 而不是直接依赖债权发行人的声誉来评估特定债权的真实性，信任可以提升到注册管理机构策展人的水平。我们称之为“ **属性注册表** ”的这个注册表允许降低实现的复杂性，因为需要验证属性的一方现在可以使用受信任的声明聚合器而不是依赖于各个声明提供者。
> 2. 债权被抽象为标准“属性”，表示分配给帐户的元数据，债权与发行方分离。属性`uint256 -> uint256`在每个帐户上注册为一个平面键值对，其重要属性是**每个属性类型对每个地址都有一个规范值**。此属性允许属性注册表和高级属性形成的可组合性。
> 3. 有一种通用方法可用于确定注册表提供的属性键或ID集。该标准未指定有关如何管理属性及其值的要求或建议，或者可能与属性关联的其他元数据。可能会在单独的EIP中提出一组标准的属性名称和元数据模式。
>
> 属性注册表的潜在高级用法包括：
>
> - 编码复杂的布尔表达式，将多个属性组合成单个uint256密钥，然后由注册表逻辑对其进行解析和评估。
> - 使用与属性关联的值来查询其他链接或脱链元数据。
> - 通过调用单独的属性注册表或其他合约来解析属性值，在不更改注册表接口的情况下委派权限。

## [#1620@资金流!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1620.md)

> 资金流表示在有限的时间段内持续支付的想法。块编号用作持续更新余额的时间代理。这种标准化的接口旨在改变我们对长期财务承诺的思考方式。由于区块链，支付不需要以块（例如月薪）发送，因为现在付费的开销要少得多。作为时间函数的金钱可以更好地在许多情景中调整激励。

## [#1681@时间重放保护!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1681.md)

> 该EIP建议以`valid-until`时间戳的形式为事务添加“时间”重放保护。此EIP非常类似于Nick Johnson和Konrad Feldmeier的<https://github.com/ethereum/EIPs/pull/599>，主要区别在于此EIP基于时钟时间/挂号而非块数。
>
> 引入基于时间的交易有效性有几种不同的激励因素。
>
> - 如果引入任何形式的粉尘账户清算，例如（<https://github.com/ethereum/EIPs/issues/168>），则需要引入重播保护，例如[https://github.com/以太坊/ EIPs / issues / 169](https://github.com/ethereum/EIPs/issues/169)。具有时间重放保护消除了在状态中改变随机行为的需要，因为事务不会在晚于用户明确设置的日期重放。
> - 在许多情况下，例如在ICO期间，很多人希望他们的交易很快（几小时内）或根本不包括在内。目前，交易排队并且可能几天不执行，对于用户（最终为未通过购买而支付Gas费用）和处理大型交易队列的网络都要付出代价。
> - 节点实现没有共同商定的度量标准，用于保留，丢弃或传播事务。在事务上使用TTL可以更容易地从系统中删除陈旧的事务。

## [#1706@禁用包含gasleft的SSTORE低于调用花费!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1706.md)

> 已被接受的提案更改了现有合约代码库的大部分安全性属性，这些属性可能无法更新和验证。这一提议将使旧的假设即使在网络升级之后仍然成立。
>
> [本文中](https://medium.com/chainsecurity/constantinople-enables-new-reentrancy-attack-ace4088297d9)描述的攻击。将调用花费明确指定为不变量将对以太坊协议安全性产生积极影响：[https](https://www.reddit.com/r/ethereum/comments/agdqsm/security_alert_ethereum_constantinople/ee5uvjt)：//www.reddit.com/r/ethereum/comments/agdqsm/security_alert_ethereum_constantinople/ee5uvjt

## [#1761@范围审批接口!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1761.md)

> 通过定义一个或多个tokenID的“范围”，允许在token合约中进行受限批准的标准接口。
>
> 在某些应用程序中，可能需要限制批准。限制性批准可以防止在用户没有审核他们所批准的合约的情况下发生损失。没有提供标准API来管理范围，因为这是特定于实现的。有些实现可能选择提供固定数量的范围，或者为某些类型分配一组特定的范围。其他实现可以向其用户开放范围配置，并提供创建范围和为其分配id的方法。

## [#1767@以太坊节点数据的GraphQL的接口!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1767.md)*

> 此EIP指定用于访问存储在以太坊节点上的数据的GraphQL架构。它旨在提供对通过当前JSON-RPC接口公开的只读信息的完全替代，同时提高可用性，一致性，效率和面向未来。
>
> 以太坊节点的当前JSON-RPC接口有许多缺点。它在区域中非正式且不完整地指定，这导致围绕诸如空字节串的表示（“”vs“0x”vs“0x0”）之类的问题不兼容，并且它必须对用户将请求的数据进行有根据的猜测，这通常会导致不必要的工作。
>
> 例如，该`totalDifficulty`字段与常见的以太坊节点实现中的块头分开存储，并且许多调用者不需要该字段。但是，每次调用`eth_getBlock`仍会检索此字段，需要单独的磁盘读取，因为RPC服务器无法知道用户是否需要此字段。
>
> 类似地，go-ethereum中的事务收据作为每个块的单个二进制blob存储在磁盘上。获取单个事务的收据需要获取和反序列化此blob，然后查找相关条目并将其返回; 这是通过`eth_getTransactionReceipt`API调用完成的。API使用者的一个常见任务是获取块中的所有收据; 因此，节点实现最终会反复获取和反序列化相同的数据，从而导致`O(n^2)`尝试从块中获取所有事务收据而不是`O(n)`。
>
> 其中一些问题可以通过对现有JSON-RPC接口的更改来修复，但代价是使接口复杂化。相反，我们建议采用标准查询语言GraphQL，这有助于实现更高效的API实现，同时还提高了灵活性。

## [#1812@以太坊可验证的债权!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1812.md)*

> 使用[EIP 712签名类型数据的](https://github.com/ethereum/EIPs/issues/712)可重复使用的可验证声明。
>
> 可重复使用的离线可验证债权提供了一个重要的功能，即将智能合约与现实世界的组织要求相结合，例如满足监管要求，如KYC，GDPR，认可投资者规则等。
>
> [ERC 735](https://github.com/ethereum/EIPs/issues/735)和[ERC 780](https://github.com/ethereum/EIPs/issues/780)提供了在链上生活的债权方法。这对于某些特定用例非常有用，其中一些关于地址的声明必须在链上进行验证。
>
> 在大多数情况下，虽然在不可变的公共数据库（如以太坊区块链）上记录包含个人识别信息（PII）的身份声明，但在某些情况下非常危险且在某些情况下是非法的（例如根据欧盟GDPR规则）。
>
> W3C [可验证债权数据模型和表示](https://www.w3.org/TR/verifiable-claims-data-model/)以及uPorts [验证消息规范](https://developer.uport.me/messages/verification)是提出的脱链解决方案。
>
> 虽然建立在[JSON-LD](https://json-ld.org/)和[JWT](https://jwt.io/)等行业标准之上，但它们都不容易与以太坊生态系统集成。
>
> [EIP 712](https://eips.ethereum.org/EIPS/eip-712)引入了一种签署链身份数据的新方法。这提供了基于Solidity ABI编码的数据格式，可以轻松地在链上解析新的JSON-RPC调用，现有的以太坊钱包和Web3客户端可以轻松支持这种调用。
>
> 这种格式允许可重复使用的离线可验证债权以便宜的方式发给用户，用户可以在需要时提供这些债权。

## [#1820@伪自审查的注册表合约!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1820.md)

> 该标准定义了一个注册表，智能合约和常规帐户可以直接或通过代理合约发布它们实现的功能。
>
> 任何人都可以查询此注册表以询问特定地址是否实现给定接口以及哪个智能合约处理其实现。
>
> 此注册表可以部署在任何链上，并在所有链上共享相同的地址。
>
> 作为最后28个字节的零（0）的接口被认为是[ERC165](https://eips.ethereum.org/EIPS/eip-165)接口，并且该注册表应该将调用转发给合约以查看它是否实现了接口。
>
> 该合约还可作为[ERC165](https://eips.ethereum.org/EIPS/eip-165)缓存，以减少Gas消耗。

## [#1822@通用可升级代理标准(UUPS)!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1822.md)*

> 下面描述了代理合约的标准，它与所有合约普遍兼容，并且不会在代理和业务逻辑合约之间产生不兼容性。这是通过利用代理合约中的唯一存储位置来存储逻辑合约的地址来实现的。兼容性检查可确保成功升级。升级可以无限次执行，也可以由自定义逻辑确定。另外，提供了一种用于从多个构造器中进行选择的方法，其不会抑制验证字节码的能力。
>
> - 改进现有代理实现，以改善开发人员部署和维护代理和逻辑协定的体验。
> - 标准化和改进验证代理协议使用的字节码的方法。

## [#1829@椭圆曲线线性组合的预编译!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1829.md)

> 目前，该EVM只支持*secp261k1*通过有限的途径`ecrecover`和*altbn128*通过两个预编译。有草案提案可以添加更多曲线。还有许多椭圆曲线可用于与现有系统集成，或者用于零知识证明的新开发曲线。
>
> 此EIP添加了一个允许使用整类曲线的预编译。

## [#1844@ENS接口发现!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1844.md)

> 此EIP指定了一种方法，用于公开与ENS名称或地址（通常是合约地址）关联的接口，并允许应用程序发现这些接口并与之交互。接口可以通过目标合约（如果有）或任何其他合约来实现。
>
> EIP 165支持接口发现 - 确定给定地址的合约是否支持所请求的接口。但是，在许多情况下，能够发现与其他合约实现的名称或地址相关联的功能非常有用。
>
> 例如，token契约本身可能不提供任何类型的“原子交换”功能，但可能存在相关的合约。通过ENS接口发现，token合约可以公开此元数据，通知应用程序可以在哪里找到该功能。

