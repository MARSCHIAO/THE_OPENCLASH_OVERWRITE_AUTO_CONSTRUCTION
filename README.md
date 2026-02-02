
<div align="center">

<h1>OpenClash Config Builder</h1>

<p>
  <b>自动化覆写配置生成器</b><br>
  从上游 YAML 智能提取 → 精简处理 → 动态生成 .conf 覆写文件
</p>

<p>
  <a href="https://github.com/YOUR_USERNAME/YOUR_REPO/actions/workflows/build.yml">
    <img src="https://img.shields.io/badge/构建状态-自动化-blue?logo=githubactions&logoColor=white" alt="Build">
  </a>
  <img src="https://img.shields.io/badge/同步-每日自动-green?logo=clockify" alt="Schedule">
  <img src="https://img.shields.io/badge/核心-Smart/Metacubex-orange?logo=linux" alt="Core">
  <img src="https://img.shields.io/badge/许可-GPL--3.0-red" alt="License">
</p>

<p>
  <a href="#快速开始">🚀 快速开始</a> •
  <a href="#项目结构">📂 项目结构</a> •
  <a href="#使用方法">📖 使用方法</a> •
  <a href="#在线文档">🌐 在线文档</a>
</p>

</div>

---

## ✨ 核心特性

<table>
<tr>
<td width="33%" valign="top">

<h3>🔄 自动同步</h3>
每日从 <a href="https://github.com/HenryChiao/mihomo_yamls">HenryChiao/mihomo_yamls</a> 拉取最新配置，自动合并上游更新

</td>
<td width="33%" valign="top">

<h3>✂️ 智能精简</h3>
仅保留 <code>proxy-providers</code>, <code>proxy-groups</code>, <code>rule-providers</code>, <code>rules</code> 及锚点，删除冗余字段

</td>
<td width="33%" valign="top">

<h3>🎯 动态变量</h3>
根据 provider 数量自动生成 <kbd>EN_KEY</kbd> / <kbd>EN_KEY1</kbd>...<kbd>EN_KEYN</kbd> 环境变量

</td>
</tr>
<tr>
<td valign="top">

<h3>🌐 三模式支持</h3>
<ul>
<li>主路由模式 (Url-test)</li>
<li>旁路由模式 (+EN_DNS)</li>
<li>Smart 智能模式</li>
</ul>

</td>
<td valign="top">

<h3>🏠 本地配置</h3>
通过 <code>cleaner_config/</code> 目录维护私有配置，与外部配置隔离处理

</td>
<td valign="top">

<h3>🤖 零人工干预</h3>
GitHub Actions 全自动构建，直接提交到仓库，支持在线编辑

</td>
</tr>
</table>

---

## 📂 项目结构

<details>
<summary><b>📁 点击展开完整目录结构</b></summary>

```

your-repo/
├── .github/workflows/
│   └── build.yml                 # GitHub Actions CI 配置
├── src/
│   ├── yaml_processor.py         # YAML 精简处理器
│   └── overwrite_generator.py    # 覆写文件生成器
├── templates/
│   ├── main.conf.j2              # 主路由模板
│   ├── bypass.conf.j2            # 旁路由模板
│   └── smart.conf.j2             # Smart 模式模板
├── cleaner_config/               # 本地配置源（手动维护）
├── processed_configs/            # 精简后的 YAML（自动提交）
│   ├── external/                 # 来自 HenryChiao/mihomo_yamls
│   └── local/                    # 来自 cleaner_config/
└── overwrite/                    # 🎯 生成的覆写文件（可直接编辑）
├── README.md
├── Overwrite-external-General_Config-HenryChiao-MihomoAIO-main.conf
├── Overwrite-external-General_Config-HenryChiao-MihomoAIO-bypass.conf
├── Overwrite-external-General_Config-HenryChiao-MihomoAIO-smart.conf
└── Overwrite-local-xxx-main.conf...

```

</details>

### 目录说明

| 目录 | 内容 | 更新方式 |
|------|------|----------|
| `cleaner_config/` | 本地 YAML 配置文件 | 手动编辑 |
| `processed_configs/external/` | 精简后的外部配置 | 自动（每日） |
| `processed_configs/local/` | 精简后的本地配置 | 自动（提交触发） |
| `overwrite/` | **生成的 .conf 覆写文件** | 自动（可直接编辑） |

---

## 🚀 快速开始

### 1️⃣ 选择配置文件

