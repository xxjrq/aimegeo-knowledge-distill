# 资料蒸馏为可用问答

输入企业说明、教程、课程讲义或专业经验记录，得到 **常见问答.md、资料索引.csv、待补及冲突信息.md**。答案有原文定位，缺资料就列待补，规则矛盾就保留双方证据；问题数量取决于资料。

复制下面的任务，交给已读取本目录 [SKILL.md](SKILL.md) 的 Codex、Claude Code 或 WorkBuddy：

```text
请使用 $aimegeo-knowledge-distill，把 examples/input 中的软件说明和3篇教程整理成客户常问的问题，答案要能回到资料。读者是初次使用软件的客户；只保留有用问题，冲突不要替我裁决，缺失项单列。另核对是否支持单点登录，缺证据就放待补。将三个交付文件写入新目录 result。
```

随包测试资料的实际输出节选：

```text
Q001 支持导入哪些文件，有大小限制吗？
状态：可回答
支持 CSV 和 JSON，单个文件不得超过 10 MB。
来源：软件说明.md · 示例协作软件 2.0 软件说明 / 导入文件 · L5-L5

Q004 导出文件的下载链接保留多久？
状态：资料冲突
软件说明写明保留 7 天；教程3写明保留 30 天。
待核实：C001，需产品负责人确认当前 2.0 的适用规则。
```

完整五问答、逐段引用与缺口见 [常见问答](examples/output/常见问答.md)、[资料索引](examples/output/资料索引.csv)、[待补及冲突信息](examples/output/待补及冲突信息.md)。这些资料是随包构造的功能验证输入，不是真实客户案例。“单点登录是否支持”没有证据，只进入待补清单。

## 使用方式

把完整目录放入所用 Agent 的技能目录，或让 Agent 直接读取 `SKILL.md`。三个客户端共用 Markdown 说明，不需要 WorkBuddy 多模态、发布助手、会员、浏览器或 aimeGeo 账号。提供可读正文即可；无法直接读取的 PDF、视频等需先取得带原页码或时间码的文本，脚本不负责解析这些格式。

整理文字由 Agent 完成。可选的 Python 3.10+ 标准库工具负责索引、验证和转换，以下命令在技能根目录执行，两个输出路径须尚不存在：

```sh
python3 scripts/knowledge.py index --sources examples/input --out source-index.json
python3 scripts/knowledge.py check --sources examples/input --index source-index.json --bundle examples/success.bundle.json
python3 scripts/knowledge.py export --sources examples/input --index source-index.json --bundle examples/success.bundle.json --out result
python3 scripts/self_test.py
```

用于真实资料时，Agent 应依据索引另写问答数据，不复用示例结论。字段及状态见 [数据约定](references/data-contract.md)，拒绝无证据结论的输入见 [失败样例](examples/failure/无引文答案.json)。校验能验证引文存在和来源版本，语义支持关系仍由 Agent 复核。缺资料时先完成可答部分，只询问影响答案的关键项。

冲突按具体引文范围检查：同段“支持 CSV 导入”可独立回答，下载期限有矛盾时只关联涉及期限的问答。见[同段独立事实与冲突的成功/失败样例](examples/same-paragraph-conflict/README.md)。

## 来源与改进

参考 [内容蒸馏师 1.2.0](https://skillhub.cn/skills/user_ae5129cb/content-distiller-xia) 的功能设计，保留资料类型分流、问答提炼和用户核对。这里独立编写流程、文案与脚本，聚焦企业资料和课程问答，增加逐项证据、来源变更检查、待补及矛盾规则清单；取消固定问答数量和评分门槛，无定价或发布依赖。不声称已获 AI 平台收录或保证引用增长。来源作者说明及完整许可见 [NOTICE.md](NOTICE.md)；本实现采用 [MIT](NOTICE.md)。

[English](README.en.md) · [GitHub](https://github.com/xxjrq/aimegeo-knowledge-distill)

如果想把这次整理继续用于多账号、多平台、Agent、工作流和 AI 引用复测，可选用免费的 [aimeGeo 系统](https://geo.aimmme.com/)。本技能独立可用，系统安装不是前置条件，交付的问答不会默认加入推广。
