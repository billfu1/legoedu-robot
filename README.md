# 乐之疆教育学校官网 · 上线部署指南

本目录 `deploy/` 就是要上传到 GitHub 的完整网站文件：

| 文件 / 目录 | 作用 |
| --- | --- |
| `index.html` | 网站主页（单文件，样式与脚本全部内联） |
| `assets/` | 校徽、标识、课程与教室配图（相对路径引用，必须整体上传） |
| `CNAME` | 自定义域名配置文件，上线前改成你买好的域名 |
| `.nojekyll` | 告诉 GitHub 不要用 Jekyll 处理静态文件，保留现有目录结构 |

> 本地预览：直接双击 `index.html` 即可在浏览器打开；不要只移动 HTML 文件，`assets/` 必须和它放在同一级目录。

---

## 总体方案

- **托管**：GitHub Pages（免费、长期稳定，站点文件存在你自己的 GitHub 账号下，随时可改、可导出）。
- **网址**：自己购买一个独立域名（域名归你所有，续费即可长期使用，最接近“永久”），绑定到 GitHub Pages。
- 完整链路：购买域名 → 创建 GitHub 仓库并上传本目录文件 → 开启 Pages → 在域名服务商后台填 DNS 记录 → 开启 HTTPS。

> GitHub Pages 默认免费地址形如 `https://你的用户名.github.io/仓库名/`；绑定独立域名后，访问你自己的域名即可。

---

## 第一步：购买独立域名（已完成）

- **域名**：`legoedu-robot.com`，注册商阿里云万网，2026-11-02 注册，到期 2027-11-02。
- **实名认证进度**：阿里云账号企业认证已于 2026-11-02 通过（主体：天津市河西区乐之疆教育培训学校）；机构信息模板（民办非企业单位登记证书）仍在注册局实名审核中，审核通过前域名处于 hold 锁定、暂不能对外解析，等待短信通知即可。
- 域名按年续费，只要不停费、不注销，就一直属于学校；GitHub Pages 托管本身免费。

> 若以后需要另购域名，注册商可选阿里云万网 `wanwang.aliyun.com`、腾讯云 DNSPod、Cloudflare；`.com` 国内注册必须先完成实名认证信息模板。

---

## 第二步：创建 GitHub 仓库并上传文件（已完成）

- **仓库**：<https://github.com/billfu1/legoedu-robot>（Public，`main` 分支根目录）。
- **临时站点**：<https://billfu1.github.io/legoedu-robot/>（仓库绑定自定义域名后，该临时地址会自动 301 跳转到独立域名；域名未生效前可能暂时无法从临时地址打开，属正常现象）。
- 仓库根目录现有：`index.html`（72563 字节）、`README.md`、`.nojekyll`、`CNAME`、`assets/`（7 张素材）。

### 以后更新网站（网页端，无需安装命令行）

1. 修改本地 `index.html`（或替换 `assets/` 里的图片）。
2. 仓库中 **Add file → Upload files**，覆盖同名文件后 Commit。
3. GitHub Pages 重新发布通常 1–2 分钟内生效；看到旧页面按 `Ctrl + F5` 强制刷新。

### 备选：安装 Git 命令行上传

本机当前未安装 Git。如需命令行方式，先安装 Git for Windows，再在本目录执行：

```bash
git init
git add .
git commit -m "首次上传学校官网"
git branch -M main
git remote add origin https://github.com/billfu1/legoedu-robot.git
git push -u origin main
```

以后更新：`git add . && git commit -m "更新内容" && git push`。

---

## 第三步：开启 GitHub Pages（已完成）

- 仓库 **Settings → Pages**：Source = Deploy from a branch，Branch = `main`，Folder = `/(root)`，Save。
- 临时站点 <https://billfu1.github.io/legoedu-robot/> 已实测首页与 7 张素材全部 HTTP 200。

---

## 第四步：绑定独立域名（DNS 记录与 CNAME 已完成，等待解析对外生效）

主域名使用**裸域** `legoedu-robot.com`，`www.legoedu-robot.com` 作为别名指向 GitHub Pages。

### 4.1 阿里云云解析 DNS 添加记录（已完成）

进入阿里云控制台 → 域名与网站 → 云解析 DNS → `legoedu-robot.com` → 解析设置，以下 5 条记录已全部添加并处于启用状态（TTL 10 分钟、默认线路）：

| 记录类型 | 主机记录 | 记录值 | 状态 |
| --- | --- | --- | --- |
| A | `@` | `185.199.108.153` | 已启用 |
| A | `@` | `185.199.109.153` | 已启用 |
| A | `@` | `185.199.110.153` | 已启用 |
| A | `@` | `185.199.111.153` | 已启用 |
| CNAME | `www` | `billfu1.github.io`（不带仓库名、不带 https://） | 已启用 |

> IPv6（可选）AAAA 记录，主机记录 `@`：`2606:50c0:8000::153`、`2606:50c0:8001::153`、`2606:50c0:8002::153`、`2606:50c0:8003::153`。当前未添加，不影响普通 IPv4 访问。
> 不要使用通配符 DNS；除上述记录外无需保留其他占位记录。

