# Wi-Fi Calling (VoWiFi) 代理规则集

![License](https://img.shields.io/github/license/gujiangjiang/Wi-Fi-Calling-Ruleset-for-Clash?style=flat-square)
![Last Commit](https://img.shields.io/github/last-commit/gujiangjiang/Wi-Fi-Calling-Ruleset-for-Clash?style=flat-square)
![Stars](https://img.shields.io/github/stars/gujiangjiang/Wi-Fi-Calling-Ruleset-for-Clash?style=social)

一份专为身处特殊网络环境（如中国大陆）的用户打造的代理规则集，旨在解决境外运营商 SIM/eSIM 卡的 **Wi-Fi Calling (VoWiFi)** 功能激活失败或连接不稳定的问题。

## 🧐 这是什么？

您是否遇到过以下问题：

* 📱 境外手机卡在国内没有信号，但手机状态栏的 `Wi-Fi Calling` 或 `VoWiFi` 标志却迟迟不出现？
* 📶 明明连接着 Wi-Fi，Wi-Fi Calling 却频繁掉线，无法稳定通话或收发短信？
* 🤯 尝试了各种方法，包括开关飞行模式，但 Wi-Fi Calling 依然无法激活？

这通常是因为您的手机无法正常连接到运营商用于 Wi-Fi Calling 服务的特定服务器（ePDG 网关）。其连接请求可能遭到了 DNS 污染或 IP 地址屏蔽。

**本项目通过精确识别 Wi-Fi Calling 相关的网络流量，并将其引导至您的代理服务器，从而绕过网络限制，让您的手机仿佛置身于运营商归属地，最终实现稳定启用 Wi-Fi Calling。**

## ✨ 功能特性

* **覆盖广泛**：支持全球主流国家和地区的众多运营商，包括美国、英国、香港、加拿大、澳大利亚、新加坡等。
* **精确高效**：规则经过精炼，仅包含 Wi-Fi Calling 服务的核心域名和 IP 段，避免“污染”不相关的流量，提升代理效率。
* **兼容主流**：主要为 Clash 设计，同时提供适用于 Surge / Quantumult X 等流行代理工具的使用方法。
* **持续更新**：根据用户反馈和网络环境变化，持续维护和更新规则。

## 🔧 如何使用

### 准备工作

您需要：
1.  一个支持 Wi-Fi Calling 的境外运营商 SIM/eSIM 卡。
2.  一台能够稳定访问国际互联网的代理服务器（建议使用与您运营商归属地相同或相近地区的节点，如香港、日本、美国、新加坡等）。
3.  安装了 Clash / Surge / Quantumult X 等代理客户端。

---

### 1. Clash 用户

这是最推荐的使用方式，配置简单。

**第一步：在 `config.yaml` 中添加 `rule-providers`**

在您的主配置文件 `config.yaml` 的 `rules:` 字段之前，添加 `rule-providers` 部分：

```yaml
# config.yaml

rule-providers:
  WifiCalling:
    type: http
    behavior: classical # 或 domain
    url: "[https://github.com/gujiangjiang/Wi-Fi-Calling-Ruleset-for-Clash/raw/refs/heads/main/Wi-Fi_Calling.yaml](https://github.com/gujiangjiang/Wi-Fi-Calling-Ruleset-for-Clash/raw/refs/heads/main/Wi-Fi_Calling.yaml)"
    path: ./ruleset/WifiCalling.yaml
    interval: 86400 # 规则更新周期，单位为秒，86400秒为一天
```

**第二步：在 `rules` 部分引用规则集**

将规则集引用添加到您的 `rules:` 列表的靠前位置，并指定其走您的代理策略组。

```yaml
# config.yaml

rules:
  # ... 其他直连或广告屏蔽规则 ...

  - RULE-SET,WifiCalling,[你的代理策略组]

  # ... 其他代理规则 ...
  - MATCH,DIRECT
```
> **注意**：请将 `[你的代理策略组]` 替换为您实际使用的策略组名称，例如 `Proxy`, `Global`, `境外流量` 等。

**第三步**：重新加载 Clash 配置即可生效。

---

### 2. Surge 用户

可以直接在规则段引用本仓库的 `yaml` 文件。

```ini
# Surge 配置文件的 [Rule] 部分

[Rule]
# ... 其他规则 ...

RULE-SET,[https://github.com/gujiangjiang/Wi-Fi-Calling-Ruleset-for-Clash/raw/refs/heads/main/Wi-Fi_Calling.yaml](https://github.com/gujiangjiang/Wi-Fi-Calling-Ruleset-for-Clash/raw/refs/heads/main/Wi-Fi_Calling.yaml),[你的代理策略组]

# ... 其他规则 ...
FINAL,DIRECT
```
> **注意**：请将 `[你的代理策略组]` 替换为您实际使用的策略组名称。

---

### 3. Quantumult X 用户

需要使用 `resource_parser` 来让 QX 识别 Clash 规则。

**第一步：在 `[server_remote]` 中添加节点订阅（如果您还没有的话）**

**第二步：在 `[filter_remote]` 中引用规则集**

```ini
[filter_remote]
# ... 其他分流规则 ...

[https://github.com/gujiangjiang/Wi-Fi-Calling-Ruleset-for-Clash/raw/refs/heads/main/Wi-Fi_Calling.yaml](https://github.com/gujiangjiang/Wi-Fi-Calling-Ruleset-for-Clash/raw/refs/heads/main/Wi-Fi_Calling.yaml), tag=Wi-Fi Calling, force-policy=[你的代理策略组], update-interval=86400
```

**第三步**：保存配置后，在 Quantumult X 主界面，点击右下角风车图标，在“分流”中找到并启用 “Wi-Fi Calling”。

## 🤝 贡献

欢迎您通过以下方式参与项目贡献：
* **提交 Issue**：如果您发现某个运营商的 Wi-Fi Calling 无法使用，或某条规则已失效，请提交 Issue。
* **发起 Pull Request**：如果您有新的、经过测试的规则，欢迎直接发起 PR。

提交时，请尽可能提供 **运营商名称** 和 **所在国家/地区**。

## ⚠️ 免责声明

* 本项目仅供网络技术学习和研究使用。
* 使用者应自行承担因使用本规则集而产生的一切结果和责任。
* 请严格遵守您所在地区以及服务器所在地的相关法律法规。

## 📄 许可证

本项目采用 [MIT License](LICENSE) 许可协议。
