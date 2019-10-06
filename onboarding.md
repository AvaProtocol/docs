# Onboarding Guideline 新人入职手册

## 任务进度跟踪
团队使用Monday.com软件跟踪任务进度，团队成员在使用时需要按照以下步骤：
1. 每天先检查分配给自己的所有任务，先处理Priority:High和已经Pass Deadline的任务。
1. 在开始一项任务时，把该任务的Status设置为"Working on it"，并且在Timeline里设置预计的天数（不满一天的按一天算）。
1. 在做完一项任务（比如PR已经merge）时， 把该任务的Status设置为"Done"。
1. 如在任务执行过程中遇到了问题、逾期等问题，需要在该任务下留言，说明情况，以便其他成员了解任务的进展。

## 日常任务
每天早晨北京时间十点开日常组会，使用微信语音。每位团队成员用2-3分钟的时间进行交流，内容包括：
1. 昨日的任务进展
1. 今日的任务计划
1. 如果当前遇到了问题，被block了，这是一个很好的机会来向其他团队成员寻求帮助，任何昨天花了超额时间但是还没有解决的问题都应该在此时提出来。早晨的两个小时时间应被大家充分利用，来解决彼此之间遇到的问题。
1. 任何建议和好工具的分享都可以在此时提出来。

## 工程规范
请查看[Engineering Guideline 工程规范](./engineering.md)一节。

## 开发环境配置
Please install and prepare below software for development.

* __Visual Studio Code for Mac__ Coding IDE. A few must-have plugins are:
    * *ESLint (Required)* - Auto-formatting Javascript code 
    * *GitLens* - Display git history in editor
    * *Prettify JSON* - JSON file formatting
    * *Beautify css/sass/scss/less* - CSS beautifier
    * *Sort package.json* - Package.json formatting

* __Chrome Devtools__ for Node.js debugging
Type `chrome://inspect/` in Chrome address bar and click on "Open dedicated DevTools for Node"

如对本文档有任何修改建议，请直接修改并PR。