# -playwright_mcp-
记录一下使用playwright_mcp的经验

## 选择一：本地安装
https://playwright.dev/docs/intro
将代码克隆到本地

### 1.下载Node.js与初始化
可按操作系统选择不同的版本和型号
下载完成后运行：
npm init playwright@latest
运行安装命令并选择以下内容开始：

在 TypeScript 或 JavaScript 之间进行选择（默认为 TypeScript）
测试文件夹的名称（如果您的项目中已经有测试文件夹，则默认为 tests 或 e2e）
添加 GitHub Actions 工作流程以轻松在 CI 上运行测试
安装 Playwright 浏览器（默认为 true）
至此完成配置

Playwright 将下载所需的浏览器并创建以下文件。

playwright.config.ts
package.json
package-lock.json
tests/
  example.spec.ts
tests-examples/
  demo-todo-app.spec.ts

可以在playwright.config中添加 Playwright 的配置，包括修改要在哪些浏览器上运行 Playwright。如果您在现有项目中运行测试，则依赖项将直接添加到package.json.

### 2.运行示例
默认情况下，测试将使用 3 个 Worker 在 Chromium、Firefox 和 WebKit 三种浏览器上运行。这可以在playwright.config 文件中进行配置。
测试以无头模式运行，这意味着运行测试时不会打开任何浏览器。测试结果和测试日志将显示在终端中。
npx playwright test
![image](https://github.com/user-attachments/assets/88bc7ed2-8566-4b95-a2bf-eb2edb2e8134)

由于示例的网页需要魔法，所以可以自己修改一下tests/example.spec.ts;可参考主包让大模型写的一个自动下载typora的ts文件

### 3.测试效果
npx playwright show-report
会展示在各个浏览器中的连接情况

## 选择二：容器
可挂载到本地目录，当服务器的linux如centOS等不被playwright_mcp支持时可使用（支持Ubuntu等）
### 1.拉取容器
docker pull mcr.microsoft.com/playwright:v1.52.0-noble
### 2.运行并挂载当前目录
docker run --rm -it \
  -v "$(pwd)":/app \
  -w /app \
  mcr.microsoft.com/playwright:v1.52.0-noble \
  /bin/bash -lc "npm ci && npx playwright test"
需要注意容器内playwright_mcp版本匹配镜像
