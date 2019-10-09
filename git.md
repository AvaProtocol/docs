# Git Flow相关

## 利用 pre-commit 做git commit hook的代码lint以及相关代码fix style的预处理
#### 为什么要 Lint？
- 更高的开发效率，工程师平均会花掉 50% 的工作时间定位和解决各种 Bug，其中不乏那些显而易见的低级错误，而 Lint 很容易发现低级的、显而易见的错误;

- 更高的可读性，代码可读性的首要因子是“表面文章”，表面上看起来乱糟糟的代码通常更难读懂;
#### 如何配置 pre-commit 做lint？
- 安装 [husky](https:/、github.com/typicode/husky)
  ```
  npm install -D husky
  ```
  or
  ```
  yarn add --dev husky
  ```
- 安装[lint-staged](https://github.com/okonet/lint-staged)
  ```
  npm install -D lint-staged
  ```
  or
  ```
  yarn add --dev lint-staged
  ```
- 在 package.json 中添加如下配置
  ```json
  "lint-staged": {
      "src/**/*.js": [
        "eslint --fix",
        "git add"
      ]
    },
    "husky": {
      "hooks": {
        "pre-commit": "lint-staged"
      }
    }
  ```
- 以上配置描述了lint-staged的作用域，以及husky介入流程的hook,这是对pre-commit 的一个基础实践,更复杂的控制请阅读相关文档;
