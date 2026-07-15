# CF SSL Auto Template

## Introduction
## 项目介绍

This template uses GitHub Actions and Cloudflare DNS API to implement free Let's Encrypt ECC certificate issuance and monthly automatic renewal.
本模板使用GitHub Actions与Cloudflare DNS接口，实现免费Let's Encrypt椭圆曲线证书签发与每月自动续期。

Certificates will be saved into ssl_certs folder automatically. Auto commit solves GitHub Actions idle disable problem.
证书自动保存至ssl_certs文件夹，自动提交解决GitHub Actions长期闲置被停用问题。

Mandatory execution order: Run first issuance workflow first, then enable scheduled renewal workflow.
强制执行顺序：先运行首次签发工作流，再启用定时续期工作流。

## Preconditions
## 前置条件

1. Your domain has been successfully imported to Cloudflare, DNS records are hosted by Cloudflare.
你的域名已经成功接入Cloudflare，DNS解析托管在Cloudflare内。

2. Create Cloudflare API Token with Edit zone DNS permission, copy your Zone ID.
创建拥有编辑DNS权限的Cloudflare API令牌，复制域名区域ID。

3. Store two secrets in repository Settings > Secrets and variables > Actions
在仓库设置的Actions密钥中填入两项内容

   - `CF_TOKEN` — Cloudflare API Token
   - `CF_ZONE_ID` — Cloudflare Domain Zone ID

## Usage Steps
## 使用步骤

### Step 1: Execute First SSL Certificate Initial Issuance
### 第一步：执行SSL证书首次初始化签发

1. Open Actions tab, select workflow `Step 1: First SSL Certificate Initial Issuance`
   打开Actions面板，选择第一步首次签发工作流

2. Click `Run workflow`, input your own domain list
   点击运行工作流，填写你自己的域名列表

3. Wait workflow finished, `ssl_certs` folder will be generated automatically
   等待流程执行完毕，仓库会自动生成ssl_certs证书目录

### Step 2: Enable scheduled auto renewal
### 第二步：开启定时自动续期

1. Confirm the first workflow completed successfully
   确认首次签发流程执行成功

2. Use Step 2 workflow for monthly automatic certificate check and renewal
   使用第二步工作流实现每月自动检测并更新证书

## Certificate Files
## 证书文件说明

| File | Description |
|------|-------------|
| `ssl_certs/fullchain.pem` | Full certificate chain (server + CA) / 完整证书链（服务器证书+CA证书） |
| `ssl_certs/private.key` | Private key / 私钥 |
| `ssl_certs/ca.cer` | CA intermediate certificate / CA中间证书 |
| `ssl_certs/domain.cer` | Domain certificate / 域名证书 |

## License
## 开源协议

MIT License
