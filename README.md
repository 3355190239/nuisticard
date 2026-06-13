# ⚡ NUIST iCard Electricity Notifier | 南信大宿舍电费自动推送助手

基于 Python 的南京信息工程大学 (NUIST) 宿舍电费自动查询与推送脚本。通过模拟登录校园统一身份认证系统 (CAS)，抓取一卡通大厅接口数据，实现每日定时查询电费，并通过微信、iOS 等多种渠道将结果自动推送到你的手机上。

---

## ✨ 核心特性

* 🔐 **CAS 全自动认证**：内置前端 `encrypt.js` 的 AES 逆向加密算法，自动换取全局 TGC 通行证，实现免密访问下游业务。
* 🖼️ **本地 OCR 验证码识别**：无缝集成 `ddddocr`，全自动光速识别登录验证码，无需人工干预或第三方打码平台。
* 💰 **电费精准查询**：直接对接南信大一卡通业务接口，可灵活配置抓取指定校区、指定楼栋、具体宿舍的电量数据。
* 📢 **多渠道消息触达**：
    * [PushPlus](https://www.pushplus.plus/) (微信公众号免签推送)
    * [Bark](https://github.com/Finb/Bark) (iOS 专属系统级无缝推送)
    * [Server酱](https://sct.ftqq.com/) (全平台跨生态支持)
* ☁️ **完美适配云端部署**：配置参数全部抽离为环境变量 (Env) 注入，完美适配 GitHub Actions 或各类 Serverless 云函数，实现零成本自动化挂机。

---

## 🚀 快速开始

### 1. 环境准备

确保你的设备上安装了 Python 3.8+。将本项目克隆到本地后，安装必需的依赖包：

```bash
pip install requests ddddocr beautifulsoup4 pycryptodome

```

### 2. 配置宿舍参数 (关键步骤)

打开主程序文件，定位到代码底部的 `icard_data` 字典。你需要将这里的参数替换为你自己宿舍的专属 ID：

```python
icard_data = {
    "type": "IEC",
    "level": "3",                        # 替换
    "feeitemid": "448",                  # 替换为你的校区参数
    "xiaoyu_id": "3&沁园",                # 替换为你的小区参数
    "loudong_id": "23&沁园30号栋",         # 替换为你的楼栋参数
    "room_id": "4186&135"                 # 替换为你的房间号参数
}

```

> **💡 抓包提示：** 建议在电脑浏览器登录[南信大一卡通网页版](https://icard.nuist.edu.cn)，按 `F12` 打开开发者工具，进入“网络 (Network)”面板。在页面上手动查询一次电费，查看抓取到的请求负载 (Payload)，即可获取上述专属 ID。

### 3. 配置账号与通知密钥

系统采用环境变量来保护你的账号隐私。请提供以下参数配置（可通过环境变量导出，或仅供本地测试时直接填入代码底部的 `os.getenv` 默认值中）：

| 变量名 | 说明 | 必填状态 |
| --- | --- | --- |
| `NUIST_USER` | 南信大统一身份认证账号 (学号) | ✅ 必填 |
| `NUIST_PWD` | 统一身份认证密码 | ✅ 必填 |
| `PUSHPLUS_TOKEN` | PushPlus 的 Token (微信接收) | 选填 |
| `BARK_KEY` | Bark 的专属 URL Key (iOS 接收) | 选填 |
| `SERVERCHAN_KEY` | Server酱 SendKey | 选填 |

*本地运行测试示例 (Linux/macOS):*

```bash
export NUIST_USER="2024xxxxxxxx"
export NUIST_PWD="YourPassword"
export PUSHPLUS_TOKEN="YourPushPlusToken"
python main.py

```


## ⚠️ 免责声明

1. 本项目仅供学习 Python 爬虫接口调用、密码学逆向与自动化脚本参考，**严禁用于任何商业用途或对校园服务器发起恶意高频攻击**。
2. 验证码识别模块 `ddddocr` 为纯本地离线执行，用户的账号与密码仅在与南信大官方服务器 (authserver.nuist.edu.cn) 通信时使用，**本项目不收集、不存储、不上传任何个人隐私数据**。
3. 使用本项目及相关代码所带来的风险和后果由使用者自行承担。

```

```
