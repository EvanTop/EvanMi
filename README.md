<div style="display: flex; align-items: center; gap: 10px;">
    <a href="https://evan.xin" target="_blank">
        <img src="https://img.shields.io/badge/Blog-Evan's%20Space-black?logo=blog&color=red&style=flat" alt="Blog-Evan's Space" style="display: inline-block;">
    </a>
    <a href="https://github.com/EvanTop/EvanMi" target="_blank">
        <img src="https://img.shields.io/github/repo-size/EvanTop/EvanMi?style=flat" alt="Repo Size" style="display: inline-block;">
    </a>
    <a href="https://github.com/EvanTop/EvanMi/stargazers" target="_blank">
        <img src="https://img.shields.io/github/stars/EvanTop/EvanMi?style=flat" alt="Stars" style="display: inline-block;">
    </a>
    <a href="https://github.com/EvanTop/EvanMi/releases" target="_blank">
        <img src="https://img.shields.io/github/downloads/EvanTop/EvanMi/total?style=flat" alt="GitHub All Releases" style="display: inline-block;">
    </a>
</div>

# EvanMi：把域名清单做成一个“好看又好用”的米表

- [详情页下载](https://github.com/EvanTop/EvanMi/releases/edit/EvanMi_V9)
- [EvanMi.zip](https://github.com/user-attachments/files/26330773/EvanMi.zip)

<img width="1123" height="1123" alt="1" src="https://github.com/user-attachments/assets/18dd54fd-c8a3-40be-8a04-14c09188f052" />
<img width="1123" height="1123" alt="2" src="https://github.com/user-attachments/assets/765c7432-a6c9-4ce2-a1bb-f1be84038287" />
<img width="1151" height="1100" alt="11" src="https://github.com/user-attachments/assets/1ec2a4d4-371b-443a-b510-1e0fa613abd5" />
<img width="1151" height="1100" alt="22" src="https://github.com/user-attachments/assets/93690c28-875c-4058-bab7-d693f73c4b37" />



如果你手里有一批域名，想要一个页面：

- 看起来体面
- 别人一眼就能搜到、筛到
- 自己更新也方便

那 EvanMi 就是为这件事准备的。


## 适合谁？

- 有域名要展示、要报价、要整理的人
- 不想折腾数据库、登录系统、复杂功能的人
- 想要“好看 + 好用 + 快”的人

---

## 一句话总结

**EvanMi = 一个好看的展示页 + 一个够用的管理后台。**

把你的域名清单变成一个随时可更新的米表，发出去也更专业。




## 其他信息
- 演示网站：https://mrmi.top

- 公众号：
- ![公众号](https://www.evan.xin/wp-content/uploads/2025/04/111.png)
- 打赏码(微信&支付宝二合一，方便哈哈)：
- ![公众号](https://www.evan.xin/wp-content/uploads/2025/05/wechat-alipay.png)


---


- # Mi.CD（宝塔面板）部署教程


## 1. 部署包内容说明（你会用到的几个文件）

解压后大致是这些：

- `index.html`：前台（原始 UI）
- `admin/`：后台（管理、导入、公告/链接设置等）
- `api.php`：数据读写接口（PHP）
- `domains.json`：域名数据（后台管理/导入会覆盖写入这个文件）
- `site_config.json`：站点内容配置（公告弹窗 + Other Links）

其中：
- 前台默认走 `GET api.php` 拉取 `domains.json`
- 前台默认走 `GET api.php?type=config` 拉取 `site_config.json`
- 后台保存域名：`POST api.php`（type=domains）
- 后台保存公告/链接：`POST api.php`（type=config）

---

## 2. 宝塔创建站点

1. 进入宝塔面板 → **网站** → **添加站点**
2. 填：
   - 域名：你的域名（例如 `mi.cd` / `www.mi.cd`）
   - 根目录：宝塔自动生成即可（例如 `/www/wwwroot/mi.cd`）
3. PHP 版本：**选择任意可用的 PHP（建议 7.4/8.0/8.1）**

> EvanMi 不需要数据库。

---

## 3. 上传文件到网站根目录

1. 在宝塔面板 → **文件** → 进入站点根目录（例如 `/www/wwwroot/mi.cd`）
2. 上传 `MiCD_deploy_originalUI_v3.zip`
3. 解压后，把解压出来的文件 **移动到站点根目录**，确保最终结构类似：

```
/www/wwwroot/mi.cd/
  index.html
  api.php
  domains.json
  site_config.json
  admin/
  ...
```

> 关键点：`index.html`、`api.php`、`domains.json`、`site_config.json` 必须与 `admin/` 同级。

---

## 4. 设置运行目录与默认首页

宝塔一般默认就会识别 `index.html`，但建议确认一下：

- 网站 → 设置 → **默认文档**：确保包含 `index.html`（放在靠前位置）

---

## 5. 设置伪静态 / 防 404（可选但推荐）

Mi.CD 前台是静态页，后台是独立目录（`/admin/`）。一般不需要复杂伪静态。

如果你想更稳一点（例如某些环境对 404 路由处理较严格），可以保持站点根目录存在 `404.html`。

> 如果你后续想做“漂亮 URL”（比如 `/admin` 直接进后台），那才需要额外伪静态规则；本项目目前不建议把网站搞复杂。

---

## 6. 权限与安全（非常重要）

因为后台要写入：
- `domains.json`
- `site_config.json`

所以需要保证 Web 服务用户对这两个文件 **有写权限**。

### 6.1 建议设置权限

在宝塔 → 文件管理里，选中：
- `domains.json`
- `site_config.json`

设置权限为：
- **644** 通常即可（前提是文件属主/属组正确）

如果遇到写入失败（后台保存报错），再退一步：
- **664** 或临时 **666**（不推荐长期 666）

> 更稳妥的做法是把文件属主改为 Web 服务用户（例如 `www`），但不同系统不同；宝塔里可视化操作即可。

### 6.2 后台密码

- 当前后台访问密钥：`mi.cd@`

修改位置（如果你未来要改）：后台里面可以直接修改密码。

---

## 7. 使用方法（上线后怎么用）

### 7.1 进入后台

- 打开前台页面，左下角有一个“锁”图标
- 输入密钥：`mi.cd@`
- 跳转到后台：`/admin/`

### 7.2 批量导入域名（覆盖式）

后台 → **域名管理** → **批量导入**：
- 支持粘贴文本或上传 CSV/TXT
- 导入后会 **直接覆盖** 现有 `domains.json`（确保数据永远最新）

### 7.3 配置公告弹窗与 Other Links

后台左侧菜单：**公告 / Other Links**
- 公告：设置“标题 + 内容”，保存后前台弹窗即更新
- Other Links：添加/删除卡片（标题、URL、说明），保存后前台即更新

---

## 8. 常见问题排查

### 8.1 后台保存提示失败

优先检查：
1. `api.php` 是否在站点根目录，并且能被访问
2. PHP 是否启用（宝塔站点绑定了 PHP）
3. `domains.json` / `site_config.json` 是否有写权限

### 8.2 前台不显示最新数据

1. 让浏览器强制刷新（Ctrl/Cmd + Shift + R）
2. 检查 `api.php` 的 GET 是否能返回 JSON