进入 <a href="./overwrite"><b>overwrite/</b></a> 目录，根据场景选择：

<table>
<tr>
<th>文件名后缀</th>
<th>适用场景</th>
<th>必需变量</th>
</tr>
<tr>
<td><code>-main.conf</code></td>
<td>主路由标准模式</td>
<td><kbd>EN_KEY</kbd> 或 <kbd>EN_KEY1~N</kbd></td>
</tr>
<tr>
<td><code>-bypass.conf</code></td>
<td>旁路由模式</td>
<td>上述变量 + <kbd>EN_DNS</kbd></td>
</tr>
<tr>
<td><code>-smart.conf</code></td>
<td>Smart 智能模式</td>
<td><kbd>EN_KEY</kbd> 或 <kbd>EN_KEY1~N</kbd></td>
</tr>
</table>

### 2️⃣ 配置环境变量

<details>
<summary><b>单 Provider 配置（1 个订阅）</b></summary>

在 OpenClash 全局设置中添加：

```

EN_KEY=https://your-subscription-url

```

</details>

<details>
<summary><b>多 Provider 配置（2+ 个订阅）</b></summary>

根据覆写文件中的 provider 数量设置：

```

EN_KEY1=https://sub1-url
EN_KEY2=https://sub2-url
EN_KEY3=https://sub3-url

```

</details>

<details>
<summary><b>旁路由额外变量（仅 bypass 模式）</b></summary>

```

EN_DNS=223.5.5.5,114.114.114.114

```

</details>

### 3️⃣ 应用覆写

<ol>
<li>在 OpenClash <b>覆写设置</b>中上传选择的 <code>.conf</code> 文件</li>
<li>或使用订阅方式：复制文件 Raw 链接</li>
<li>填入上述环境变量</li>
<li>保存并重启 OpenClash</li>
</ol>

---

## 🔧 工作流程

```

HenryChiao/mihomo_yamls (上游)
│
▼
┌─────────────────┐
│ 1. 同步 YAML    │  ← 每日 06:00 UTC 自动执行
│    (GitHub      │
│     Actions)    │
└────────┬────────┘
│
▼
┌─────────────────┐
│ 2. 精简处理     │  ← 提取核心字段，删除冗余配置
│    - 保留:      │
│      proxy-     │
│      providers  │
│      proxy-     │
│      groups     │
│      rule-      │
│      providers  │
│      rules      │
│      anchors    │
│    - 删除:      │
│      port, dns, │
│      tun...     │
└────────┬────────┘
│
▼
┌─────────────────┐
│ 3. 分析生成     │  ← 统计 Provider 数量
│    生成环境变量  │     渲染 Jinja2 模板
│    EN_KEY1N    │
└────────┬────────┘
│
▼
┌─────────────────┐
│ 4. 提交仓库     │  ← 自动 push 到 overwrite/
│                 │     可直接在线编辑
└─────────────────┘

```

---

## 📝 命名规则

| 文件名模式 | 来源 | 模式 | 变量示例 |
|------------|------|------|----------|
| `Overwrite-external-*-main.conf` | HenryChiao | 主路由 | `EN_KEY` |
| `Overwrite-external-*-bypass.conf` | HenryChiao | 旁路由 | `EN_KEY` + `EN_DNS` |
| `Overwrite-external-*-smart.conf` | HenryChiao | Smart | `EN_KEY1`, `EN_KEY2`... |
| `Overwrite-local-*-main.conf` | cleaner_config | 主路由 | 视配置而定 |

---

## 🛠️ 本地构建

如需本地运行：

```bash
# 1. 安装依赖
pip install pyyaml jinja2

# 2. 处理 YAML
python src/yaml_processor.py \
  --input cleaner_config/ \
  --output processed_configs/local \
  --recursive

# 3. 生成覆写
python src/overwrite_generator.py \
  --input processed_configs/local \
  --output overwrite/ \
  --types main bypass smart \
  --source local
```

<div align="center">
<p><b>如果这个项目对你有帮助，请给个 ⭐ Star！</b></p>
<p>
  <a href="https://github.com/vernesong/OpenClash">OpenClash</a> •
  <a href="https://github.com/HenryChiao/mihomo_yamls">mihomo_yamls</a> •
  <a href="https://wiki.metacubex.one/">Mihomo Wiki</a>
</p>
<p><sub>基于 GPL-3.0 许可证开源</sub></p>
</div>