### 4.2 仓库根目录 CNAME 文件（已完成）

仓库根目录 `CNAME` 文件已提交，内容只写一行主域名（不带 `https://`、不带路径、不带末尾斜杠）：

```text
legoedu-robot.com
```

### 4.3 GitHub Pages 自定义域名（已保存，等待 DNS 校验通过）

1. 仓库 **Settings → Pages → Custom domain** 已填入 `legoedu-robot.com` 并保存。
2. 当前页面显示红色 **DNS check unsuccessful / InvalidDNSError**，是因为域名仍在 hold、对外 DNS 尚未生效，属正常现象，不是配置错误。
3. 域名实名审核通过、解析扩散后，回到本页点 **Check again**，校验结果会转为正常（绿色）。

---

## 第五步：开启强制 HTTPS（等待 DNS 校验通过后执行）

当前 **Enforce HTTPS** 复选框为灰色不可选，提示域名未正确配置，属正常状态。

1. 域名实名审核通过、DNS 记录对外生效（一般 10 分钟到几小时，最长 24 小时）后，先在 **Settings → Pages** 点 **Check again**，待 DNS 校验转绿、GitHub 自动签发 HTTPS 证书。
2. 证书就绪后 **Enforce HTTPS** 复选框变为可选，勾选它，强制所有访问走 `https://`。
3. 浏览器打开 `https://legoedu-robot.com`，确认地址栏是小锁图标、页面正常。

### 本机快速检查 DNS 是否生效（PowerShell）

```powershell
Resolve-DnsName legoedu-robot.com
Resolve-DnsName www.legoedu-robot.com
```

裸域返回四条 GitHub Pages A 记录、`www` 返回指向 `billfu1.github.io` 的 CNAME 链，即表示解析已生效。

---

## 日常更新网站内容

1. 修改本地 `index.html`（或替换 `assets/` 里的图片）。
2. 网页端：仓库中 **Add file → Upload files**，覆盖同名文件后 Commit。
3. 命令行：`git add . && git commit -m "说明本次修改" && git push`。
4. GitHub Pages 重新发布通常在 1–2 分钟内生效；若看到旧页面，按 `Ctrl + F5` 强制刷新。

### 常见修改位置对照

- 校徽 / 横版标识：替换 `assets/logo-badge.jpg`、`assets/logo-wordmark.jpg`（保持文件名不变即可，图片自动更新）。
- 新闻文章：编辑 `index.html` 内 `ARTICLES` 数组。
- 问答库：编辑 `index.html` 内 `FAQ` 数组（`q` 问题、`a` 答案、`k` 触发关键词），底部聊天窗口与上方问答专栏共用同一数据。
- 联系信息、地址、营业时间：在 `index.html` 联系区块内直接改文字。

---

## 常见问题

**Q：打开域名显示 404 / 图片裂图？**
A：检查仓库根目录是否有 `index.html` 与 `assets/` 文件夹；`assets` 内图片路径必须是 `assets/xxx.jpg`；确认 Pages 的 Branch 选的是 `main` 根目录。

**Q：自定义域名填完显示 DNS Error？**
A：DNS 尚未生效，等待 10 分钟至 24 小时；确认 `www` 的 CNAME 值只写 `billfu1.github.io`，裸域四条 A 记录都已添加；删除服务商自动生成的默认停放记录。

**Q：Enforce HTTPS 是灰色不可点？**
A：域名解析尚未被 GitHub 识别，先访问一次 `https://legoedu-robot.com`，等待证书签发完成（最长 24 小时）后再勾选。

**Q：国内访问 GitHub Pages 速度不稳定？**
A：GitHub Pages 服务器在境外，国内网络高峰期可能偏慢。备选方案：
  - **Gitee Pages（码云）**：国内访问快，流程与 GitHub 类似，需实名认证；
  - **Vercel / Netlify**：同样免费静态托管，全球 CDN；
  - **国内云服务器 + 备案**：面向国内长期招生使用、对打开速度要求高时，最稳妥，需按规定完成 ICP 备案后再绑定域名。

**Q：需要 ICP 备案吗？**
A：网站托管在 GitHub Pages（境外服务器）时，国内法规层面不强制备案；若后续迁移到国内服务器或使用国内 CDN，则必须先完成 ICP 备案才能绑定国内域名对外访问。页面页脚预留了备案号占位，备案完成后填写实际备案号。

---

## 事实与内容边界（上线前请核对）

- 站点中的评分（4.8）、评价条数（473）、团购价格、地址与营业时间来自大众点评店铺公开页面；正式上线前请与校区当期公示再核对一遍。
- 预约表单为**纯前端演示**，不会把信息发送到服务器；正式使用时需对接后端表单接口（如腾讯问卷、金数据、表单大师，或学校自有系统），或引导家长通过大众点评 / 电话预约。
- 完整课程价目、师资名单、课表未在公开页面披露，页面已按“以到校测评 / 当期课表为准”措辞处理，未虚构数字。
