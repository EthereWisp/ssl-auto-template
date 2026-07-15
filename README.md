# CF SSL Auto Template

## Introduction
## 项目介绍

✨ This open-source template is built based on GitHub Actions + acme.sh + Cloudflare DNS API
本开源模板基于 GitHub Actions + acme.sh + Cloudflare DNS API 打造

✅ Fully automatic issuance of free Let's Encrypt ECC SSL certificates
全自动签发免费 Let's Encrypt 椭圆曲线加密SSL证书

✅ Scheduled monthly auto-renewal cycle (check every 30 days, skip if certificate is still valid)
按月定时自动续期（每30天检测一次，证书未到期自动跳过任务）

✅ Certificates auto saved to `ssl_certs` directory inside repository
证书文件自动持久保存至仓库 `ssl_certs` 文件夹

✅ Auto git commit to prevent GitHub Actions deactivation caused by long idle
自动提交仓库代码，规避 Actions 闲置60天被系统禁用问题

✅ No manual intervention after initial configuration, fully unattended operation
初始化配置完成后无需人工操作，全程无人值守运行

✅ Template repository support, one-click clone for all users
支持模板仓库，其他用户一键复刻完整项目环境

> Mandatory execution sequence (cannot be reversed)
> 强制执行使用顺序（不可颠倒）

1. Run first issuance workflow to initialize certificate cache
1. 先运行首次签发工作流，完成证书与缓存初始化

2. Enable scheduled auto-renew workflow for long-term use
2. 再启用定时续期工作流长期使用

---

## Preconditions
## 前置必备条件

1. Your domain names are fully hosted on Cloudflare DNS
你的域名已完整接入 Cloudflare，域名DNS解析托管于 Cloudflare

2. Prepare Cloudflare API Token & Zone ID with DNS edit permission
准备拥有DNS编辑权限的 Cloudflare API 令牌、域名区域ID

3. Create two repository Secrets for Actions environment variables
在仓库 Actions 密钥库创建两组密钥环境变量

- `CF_TOKEN` — Cloudflare API Token
- `CF_ZONE_ID` — Cloudflare Domain Zone ID

---

## Step 1: Get Cloudflare Credentials
## 步骤一：获取 Cloudflare 密钥信息

### 1. Get Zone ID
### 1. 获取 Zone ID

1. Log in Cloudflare official dashboard, select your target domain
登录 Cloudflare 后台，进入你需要申请证书的域名主页

2. Check the right sidebar of overview page, copy the Zone ID string
在概览页面右侧栏直接复制 Zone ID 字符串

3. Save this value for later Secrets configuration
保存该字符串，后续填入仓库密钥

### 2. Create Cloudflare API Token
### 2. 创建 Cloudflare API Token

1. Click avatar in top-right corner → `My Profile` → `API Tokens`
点击右上角头像 → 个人资料 → API令牌

2. Click `Create Token`, select template `Edit zone DNS`
点击创建令牌，选择模板：编辑区域DNS

3. Zone Resources: Select your target domain under Include list
区域权限：仅勾选你操作的域名

4. Keep other default settings, generate token and copy the full key
其余配置保持默认，生成令牌并完整复制

5. Store token safely, it only shows once
妥善保存令牌，页面仅展示一次，刷新后无法查看

---

## Step 2: Configure Repository Secrets
## 步骤二：配置仓库密钥

1. Enter your cloned template repository → `Settings` → `Secrets and variables` → `Actions`
进入复刻后的模板仓库 → 设置 → 密钥与变量 → Actions

2. Click `New repository secret` to create two items
点击新建仓库密钥，创建两组变量

| Secret Name | Fill Content |
| ---- | ---- |
| CF_TOKEN | Your copied Cloudflare API Token |
| CF_ZONE_ID | Your copied Cloudflare Zone ID |

---

## Step 3: Official Usage Guide
## 步骤三：标准使用流程

### Phase 1: First Certificate Initial Issuance (MANDATORY FIRST)
### 阶段1：首次证书初始化签发（必须优先执行）

1. Switch to `Actions` tab, select workflow `Step 1: First SSL Certificate Initial Issuance`
切换至 Actions 面板，选择工作流：第一步 首次证书初始化签发

2. Click `Run workflow`, modify the default domain input box
点击运行工作流，修改弹窗内默认域名
   Format example: `blog.domain.com *.domain.com`
   填写格式示例：`blog.domain.com *.domain.com`

3. Confirm and start the task, wait all steps turn green
确认并启动任务，等待所有步骤绿色成功

4. After execution, folder `ssl_certs` will appear with full certificate files
执行完成后仓库自动生成 ssl_certs 证书文件夹

### Phase 2: Enable Monthly Auto Renewal
### 阶段2：开启每月自动续期

1. Confirm Phase 1 workflow finished without error
确认第一阶段签发流程无报错完成

2. Select workflow `Step 2: Cached scheduled automatic certificate renewal`
选择工作流：第二步 带缓存定时自动续期

3. The workflow runs automatically on the 1st day of every month (UTC 02:00)
该工作流每月1号 UTC 凌晨2点自动执行检测续期

4. You can also trigger manual renewal at any time via `Run workflow`
你也可以随时手动点击运行，即时执行续期检测

---

## Certificate File Description
## 证书文件说明

All generated certificates are stored in `./ssl_certs/`
所有生成证书统一存放于仓库 `./ssl_certs/` 目录

- `fullchain.pem` — Full certificate chain, used for website HTTPS deployment
  完整证书链，网站HTTPS部署通用文件

- `private.key` — Domain private key, keep confidential
  域名私钥，请勿对外泄露

- `ca.cer` — Root CA certificate
  根证书文件

- `domain.cer` — Domain single certificate
  域名单证书

---

## Common Error Troubleshooting
## 常见报错排查

1. API request failed / DNS record create error
API请求失败 / 无法创建DNS解析记录

   - Check if `CF_TOKEN` and `CF_ZONE_ID` are filled correctly
   检查 CF_TOKEN、CF_ZONE_ID 密钥是否填写无误

   - Confirm API Token has Edit DNS permission for your domain
   确认API令牌拥有对应域名的DNS编辑权限

2. Certificate issuance timeout
证书签发校验超时

   - Verify your domain's DNS server is fully switched to Cloudflare
   确认域名DNS服务器已完整切换为Cloudflare官方NS

3. Workflow skipped every month
每月任务直接跳过无操作

   - This is normal behavior: certificate still within 90-day validity
   属于正常现象：当前证书仍在90天有效期内，无需续期

---

## Our Bilibili Channel
## 作者哔哩哔哩账号

Check our channel for related tutorials and updates:
更多使用教程与项目动态，欢迎关注作者哔哩哔哩账号：

https://space.bilibili.com/178329117
