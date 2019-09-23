## [#191@签署数据标准!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-191.md) 

> 这个ERC提出了一个关于如何在Ethereum合约中处理已签名数据的规范。
>
> 有几个使用多签名的钱包已经接受了`presigned`的交易，一个`presigned`的交易。`presigned`的交易是一个带有签名(r、s、v)的signed_data二进制块，因为没有指定对signed_data的解释，导致了以下几个问题:
>
> * 标准的Ethereum交易可以作为signed_data提交。Ethereum交易可以分解成以下几个数据段: `RLP<nonce, gasPrice, startGas, to, value, data>` (这里把它叫做 `RLPdata`), `r`, `s` a和 `v`. 如果`signed_data`是没有语法约束的，那么这就意味着RLPdata会被认作是语法有效的presigned交易
> * 多签名钱包还存在一个问题，即presigned的交易无法绑定到特定的验证器:
>   1. 用户 `A`, `B` 和 `C` 拥有 `2/3`-wallet `X`
>   2. 用户`A`, `B` 和`D` 拥有 `2/3`-wallet `Y`
>   3. 用户`A` 和 `B` 在x提交 `presigned` 交易.
>   4. 攻击者可以拒绝A,B在x的 presigned 交易, 并且把它通过 `Y`提交.

## [#712@以太坊类型的结构化数据哈希和签名!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-712.md) *

