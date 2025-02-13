## SECURITY

### datasource-1 
> https://learnblockchain.cn/article/5853

#### chapter-1
- 重入攻击
  - 合约调用回调函数时，可以理解为把控制权交出来
  - 不重视CEI 合约的中间态 就会被利用，完成重入攻击
  - 可能跨函数 甚至跨合约
  - 只读重入 不太清晰了
- 访问控制
  - claim之后没有维护claimed列表，导致反复claim
  - 交易机器人被利用，没详细介绍，姑且放在这里
- 不正确的输入验证
- 过多的函数限制
  - 可以避免被盗  也可能导致资金被锁
  - 有详细资料
  - 掌握好平衡  权利太大 太小都不行
- 双重投票
  - 解决办法  快照
- 闪电贷治理攻击
  - 通过闪电贷 临时获得额度 提高投票权等
- 闪电贷价格攻击
- isContract
  - 不稳定
  - 能确定一个地址肯定是合约  但是没法确定它不是合约
- tx.origin
  - 仅使用tx.origin进行判断，可能会发生中间人攻击
  - 判断了发起人，无法避免中间被动手脚
  - msg.sender == tx.origin
  - tx.origin 使用频率不高，一定确定好是否安全
- gas导致拒绝服务
  - 内部函数写了一个死循环
  - 外部函数虽然每次会保留1/64gas
  - 但是多次调用下来  也几乎没的剩了
  - 内部函数也可能返回一个巨大数组  消耗gas
    - 这种可以不接收 或者限制大小   assembly
  - 清理slot  也可能是由于gas成本过高 失败
  - onERC721Received 可能强制回滚 故意捣乱


#### chapter-2
- 不安全的随机数
略

#### chapter-3
- ERC20 代币问题
  - 可能转账扣费  要确认好合约内容
  - rebase代币  每个人的余额不是固定的，随着池子而不动
  - ERC777 在uniswap中发生了重入  received回调函数导致
  - 不是所有转账都会返回true,有的revert 有的false
  - 地址投毒  转账零代币
- 借贷协议
  - 略
- 抵押协议漏洞
  - 略
- 未检查的返回值
  - 必须得确定结果  然后进行后续的逻辑；否则破坏事物了
- msg.value 在一个循环中
  - multicall中 被重复使用，双花
- 私有变量
  - 私有不代表不可见
  - 一种是线上合约可以使用
  - 一种我直接fork mainnet 读取之后使用
- 不安全的代理调用
  - delegateCall 危险的合约，直接把钱给转走了，gg
- 升级与代理有关的bug
  - 略



#### chapter-4
- 权利过大的管理员
  - 权利过大的管理员  也会对合约造成风险
- Ownable2Step
- 四舍五入的错误
  - 零头可能会造成损失，需要注意
- 抢跑
  - MEV
  - 抢先调用提款函数
  - 抢先向ERC4626 通胀攻击；先存钱进去，导致后面的人获得份额降低
  - 授权抢跑
    - 本来授权100 想更新为授权50
    - 脚本趁机先提走100 然后授权还是50 相当于拿到了150￥
    - 解决： 更新前 先归零
  - 三明治攻击
  - 黑暗森林  两篇文章
- 签名
  - 授权交易 之后执行
  - ERC20Permit 
  - ecrecover 签名无效这里会返回address(0) 需要防范
  - 需要防范 重放攻击，一个签名不能多多次
  - 签名可塑性  可以根据真签名推导山寨签名   也能通过校验
  - 安全签名
    - 条件？？？
  - 略  这里需要补充
- 编译器版本错误
- 假设合约是不可变的？ 是可能升级的
- transfer  send 限制2300gas，与多签钱包一起用，容易出现gas耗尽
- solidity内置 算术溢出   可以使用unchecked取消检查
- 边缘案例  
  - 验证者为0，是否通过验证
  - 除0
  - compound 用户stake 0 token 却领取了代币
- 真实世界的黑客
- 钱包的攻击载体
  - 不够随机  比如前导零的地址
- 大多数漏洞是特定应用发生
- 单元测试
  - slither
  - 单元测试
  - matation 测试
  - fuzz
  - 不变量测试
  - 形式化验证  ？？
- 更多资源链接
- 安全审计是一个有限的市场，无限的难度
- 路还很长

