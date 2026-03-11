<div align="center">
  <img src="https://gitee.com/OpenClaw-CN/openclaw-website/raw/master/docs/public/logo.png" width="200" alt="OpenClaw CN Logo">
  <h1>OpenClaw 中国社区 (CN)</h1>
  <p>
    <b> 🦞 你的私人 AI 助手 (Personal AI Assistant)</b>
  </p>
  <p>
    <code>Based on OpenClaw v2026.2.23</code>
  </p>
  <p>
    更安全 · 更懂中文 · 适配 DeepSeek 的 Local Agent 基础设施
  </p>
  <p>
    <a href="https://open-claw.org.cn">🌐 官方文档</a> • 
    <a href="./README_EN.md">English README</a>
  </p>
</div>

> **OpenClaw CN (中国社区版)** 是 OpenClaw 的本地化维护版本，针对国内网络环境进行了深度优化，**源码级原生支持** DeepSeek/Qwen 等国产大模型，致力于让中国开发者拥有最顺滑的 Agent 开发体验。

---

## 🇨🇳 社区版特性 (Features)

本项目是 OpenClaw (原Clawdbot) 的**中国本土化维护版本**，针对国内网络与使用习惯进行了深度优化：

* 🧠 **国产大脑**：源码内置 **DeepSeek-V3** / **Qwen** 支持，开箱即用，告别昂贵的 Claude。
* 🛡️ **安全审计**：代码经社区人工审计，剔除潜在风险与不必要的遥测。
* ⚡ **极速下载**：预配置 pnpm 国内镜像源，安装依赖“飞”一般的感觉。
* 🔌 **生态落地**：已集成飞书并开发企业微信、钉钉等连接器。

## 🚀 快速开始 (Quick Start)

请访问我们的文档站查看保姆级教程：
这里探索了极速的一键安装方式，网站内提供了针对 Windows 原生环境、Linux、WSL 与 macOS 的保姆级图文部署指南。