> 该EIP旨在提高离线消息签名在链上的可用性。我们看到越来越多的人采用脱机消息签名，因为它节省了算力，减少了区块链上的事务数量。当前已签名的消息是一个不可读的的十六进制字符串，显示给用户，几乎没有关于组成消息的项的上下文。
>
> [![img](https://github.com/ethereum/EIPs/raw/master/assets/eip-712/eth_sign.png?raw=true)](https://github.com/ethereum/EIPs/blob/master/assets/eip-712/eth_sign.png?raw=true)
>
> 在这里，我们提出了一种对数据进行编码的方案及其架构，允许在签名时将其显示给用户进行验证。以下是签署EIP712消息时可以显示用户的示例。
>
> [![img](https://github.com/ethereum/EIPs/raw/master/assets/eip-712/eth_signTypedData.png?raw=true)](https://github.com/ethereum/EIPs/blob/master/assets/eip-712/eth_signTypedData.png?raw=true)

## [#725@代理账户!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-725.md) *

> 下面描述一个惟一的可识别代理帐户的标准功能，该代理帐户将被人类、组织和机器使用。代理有两种能力:(1)它可以执行任意合约调用，(2)它可以通过通用键值对的形式存储任意数据。其中一把钥匙应该是合约所有者的。对于更复杂的管理逻辑，所有者可以是关键管理器合约。最重要的是，这个合约应该是一个持久的可识别配置文件的参考点。
>
> 标准化代理帐户的最小接口允许第三方以一致的方式与各种代理帐户合约进行交互。这样做的好处是一个持久性帐户，它不依赖于单个密钥，可以附加任意数量的信息到verifiy，或者增强帐户的用途。

## [#777 @新的更高级的令牌标准!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-777.md) *

> 此EIP定义了token合约的标准接口和行为。
>
> 本标准试图改进广泛使用的[ERC20 ](https://eips.ethereum.org/EIPS/eip-20)token标准。该标准的主要优点是：
>
> 1. 让token使用与以太相同的传输理念，即send(dest, value, data)。
> 2. 合约和常规地址都可以通过注册`tokensToSend`钩子来控制和拒绝它们发送的token。（拒绝是通过`revert`钩子函数完成的。）
> 3. 合约和常规地址都可以通过注册`tokensReceived`钩子来控制和拒绝他们收到的token。（拒绝是通过`revert`钩子函数完成的。）
> 4. 该`tokensReceived`钩子允许token发送到合约，并只用在一方的交易进行通知，不像[ERC20](https://eips.ethereum.org/EIPS/eip-20)需要两方通知（`approve`/ `transferFrom`）来实现这一目标。
> 5. token持有者可以“授权”和“撤销”可以代表他们发送token的代理账户。这些代理账户是一种经过验证的合约，例如交换机，支票处理器或自动收费系统。
> 6. 每个token交易包含`data`和`operatorData`字节字段，分别用于自由地从token持有者和操作员传递数据。
> 7. `tokensReceived`通过部署实现`tokensReceived`钱包钩子的代理协议向后兼容不包含挂钩功能的钱包。

## ![img](file:///C:\Users\TANREN~1\AppData\Local\Temp\SGPicFaceTpBq\6964\A4E1E9EA.png)[#902@token验证!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-902.md) *

> 用于提供token所有权和转移验证的服务的协议。
>
> 资产的代币化具有广泛的应用，尤其是证券和证券代币等金融工具。大多数司法管辖区对可交易的东西设置了法律限制。从广义上讲，这包括KYC和AML验证，但也可能包括基于时间的支出限额，交易总量等。
>
> 监管机构和受到制裁的第三方合规机构需要通过某种方式，将身份和住所等链外合规信息与链上服务联系起来。这种设计的应用范围比法律法规更广，包括用于创建、管理和交易令牌的各种业务逻辑权限。
>
> 与其每个令牌维护自己的白名单(或其他机制)，不如共享链上资源、规则、列表等等。人们还希望聚合跨多个验证器的数据和规则，或者应用复杂的行为(例如切换逻辑、门、状态机)将分布式数据应用到应用程序中。
>
> ## `TokenValidator`
>
> ```
> interface TokenValidator {
>     function check(
>         address _token,
>         address _subject
>     ) public returns(byte statusCode)
> 
>     function check(
>         address _token,
>         address _from,
>         address _to,
>         uint256 _amount
>     ) public returns (byte statusCode)
> }
> ```

## [#927@授权!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-927.md) *

> 此EIP指定了一种通用授权机制，可用于实现各种授权模式，替换ERC20中的批准，ERC777中的操作员以及各种其他类型合约中的定制授权模式。
>
> 智能合约通常需要提供一个接口，允许第三方调用者代表用户执行操作。最常见的例子是token授权/操作符，但是在整个生态系统中也存在其他类似的情况，包括授权ENS域上的操作。通常，每个标准都为自己重新定义这个系统，导致大量不兼容的相同基本模式实现。在这里，我们提出了一个通用的方法，可用于所有这些契约。
>
> 这里实现的模式受到[ds-auth](https://github.com/dapphub/ds-auth)和OAuth的启发。

## ![img](file:///C:\Users\TANREN~1\AppData\Local\Temp\SGPicFaceTpBq\6964\A4E1BC33.png)[#1066@Status Codes!lastcall](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1066.md) *

> 此EIP提议了一种广泛适用的智能合约Status codes。该标准概述了一组与HTTP状态类似的通用状态代码。这提供了一组共享信号，以允许智能合约自动响应情况，向用户公开本地化错误消息等。
>
> 目前的技术水平要么除非已经明显的执行成功了，否则不回复（即：需要人为干预），要么返回很少有上下文的`true`或者`false`。Status codes与revert-with-reason类似，但是更加针对自动化的调试和最终用户反馈，它们完全兼容revert和revert-with-reason。
>
> 与HTTP的情况一样，拥有一组标准的已知codes对开发人员有很多好处。它们消除了需要为每个合约开发自己的方案的摩擦，使合约间自动化的调用执行更容易，并且更容易广泛地了解您的请求产生的有限状态。重要的是，它可以更容易区分预期的错误状态，需要暂停执行的真正异常条件，正常状态转换和各种成功案例。

## [#1470@智能合约弱点分类（SWC）!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1470.md) *

> 该EIP提出了以太坊智能合约中安全漏洞的分类方案。
>
> SWC计划的目标如下：
>
> - 提供一种直接的方法来分类智能合约系统中的弱点。
> - 提供一种直接的方法来识别导致智能合约系统中的漏洞的弱点。
> - 定义一种通用语言来描述智能合约系统的架构，设计和代码中的弱点。
> - 培训并提高智能合约安全分析工具的性能。
>
> 目前，对于以太坊智能合约特有的弱点，不存在这种弱点分类方案。安全弱点的共同语言和意识主要来自学术论文，最佳实践指南和发表的文章。审计报告和安全工具分析的结果增加了用于描述已发现的弱点的广泛术语。即使对于安全专家来说，理解技术根本原因和与不同来源的发现相关的风险通常也很耗时。

## [#1577@ENS的内容哈希字段!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1577.md) *

> 该EIP为ENS解析器引入了新的contenthash字段，允许更好地定义将名称映射到网络和内容地址的系统。此外，不推荐使用`content`和`multihash`字段。包括[Metamask](https://metamask.io/)和移动客户端（如[Status](https://status.im/)）在内的多个应用程序已开始将ENS名称解析为托管在分布式内容存储系统（如[IPFS](https://ipfs.io/)和[Swarm](https://swarm-guide.readthedocs.io/)）。由于存储和寻址内容的方式多种多样，因此需要一个标准，以便这些应用程序知道如何解析名称，并且域所有者知道如何解析它们的内容。该`contenthash`字段允许在ENS中轻松指定网络和内容地址。

## ![img](file:///C:\Users\TANREN~1\AppData\Local\Temp\SGPicFaceTpBq\6964\A4E08818.png)[#1484@数字身份聚合器!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1484.md)*

> 用于聚合数字身份信息的协议，该协议可与现有的，提议的和假设的未来数字身份标准广泛互操作。该EIP在以太坊区块链上提出了身份管理和聚合框架。它允许实体`Identity`通过单一的`Identity Registry`智能合约声明，通过各种有意义的方式将其与以太坊地址相关联，并使用它与智能合约进行交互。这使得任意复杂的身份相关功能成为可能。值得注意的是（在其他功能中）ERC-1484 `Identities`：是自我主权的，可以原生支持[ERC-725](https://github.com/NoahZinsmeister/ERC-1484/tree/master/contracts/examples/Resolvers/ERC725)和[ERC-1056](https://github.com/NoahZinsmeister/ERC-1484/tree/master/contracts/examples/Resolvers/ERC1056)身份，符合[DID标准](https://github.com/NoahZinsmeister/ERC-1484/blob/master/best-practices/DID-Method.md)，并且可以完全由[元交易](https://github.com/NoahZinsmeister/ERC-1484/tree/master/contracts/examples/Providers/MetaTransactions)提供动力。
>
> 新兴的标准，或由以太坊社区（包括EIP审查委员会提出的相关框架[725](https://github.com/ethereum/EIPs/issues/725)，[735](https://github.com/ethereum/EIPs/issues/735)，[780](https://github.com/ethereum/EIPs/issues/780)，[1056](https://github.com/ethereum/EIPs/issues/1056)，等等）定义和各种方式工具化的数字身份。随着现有方法的成熟，出现了新的标准，孤立的，非标准的身份开发方法，协调身份将对区块链用户和开发人员变得越来越繁重，并且涉及不必要的重复工作。
>
> 链上身份解决方案的激增可以追溯到这样一个事实：每个解释身份概念并将其与以太坊的特定方面联系起来（声明协议，每身份智能合约，签名验证方案等）。该提议避开了这种方法，而是在以太坊网络和个人身份应用程序之间引入协议层。这通过使任何身份驱动的应用程序能够利用不同意见的身份管理协议来解决身份管理和互操作性挑战。

## ![img](file:///C:\Users\TANREN~1\AppData\Local\Temp\SGPicFaceTpBq\6964\A4E05BF7.png)[#1767@以太坊节点数据的GraphQL的接口!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1767.md)*

> 此EIP指定用于访问存储在以太坊节点上的数据的GraphQL架构。它旨在提供对通过当前JSON-RPC接口公开的只读信息的完全替代，同时提高可用性，一致性，效率和面向未来。

## ![img](file:///C:\Users\TANREN~1\AppData\Local\Temp\SGPicFaceTpBq\6964\A4E02602.png)[#1822@通用可升级代理标准(UUPS)!draft](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1822.md)

> 下面描述了代理合约的标准，它与所有合约普遍兼容，并且不会在代理和业务逻辑合约之间产生不兼容性。这是通过利用代理合约中的唯一存储位置来存储逻辑合约的地址来实现的。兼容性检查可确保成功升级。升级可以无限次执行，也可以由自定义逻辑确定。另外，提供了一种用于从多个构造器中进行选择的方法，其不会抑制验证字节码的能力。
>
> - 改进现有代理实现，以改善开发人员部署和维护代理和逻辑协定的体验。
> - 标准化和改进验证代理协议使用的字节码的方法。。