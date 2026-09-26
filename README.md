# ⚡ 魔方云系统 · 理论永久免费云端授权

> Cloudflare Workers 实现的智简魔方（ZJMF Cloud）授权 API
> 无需自己搭服务器，永久免费对接，不限调用次数

---

## 🚀 一键部署（最简单，点一下就好）

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/FenhaoLost/zjmf)

点击上面的按钮 → 用 Cloudflare 账号登录 → 确认部署 → 搞定 ✅

部署完成后你会得到一个 Worker 地址，格式类似：
```
https://zjmf-auth-xxx.<你的Workers子域>.workers.dev
```

## 🔧 方式 B：GitHub Actions 自动部署

如果你想 push 代码自动部署，配置两个 GitHub Secrets 就行：

1. **CLOUDFLARE_API_TOKEN** — Cloudflare Dashboard → My Profile → API Tokens → Create Token → 选 "Edit Cloudflare Workers" 模板
2. **CLOUDFLARE_ACCOUNT_ID** — Dashboard 右上角账户菜单 → 复制 Account ID

配置位置：仓库 **Settings → Secrets and variables → Actions → New repository secret**

push 到 `main` 分支后会自动触发 `.github/workflows/deploy.yml`

---

## 📦 使用方法（3 步）

```bash
# 1. 下载官方安装脚本
wget https://raw.githubusercontent.com/aazooo/zjmf/main/install-zjmf-cloud_new -O install-zjmf-cloud_new
chmod +x install-zjmf-cloud_new

# 2. Patch 硬编码 IP 为你的 Workers 地址（域名长度 ≤ 13 字符）
python3 patch/patch_installer.py install-zjmf-cloud_new "你的workers域名短名"

# 3. 正常运行安装
./install-zjmf-cloud_new -nokernel -norepo -l
```

## 📁 项目结构

```
zjmf/
├── src/index.ts                   # Cloudflare Workers 授权 API（15 个端点）
├── patch/patch_installer.py       # ELF patch 脚本（替换硬编码 IP）
├── install-zjmf-cloud_new.go      # 逆向的 Go 安装程序源码
├── auth_php/                      # 原始 PHP 授权站源码
├── wrangler.toml                  # Workers 部署配置
├── package.json                   # npm 依赖
└── .github/workflows/deploy.yml   # CI/CD 自动部署
```

## 🔐 授权端点清单

| 端点 | 用途 |
|------|------|
| `/app/api/auth` | 基础授权检查 |
| `/app/api/toggle_version` | 版本切换授权 |
| `/app/api/auth_update` | 授权更新 |
| `/app/api/auth_complete` | 授权完成 |
| `/app/api/auth_rc` | 业务 V10 授权 |
| `/app/api/auth_rc_plugin` | RC 插件授权 |
| `/app/api/auth_image_download` | 镜像下载授权 |
| `/app/api/ip` | 获取客户端 IP |
| `/app/api/get_new_version` | 检查新版本 |
| `/app/api/get_version` | 获取版本信息 |
| `/app/api/get_image_version` | 镜像版本列表 |
| `/app/api/get_images` | 镜像列表 |
| `/market/index` | 应用市场 |
| `/api/auth/version` | v1 版本检查 |
| `/api/auth/check` | v1 授权验证 |

## 🛡️ 安全说明

- Go 安装程序端**不验签**，只检查 `status=200` 和 `professional=true`
- RSA 加密是给部署后运行时的 ZJMF PHP 面板用的
- 所有下载走 HTTP 明文（官方设计如此），注意网络环境

## 📜 许可证

仅用于学习研究，请勿用于商业用途。
