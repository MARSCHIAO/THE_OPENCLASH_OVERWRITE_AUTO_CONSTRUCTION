# OpenClash 覆写配置文件

自动生成于: $(date +'%Y-%m-%d %H:%M:%S UTC')

## 📋 配置文件列表
- [*.conf](*.conf)

## 🚀 使用方法

### 1. 添加覆写模块
在 OpenClash 中添加覆写模块，使用以下格式的 URL:

```
https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/output/文件名.conf
```

### 2. 配置环境变量
- 单订阅: EN_KEY=机场订阅链接
- 多订阅: EN_KEY1=订阅1;EN_KEY2=订阅2;...
- 旁路由: 额外需要 EN_DNS=DNS服务器地址

### 3. 应用配置
保存并重启 OpenClash。

## 📝 配置类型说明
| 后缀 | 说明 |
|------|------|
| (无) | 主路由标准配置 |
| -bypass_router | 旁路由配置 |
| -smart | Smart 智能模式 |

## ⚠️ 注意事项
1. 所有配置会自动下载对应的精简 YAML
2. 精简后的 YAML 只包含 providers 与 rules
3. 根据 provider 数量自动生成环境变量要求

