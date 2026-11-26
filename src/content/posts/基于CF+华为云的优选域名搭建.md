---
published: 2026-11-30
title: 基于CF的优选域名搭建(其一：使用华为云)
pinned: false
description: 基于CF的优选域名搭建，最新方案更新！
tags: [Markdown, Huawei Cloud]
category: 教程
image: ./images/2.jpg
---
### 提前准备
1. Cloudflare账号 x1

2. 华为云国际版账号 x1

3. 域名 x1，注意不能是 xxx.cc.cd、xxx.ddns.ge等复合后缀产生的域名后缀
## 步骤
1. 在 Cloudflare 内新建一个worker，名称任意，**只能绑定自定义域路由**~~绑定自己域名 `cf.example.com`或路由 `cf.example.com/*`~~

2. 点击【编辑代码】，将以下代码粘贴进去，点击【部署】，访问显示 OK 即完成

```worker.js
骗你的，worker里不用加内容
但如果想让域名被访问时显示内容，则根据需要编写即可
```

3. 在华为云国际版内添加该域名，并在 Cloudflare 内手动添加相关的 NS 记录

4. 使用 AI 或上相关网站查找别人现成的优选域名或优选IP，不想找的话可复制 [次元星域优选域名](https://cf.107211.xyz/)或[上游](https://cdn.107211.xyz/)

5. 按下表参考样式填写

| 域名 | 记录类型 | 线路类型 | TTL | 记录值 | 备注 |
| :---: | :---: | :---: | :---: | :---: | :---: |
| cf.example.com | CNAME/A | 全网默认 | 300 | 你找的优选域名/优选IP | 上网查找 |
| cf.example.com | CNAME/A | 中国大陆 | 300 | 你找的优选域名/优选IP | 同上 |
| *.cf.example.com | CNAME/A | 全网默认 | 300 | 你找的优选域名/优选IP | 同上 |

↑上表为新方案

下表为旧方案↓

| 域名 | 记录类型 | 线路类型 | TTL | 记录值 | 备注 |
| :---: | :---: | :---: | :---: | :---: | :---: |
| cf.example.com | CNAME | 全网默认 | 300 | 你的worker地址 | / |
| cf.example.com | CNAME | 中国大陆 | 300 | 你找的优选域名 | 若找的是优选IP，此时【记录类型】为A，记录值为优选IP |
| *.cf.example.com | CNAME | 全网默认 | 300 | 你找的优选域名 | 同上 |

6. 稍等一两分钟，再次使用 `nslookup` 命令检查域名，若能顺利显示配置的内容则进行下一步

7. 使用 itdog 等测速网站，进行 ping 测速、HTTP3 测速和 dns 测速，若三个值都较小，则完成