# MSOP · 内部规程图形载体

MUKE 的 SOP 文档里那些能点、能动的图，都由这个仓库托管。
线上：<https://zanboooo.github.io/MSOP/>

它和招聘门户 [`muke`](https://github.com/zanboooo/muke) 是**两个仓库、两条线**：

| | `muke` | `MSOP`（本仓库） |
|---|---|---|
| 面向 | 候选人 | 员工 / 内部规程 |
| URL 出现在 | 招聘广告、二维码、经纪人链接 | 只在 Lark 文档与入职私信里 |
| 数据 | `data/` 由 CI 每天 8 次自动生成 | 无自动同步，全是静态页 |
| 改动频率 | 天 | 月 |

**为什么要拆开：** 报名页的网址印在对外招聘物料上，任何人都能顺藤摸到仓库。
拆开之后，从招聘广告出发不会直接落到内部规程页上。

---

## ⛔ 动手前先读这一段

**这是一个 public 仓库。放进来的每个文件，任何人拿到 URL 都能下载。**

| 红线 | 为什么 |
|---|---|
| **不放真人姓名、工号、电话** | 演示页一律用虚构数据。工号只用 `MK90xx` 段（真实号段是 `MK20xx`/`MK21xx`） |
| **不放工资结构** | 时薪、月薪、扣款基数一概不行——除以 173 就能反推出真实档位 |
| **`avatar/data/roster.enc.json` 是加密名册** | 密钥 = 手机号后 9 位 ＋ 工号双因子。别提交明文版，别降低密钥强度 |
| **`avatar/vendor/` 不要手改** | 上游模型镜像，内容哈希命名。要更新跑 `AI CEO/tools/mirror_imgly.js` |
| **删页面前先查 SOP 有没有内嵌** | 线上 SOP 用 `block_type=26` iframe 挂着这些页。删页 = 文档里出现空白框 |

**删文件删不掉历史。** `git rm` 只加一个删除提交，文件仍在之前每一个提交的树里，
GitHub 会一直按 SHA 提供它。所以这个仓库从**一个孤儿提交**起步——
「历史是干净的」只需 `git rev-list --count HEAD == 1` 就能证明。请保持这个性质。

---

## 里面有什么

| 页面 | 是什么 | 被谁用 |
|---|---|---|
| `avatar/` | 员工头像制作台（含 141MB 上游模型镜像） | SOP-MED-005 内嵌 ＋ 入职私信直发 |
| `avatar/flow/` | 头像制作流程图 | SOP-MED-005 |
| `welcome/` | 新人第一天欢迎页 | 入职私信直发（`AI CEO/tools/gen_invite_links.js` 生成） |
| `onboarding/` `h01demo/` `h01map/` `footer/` | 入职建档、字段地图、签到 | SOP-HR 系列 |
| `akses/` `aksesflow/` | 权限矩阵与三层账号模型 | SOP-GOV-004 |
| `absensi/` `dayldemo/` | 考勤扣款试算、每日异常巡检 | SOP-HR-005 |
| `examdemo/` `nilai/` | 笔试成绩查看、档位换算 | SOP-HR-012 |
| `cutistep/` `kalkulator/` | 请假补卡演示、假期试算 | SOP-HR-002 |
| `h24demo/` `lanjut/` `notice/` | 团队同步演示、候选人续接演示、密级声明 | SOP 内嵌 / 备用 |
| `index.html` | 制图台（内部作图工具） | 只给 AI 和张博 |
| `probe.html` | iframe 嵌入探针 | 排查文档里内嵌不显示时用 |

## 改了页面之后

静态站，推上来就生效，没有 CI。但 **Lark 文档里的 iframe 不会自己刷新缓存**，
改完让张博在文档里点一下重新加载，或者直接开新 URL 确认。

⚠ **页面路径就是 SOP 里的 iframe 地址。改目录名 = 文档里出现空白框。**
真要改，先在 `AI CEO` 跑一遍 `scripts/probe_sop_iframes.js` 拿到受影响的块清单。
