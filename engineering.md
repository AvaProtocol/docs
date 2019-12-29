# Engineering Guideline 工程规范

## 开篇，为什么要保证规范？
在实际工程中，代码需要不断修改，旧的代码会过时，在多人协作的情况下，如果没有统一的代码格式，阅读起来是相当困难的。

Javascript代码库均需要使用 es2015,stage-2 preset代码格式

## 问题解决流程
1. 一个任务首先要明确需求，如果不是很清楚，用自己的语言把需求重复出来，得到确认之后再开始工作
1. 如果是修bug，先明确问题的重现方式
1. Debug！Debug！Debug！调试源代码是最好的解决问题的办法
1. 创建分支，修改代码，修改完成之后重复问题重现操作，确定问题不会被重现
1. 提交Github  PR，并在PR里进行代码Review和评论

## 沟通注意事项
1. 主要沟通渠道是微信群，但是有特定对象的对话，一定@ ，重要的事情，对方没有回复的时候，直接打电话过去。 不要傻等
1. 收到消息，一定要及时回复，处理问题，一定要及时更新进度。 比如我在外面吃饭，我要回复，现在不在，2小时候回家处理。 比如我debug了1个小时，毫无头绪，我要回复这个问题很难，感觉还要很久。 这样其他人才能了解互相的进展。
1. 功能，修改说清楚，举个例子， 错误的汇报例子就是说 xxx那个好了。什么叫好了，好到什么程度了？怎么好的？ 肯定好了吗？ 好的在哪里。清晰的沟通语句包括：
    - 出现的问题是xxx, 我复现了
    - 我找到的原因，是某个组件在xxx的时候xxx了，导致了xxx
    - 我有一个fix，就是在xxx里面改了xxx，这样就避免了xxx
    - 我测试了没有问题，然后我pr了，你去review一下，看看是不是好了
1. 项目负责人需要不断提醒项目deadline

## 代码规范
### 命名
1. 不论是常量、变量还是函数的命名，名字必须有意义，坚决不能出现像tmp这样的变量名
1. 变量、函数名的首字母要小写，不能以数字开头，遵循驼峰规则
1. 布尔值变量的命名需要以be, has, should等开头，表示状态，比如 isActive
1. 函数名应该为动词，比如 imageFromPath -> loadImageFromPath
1. 常量定义需要在文件头部，库引用之后，正文之前，单词之间使用_隔开，例如
    `const MAX_POOL_SIZE = 10;`

### 函数
1. 函数内部不允许嵌套函数
1. 函数参数不得超过3个，超过3个需要使用object封装所有参数
1. 循环语句内不允许定义变量，需要使用的变量需要在循环之前定义
1. Switch必须有default处理case

### 注释
1. 每个函数必须有配套的英文注释，这个函数是做什么的，变量是什么，有什么要求，例如, returns ... when ... （在满足什么条件的情况下返回什么）
    ```
    /**
    * Returns checksummed address for RSK
    * @param {string} address
    * @param {number} chain where checksummed address should be valid.
    * @returns {string} address with checksum applied.
    */
    function toChecksumAddress(address, chainId = null) {
    ... ...
    }
    ```

1. 每个新创建的文件，需要在文件头标注出文件的owner，今后所有对这个文件的改动需要经过文件owner的审阅
1. 对于比较复杂的逻辑，比如嵌套if else，循环，需要在代码段之前加注释，解释逻辑
1. Production代码里不能出现注释的代码，不用的代码就删掉
1. Production代码里不能出现console.log的代码，调试的代码测试好了再提交

### 常用工具
以下常用的Js工具函数需要统一使用lodash库的对应函数，为了避免出现 transactions.filter()中transactions object/array undefined的问题。
.filter(), .includes(), .map(),.filter()

### 代码提交
1. Git commit和PR的标题需要解释清楚这个Change/PR是做什么，加了什么功能，或者解决了什么问题，常用句式"Fixed a problem where ..."，如果字数很少可以写上文件路径，最少字数20字。强制git-commit hook限制，https://gist.github.com/leocaseiro/be1e82ebd68edf18f613433a68861672
补充：__PR里需要写上 "Fix \<Issue Number\>"__
1. 写的新功能，或者修的bug，在代码提交之前务必经过自己手动测试，保证代码在各种情况下是正常工作的
1. 每个功能，或者bug的修复都需要创建一个新的branch，然后在Github创建Pull Request，提交给文件的owner和项目的owner进行审阅.
    - 如果有任何的Request Change，必须修改之后进行重审
    - 如果没有Request Change，可以在至少一人Approve的情况通过进行merge
1. 创建PR之前，需要先git merge master，保证自己的代码是在最新的master之上的
1. 创建了PR之后，自己一定先在PR审阅所有更改过的代码，在给别人审阅之前保证代码符合规范。
1. Merge的时候需要Squash merge，把在这个分支之上的所有commit打包一起送到master
1. Merge之后需要在远程删除分支branch


如对本文档有任何修改建议，请直接修改并PR。
