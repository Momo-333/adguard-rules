# AdGuard Home 合并精简规则

适用于 **AdGuard Home 的 DNS 过滤订阅**。将三份上游列表中适合 DNS 过滤的规则提取、合并和精简，提供一个可直接订阅的 HTTPS 地址。

当前发布文件包含 **494,948 条规则**，大小约 **13.05 MB**，已使用 **AdGuard Home v0.107.80 实际导入验证**，引擎报告的加载数量与文件条目数一致。

> 当前为人工生成并发布的规则快照，**尚未配置自动同步上游或定时更新**。AdGuard Home 定时刷新订阅，只会获取本仓库已经发布的版本。

## 订阅地址

复制下面的地址，添加到 AdGuard Home 的 DNS 黑名单：

```text
https://raw.githubusercontent.com/Momo-333/adguard-rules/main/AdGuardHome-merged.txt
```

[打开或下载规则文件](https://raw.githubusercontent.com/Momo-333/adguard-rules/main/AdGuardHome-merged.txt) · [在仓库中查看文件](./AdGuardHome-merged.txt)

如果首次下载较慢，请稍后重试；订阅文件约 13 MB，访问速度取决于设备到 GitHub Raw 的网络连接。

## 如何添加

1. 打开 AdGuard Home 管理页面。
2. 进入 **过滤器 → DNS 黑名单**。
3. 选择 **添加黑名单 → 添加自定义列表**（不同语言或版本的按钮名称可能略有差异）。
4. 名称填写“合并精简规则”，URL 粘贴上面的订阅地址，保存并等待下载完成。
5. 在列表中确认已启用；遇到误拦截时，使用 **查询日志** 和 **检查过滤** 功能查找命中规则，再按需添加自定义例外。

这份文件内部已经包含白名单例外 `@@`，作为一个 DNS 黑名单订阅导入即可，无需把整个文件再加入 DNS 白名单。

## 来源与处理方式

上游仓库：[BlueSkyXN/AdGuardHomeRules](https://github.com/BlueSkyXN/AdGuardHomeRules)。本次处理的三个源文件：

- [all.txt](https://github.com/BlueSkyXN/AdGuardHomeRules/blob/master/all.txt)：主要规则集合，包含 hosts、域名过滤及浏览器过滤等多种语法。
- [skyrules.txt](https://github.com/BlueSkyXN/AdGuardHomeRules/blob/master/skyrules.txt)：补充拦截规则。
- [ok.txt](https://github.com/BlueSkyXN/AdGuardHomeRules/blob/master/ok.txt)：白名单例外。

处理包括：

- 提取选定的 DNS 兼容规则与 hosts 映射，去除重复项。
- 排除网页元素隐藏、URL 路径、浏览器条件及本次未支持的修饰符规则。
- 排除条件块、未解析的外部引用，以及本次识别出的被 `$badfilter` 禁用目标。
- 删除已有纯父域拦截规则覆盖的冗余子域条目和零地址 hosts 条目，不为减少数量而新增父域封锁。
- 保留提取出的 DNS 白名单与 hosts 映射含义。

这是针对 AdGuard Home 制作的筛选快照，**不与原始浏览器过滤列表完全等价**，也不宣称保留上游每一种 DNS 语法。DNS 过滤无法按网页元素或 URL 路径进行拦截。

## 当前文件统计

| 项目 | 数量 |
| --- | ---: |
| 实际加载规则总数 | 494,948 |
| 白名单例外 | 337 |
| DNS 模式规则 | 390 |
| 纯域名拦截规则 | 140,660 |
| hosts 条目 | 353,561 |
| 文件大小 | 13,052,290 字节 |

相比三个源文件合计的 1,127,527 个非注释条目，数量减少约 **56.10%**。这个比例同时包含不兼容或未支持语法的剔除和冗余精简，不能全部理解为重复率；源列表条目数也不等于其在 AdGuard Home 中的实际加载数。

## 白名单策略与注意事项

保留原列表中提取出的放行策略，意味着部分统计或测量服务仍会被允许。例如：

- `analytics.google.com`
- `app-measurement.com`
- `@@||tagtic.cn^` 会放行该域及其子域，因此会覆盖普通的 `ad-static-xg.tagtic.cn` 封锁。

如果你的目标是封锁这些服务，请先通过查询日志检查实际命中情况，再调整订阅内容或自定义规则；白名单的存在是当前发布版本的明确行为。

这份列表规模较大，实际内存占用和处理速度取决于设备及 AdGuard Home 版本。不要将“加载成功”理解为对所有网站均无误拦截或覆盖全部广告。

## 验证结果

在独立的 AdGuard Home v0.107.80 测试实例中：

- 实际导入成功，报告 **494,948 条规则**。
- 抽查 `doubleclick.net` 被拦截。
- 抽查原列表中部分微信、Google Analytics 和 `app-measurement.com` 放行规则生效。
- 上传后核对远端 Git 文件对象与本地文件一致。

规则文件 SHA-256：

```text
cae3227d58c68c47a32ee65561922eace336c0e787d169752f10704e4241e720
```

这些是加载与样例命中验证，不是对全部域名行为的穷尽测试。

## 更新与反馈

当前没有自动更新工作流。后续发布时覆盖根目录的 `AdGuardHome-merged.txt`，订阅地址即可保持不变；数量和校验值应随文件一并更新。

如遇误拦截或漏拦截，可在本仓库提交 [Issue](https://github.com/Momo-333/adguard-rules/issues)，附上域名、AdGuard Home 版本及命中规则。请勿提交包含账号、令牌或其他隐私的完整日志。

## 来源声明

本仓库提供上游规则的整理版本，非 AdGuard 官方规则库。规则内容的权利与使用条件以各原作者及上游项目声明为准；此处不为上游内容重新授予许可。
