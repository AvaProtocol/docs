# simple-crowdfund搭建和流程理解

Crowd fund， 众筹，是让散户将自己的DOT代币借给平行链开发者，用于Polkadot网络插槽的拍卖。

Polkadot网络插槽拍卖规则，参考以下文章。

[怎样参与波卡插槽拍卖？什么时候开始拍？｜回答 8 个你关心的问题](https://mp.weixin.qq.com/s/ZtPHLj5q1Xf84QcjeiFdWA)

[波卡平行链卡槽如何分配、如何竞拍、成本是多少？](https://mp.weixin.qq.com/s?__biz=MzI3MzYxNzQ0Ng==&mid=2247485450&idx=1&sn=ea162d9fa9a60aa664fb0a9ad87985b3&chksm=eb21cf43dc56465550879a8f39c3b458346b51348781b89c4d62500955ff42f4569a42a538b3&scene=21#wechat_redirect)

[Obtaining a Parachain Slot on Polkadot](https://polkadot.network/obtaining-a-parachain-slot-on-polkadot/)



## simple-crowdfund模块搭建

1. recipes

   substrate.dev的recipes提供了很多可供学习的pallets，其中包含了simple-crowdfund。

   https://substrate.dev/recipes/crowdfund.html

2. 下载recipes源码

   

   我们将整个recipes工程下载，通过：

   ```
   git clone https://github.com/substrate-developer-hub/recipes.git
   ```

3. 编译recipes工程

   ```
   WASM_BUILD_TOOLCHAIN=nightly-2020-10-05 cargo build --release
   ```

4. 启动节点

   ```
   ./target/release/kitchen-node --dev -lruntime=debug
   ```

5. 设置类型数据

   1. 在浏览器中打开https://console.acala.network/

   2. 点击"设置"-"开发者"

   3. 在文本中输入类型数据，保存。

      ```
      {
        "Address": "AccountId",
        "LookupSource": "AccountId",
        "ContinuousAccountData": {
          "principal": "u64",
          "deposit_date": "BlockNumber"
        },
        "U16F16": "[u8; 4]",
        "GroupIndex": "u32",
        "ValueStruct": {
          "integer": "i32",
          "boolean": "bool"
        },
        "BufferIndex": "u8",
        "AccountIdOf": "AccountId",
        "BalanceOf": "Balance",
        "FundInfoOf": "FundInfo",
        "FundInfo": {
          "beneficiary": "AccountId",
          "deposit": "Balance",
          "raised": "Balance",
          "end": "BlockNumber",
          "goal": "Balance"
        },
        "FundIndex": "u32",
        "InnerThing": {
          "number": "u32",
          "hash": "Hash",
          "balance": "Balance"
        },
        "SuperThing": {
          "super_number": "u32",
          "inner_thing": "InnerThing"
        },
        "InnerThingOf": "InnerThing"
      }
      ```

6. 用acala console连接本地节点

   在浏览器中打开https://console.acala.network/，设置连接URL为127.0.0.1:9944，点击转换。如下图：

   ![1](/Users/star/devlab/docs/images/1.png)





## simple-crowdfund模块流程理解

1. 流程：

   模块代码路径在pellets/simple-crowdfund。

   Simple-crowdfund提供了众筹的一个简单实现。

   1. 区块链开发者抵押(doposit)一些币作为众筹奖励。
   2. 散户调用contribute参与众筹。
   3. 当众筹成功后，开发者调用dispense将奖励分发。
   4. 散户调用dissolve获得奖励。
   5. 散户可以调用withdraw，退出众筹。

2. 使用acala console调用simple-crowdfund。

   点击“开发者” - “交易”，左侧下拉框选择simpleCrowdfund，右侧下拉框显示出可调用方法。通过create方法创建一个众筹。

   ![image-20210119225524674](/Users/star/Library/Application Support/typora-user-images/image-20210119225524674.png)

3. 查询众筹状态

   在runtimes/super-runtime/src/lib.rs中模块的定义

   ```
   parameter_types! {
   	pub const SubmissionDeposit: u128 = 10;
   	pub const MinContribution: u128 = 10;
   	pub const RetirementPeriod: u32 = 10;
   }
   
   impl simple_crowdfund::Trait for Runtime {
   	type Event = Event;
   	type Currency = Balances;
   	type SubmissionDeposit = SubmissionDeposit;
   	type MinContribution = MinContribution;
   	type RetirementPeriod = RetirementPeriod;
   }
   ```

   SubmissionDeposit是开发者设置的抵押物。 

   同样，浏览器也可以查询到众筹状态。

   点击“开发者” - “链状态”，左侧下拉框选择simpleCrowdfund，右侧下拉框选择funds，填入fundIndex参数，提交。

   ![3](/Users/star/devlab/docs/images/3.png)