👉 **[https://open-claw.org.cn/guide/getting-started](https://open-claw.org.cn/guide/getting-started)**

---
## ⚡️ 源码安装（贡献者/开发）

为了确保在国内网络环境下依赖能快速下载，我们**强制推荐**使用 `pnpm` 并配置国内镜像源。

### 1. 准备环境
确保你的 Node.js 版本 `≥22`。如果未安装 pnpm，请运行：
```bash
npm install -g pnpm
```

### 2. 配置国内高速镜像 (关键步骤)
在开始安装前，请务必设置 pnpm 的镜像源，否则下载速度会很慢：
```bash
# 设置淘宝/阿里云镜像
pnpm config set registry https://registry.npmmirror.com/
```

### 3. 安装与启动
```bash
# 1. 克隆仓库
git clone https://gitee.com/OpenClaw-CN/openclaw-cn.git
cd openclaw-cn

# 2. 安装依赖
pnpm install

# 3. 首次构建 UI 依赖
pnpm ui:build

# 4. 构建项目
pnpm build

# 5. 启动初始化向导
pnpm openclaw onboard --install-daemon

# 6. 使用初始化向导安装完毕后，再次启动网关（关闭后再次启动）
pnpm openclaw gateway

# 6.1 再次启动网关后，如何再次打开管理页面（管理页面已关闭的前提下）
pnpm openclaw dashboard
```

> **💡 开发模式**: 如果你想修改源码并实时预览，请使用 `pnpm gateway:watch` 启动。

---

## 🗑️ 卸载说明 (Uninstall)

如果你需要移除 OpenClaw 服务，请根据操作系统执行以下命令。

### 删除命令（CLI已安装）
源码安装的执行以下命令
```bash
pnpm openclaw uninstall
```

一键脚本安装的执行以下命令
```bash
openclaw uninstall
```

*如果 CLI 已删除但服务仍在运行，使用手动服务移除。*，执行以下命令：

```bash
npm rm -g openclaw
pnpm remove -g openclaw
bun remove -g openclaw
```

如果你安装了 macOS 应用：
```bash
rm -rf /Applications/OpenClaw.app
```

*如果 CLI 未安装，使用手动服务移除。*执行以下命令：

### 🍎 macOS (launchd)
默认标签是 `bot.molt.gateway`（或 `bot.molt.<profile>`；旧版 `com.openclaw.*` 可能仍然存在）：
```bash
# 1. 停止并移除服务
launchctl bootout gui/$UID/bot.molt.gateway
rm -f ~/Library/LaunchAgents/bot.molt.gateway.plist

# 2. (可选) 清理遗留的旧版配置
# 如果使用了配置文件，请将标签和 plist 名称替换为 bot.molt.<profile>
# 如果存在任何旧版 com.openclaw.* plist，请将其移除。
rm -f ~/Library/LaunchAgents/com.openclaw.*
```

### 🐧 Linux (systemd)
默认单元名称是 `openclaw-gateway.service`（或 `openclaw-gateway-<profile>.service`）：
```bash
# 1. 停止用户级服务
systemctl --user disable --now openclaw-gateway.service

# 2. 移除服务文件并重新加载
rm -f ~/.config/systemd/user/openclaw-gateway.service
systemctl --user daemon-reload
```

### 🪟 Windows (计划任务)
默认任务名称是 `OpenClaw Gateway`（或 `OpenClaw Gateway (<profile>)`）。

**CMD (命令提示符):**
```cmd
schtasks /Delete /F /TN "OpenClaw Gateway"
rmdir /s /q "%USERPROFILE%\.openclaw"
```

**PowerShell:**
```powershell
Unregister-ScheduledTask -TaskName "OpenClaw Gateway" -Confirm:$false
Remove-Item -Path "$env:USERPROFILE\.openclaw" -Recurse -Force
```

---

## 🧠 模型配置 (DeepSeek 原生支持)

得益于 OpenClaw CN 的源码改造，DeepSeek 已成为系统的一级公民，配置极其简单。

### 方法一：通过向导配置 (最推荐)
在运行 `pnpm openclaw onboard` 时：

1.  **Select Provider**: 在列表中直接选择 👉 **`DeepSeek (Recommended for CN)`**
2.  **API Key**: 输入您的 DeepSeek Key (`sk-xxxxxxxx`)。
3.  **Model**: 系统会自动为您配置 `deepseek-chat` (V3) 为默认模型。

### 方法二：手动修改配置文件
如果您需要手动干预配置，请修改 `~/.openclaw/openclaw.json`。得益于原生集成，您只需关注 `auth` 和 `agents` 部分：

```json
{
  "auth": {
    "profiles": {
      "deepseek:default": {
        "provider": "deepseek",
        "mode": "api_key",
        "apiKey": "sk-你的Key"
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "deepseek/deepseek-chat"
      }
    }
  }
}
```

> **注意**：无需再手动配置 `baseUrl`，系统源码已内置 DeepSeek 官方节点 (`https://api.deepseek.com`)。

---

## 🤝 参与贡献与生态共建 (Contributing & Sponsorship)

OpenClaw CN 是一场由开发者驱动的本地化“闪电战”。目前，我们已成功筑基，完成了 DeepSeek 等国产大模型的源码级适配。现在，我们正向着路线图的第二阶段——**“连接中国互联网生态”** 全面冲刺！

我们深知，一个繁荣的开源生态离不开所有人的共建。无论是提交一行高质量的代码，还是提供生态赞助，都是在为中国 Agent 基础设施添砖加瓦。

### 🛠️ 成为核心贡献者 (Code & Community)
这个时代属于行动者，我们急需你的加入来推进以下关键战役：
1.  **生态连接器开发 (核心)**：参与开发 **企业微信、钉钉** 等国民级应用的接入插件，赋予 AI 真正接管繁琐工作的手脚。
2.  **极简部署与测试**：在多平台下测试源码安装流程，或参与构建 Docker 模板等运维自动化方案，发现 Bug 请随时提交 Issue。
3.  **文档与布道**：撰写你的 Agent 实战教程，分享给更多开发者。

👉 **查看详细贡献指南**: [https://open-claw.org.cn/plugins/contribution](https://open-claw.org.cn/plugins/contribution)


### 💎 商业赞助 (Sponsorship)
我们坚信，开源生态的长期繁荣离不开健康的商业正循环。我们对商业化保持开放态度，并欢迎与开发者生态高度契合的优秀企业成为我们的战略赞助商。

如果你所在的团队希望通过支持开源获取本站高价值的开发者品牌曝光，欢迎联系社区主理人洽谈商业赞助事宜。

让我们一起使优秀的开源项目在中国更好地落地！

---

## 🔥 加入“燎原”计划

我们要寻找前 100 位核心贡献者。
如果你想第一时间体验 DeepSeek 适配版，或者想与我们一起从 0 到 1 建设生态，
欢迎加入 OpenClaw CN 早期共建群。

> **📢 关于微信交流群限流的说明**
> 随着 OpenClaw CN 社区的快速发展，每日入群人数激增。为了提供更优质的开源互助环境，避免群消息泛滥，**每天晚上 8 点将定时释放新的交流群二维码，各系统每日限额 1 个群**。如遇二维码提示“群已满”，请于次日晚 8 点再来扫码加入。

<div align="center">
  <table>
    <tr>
      <td align="center">
        <img src="./wechat-group-win.jpg" alt="Windows 交流群" width="200">
        <br>
        <b>🖥️ Windows 交流群</b>
      </td>
      <td align="center">
        <img src="./wechat-group-mac.jpg" alt="macOS 交流群" width="200">
        <br>
        <b>🍎 macOS 交流群</b>
      </td>
      <td align="center">
        <img src="./wechat-group-linux.jpg" alt="Linux 交流群" width="200">
        <br>
        <b>🐧 Linux/WSL 交流群</b>
      </td>
    </tr>
  </table>
</div>

---

## ☕ 请开发者喝杯咖啡 (Buy Us a Coffee)

OpenClaw CN 致力于为中国开发者提供最顺滑的 Local Agent 基础设施。如果这个开源项目为你节省了宝贵的开发时间，或者让你觉得“这代码写得有点意思”，欢迎用微信扫码，请团队喝杯冰美式！

你的微量赞助将直接用于填补服务器开销，以及购买更多的防脱发洗发水。

> **💡 特别说明**：本区块仅限个人开发者的“用爱发电”。如果您是企业用户，并希望获得首页 Logo 墙、导航栏底座等千万级流量曝光，请向上查看 **【🤝 商业赞助】** 板块。

<div align="center">
  <img src="./wechat-reward.jpg" alt="微信赞助" width="220">
  <br>
  <b>💚 微信赞助</b>
  <br><br>
  <p><i>❤️ 感谢每一位支持开源生态的开发者！</i></p>
</div>


## ⚠️ 免责声明

本项目基于 [OpenClaw Official](https://github.com/openclaw/openclaw) 构建。
原项目版权归原作者所有，遵循 MIT 协议。我们致力于让优秀的开源项目在中国更好地落地。

---
<div align="center">
  <sub>OpenClaw-CN Community © 2026</sub>
</div>