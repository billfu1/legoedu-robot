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

## 第一步：购买独立域名

1. 选择域名注册商（任选其一，均为网页操作、支持支付宝/微信）：
   - **阿里云万网**：`wanwang.aliyun.com`
   - **腾讯云 DNSPod**：`dnspod.cloud.tencent.com`
   - **Cloudflare**：`cloudflare.com`（海外，后缀选择多）
2. 搜索并挑选后缀，建议优先级：
   - `.com.cn`（国内机构常用，约几十元/年）
   - `.com`（国际通用，约几十到上百元/年）
   - 示例命名：`lezhijiang.com.cn`、`lezj-edu.com`、`lezjrobot.com`（以实际可注册为准）。
3. 按页面提示完成**实名认证**（个人或学校机构名义均可；`.com.cn` 国内后缀必须实名）。
4. 买好后，进入该域名的 **DNS 解析 / 解析设置** 后台备用。

> 费用提示：域名按年续费，只要不停费、不注销，就一直属于你；GitHub Pages 托管本身免费。

---

## 第二步：创建 GitHub 仓库并上传文件（无需安装命令行）

1. 登录你的 GitHub 账号，打开 <https://github.com/new>。
2. 填写仓库信息：
   - **Repository name（仓库名）**：建议 `lezhijiang-school`（或 `lezj-edu`）。
   - **Public（公开）**：必须选 Public，免费 Pages 仅支持公开仓库。
   - 勾选 **Add a README file** 可勾可不勾。
   - 点击 **Create repository**。
3. 在仓库页面点击 **Add file → Upload files**。
4. 把本 `deploy/` 目录里的**全部内容**（`index.html`、`assets` 整个文件夹、`CNAME`、`.nojekyll`）拖入上传区。
   - 注意：`assets` 是文件夹，要保持目录结构，不要把里面的图片平铺到根目录。
5. 页面底部填写提交信息（如“首次上传学校官网”），点击 **Commit changes**。

> 上传完成后，仓库根目录应能看到：`index.html`、`CNAME`、`.nojekyll`、`assets/` 文件夹。

### 备选：安装 Git 命令行上传（以后更新更方便）

本机当前未安装 Git。如需命令行方式，先安装 Git for Windows，再在本目录执行（把 `用户名`、`仓库名` 替换成你自己的）：

```bash
git init
git add .
git commit -m "首次上传学校官网"
git branch -M main
git remote add origin https://github.com/你的用户名/仓库名.git
git push -u origin main
```

以后更新网站：替换本地文件后执行 `git add . && git commit -m "更新内容" && git push`。

---

## 第三步：开启 GitHub Pages

1. 进入仓库顶部 **Settings → 左侧 Pages**。
2. **Branch** 选择 `main`，目录保持 `/(root)`，点击 **Save**。
3. 等待 1–3 分钟，页面顶部会出现站点地址：
   `https://你的用户名.github.io/仓库名/`
4. 先用这个临时地址访问一次，确认页面、图片、导航和问答窗口都正常。

---

## 第四步：绑定你的独立域名

### 4.1 修改仓库里的 CNAME 文件

1. 在仓库文件列表中点击 `CNAME`，点右上角铅笔图标编辑。
2. 把内容改成你要对外使用的主域名（**不要带 https://，不要带路径**），例如：

   ```text
   www.lezhijiang.com.cn
   ```

   如果你想用不带 `www` 的裸域作为主域名，则写：

   ```text
   lezhijiang.com.cn
   ```

3. 提交保存（Commit）。

### 4.2 在域名服务商后台添加 DNS 记录

以主域名使用 **`www.你的域名`** 为例（推荐，HTTPS 最稳妥）：

| 记录类型 | 主机记录 | 记录值 |
| --- | --- | --- |
| CNAME | `www` | `你的用户名.github.io` |

> CNAME 记录值只填 `用户名.github.io`，**不要**带仓库名、不要带 `https://`。

如果你同时希望裸域 `你的域名` 也能访问，再为裸域（主机记录填 `@`）添加四条 **A 记录**：

| 记录类型 | 主机记录 | 记录值 |
| --- | --- | --- |
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |

> IPv6（可选）AAAA 记录：`@` → `2606:50c0:8000::153` / `2606:50c0:8001::153` / `2606:50c0:8002::153` / `2606:50c0:8003::153`。

### 4.3 在 GitHub Pages 里填写自定义域名

1. 回到仓库 **Settings → Pages**。
2. **Custom domain** 输入和 CNAME 里一致的域名（如 `www.lezhijiang.com.cn`），点 **Save**。
3. GitHub 会自动检测 DNS 配置是否生效；刚填完可能显示未配置，属正常。

---

## 第五步：开启强制 HTTPS

1. DNS 记录生效（一般 10 分钟到几小时，最长 24 小时）后，**Settings → Pages** 里的 **Enforce HTTPS** 复选框变为可选。
2. 勾选 **Enforce HTTPS**，强制所有访问走 `https://`。
3. 浏览器打开你的域名，确认地址栏是小锁图标、页面正常。

### 本机快速检查 DNS 是否生效（PowerShell）

```powershell
Resolve-DnsName www.lezhijiang.com.cn
```

返回结果指向 `你的用户名.github.io` 的 CNAME 链，即表示解析已生效。

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
A：DNS 尚未生效，等待 10 分钟至 24 小时；确认 CNAME 值只写 `用户名.github.io`，A 记录四条都已添加；删除服务商自动生成的默认停放记录。

**Q：Enforce HTTPS 是灰色不可点？**
A：域名解析尚未被 GitHub 识别，先访问一次 `https://你的域名`，等待证书签发完成（最长 24 小时）后再勾选。

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
