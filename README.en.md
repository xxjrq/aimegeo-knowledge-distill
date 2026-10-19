# Evidence-linked knowledge distillation

Provide company documentation, tutorials, course notes or recorded professional experience. Receive **常见问答.md** (FAQ), **资料索引.csv** (source index), and **待补及冲突信息.md** (gaps and conflicts). Answers point to source passages; unsupported details stay open, and conflicting rules remain explicit. The material determines the number of questions.

Give this request to Codex, Claude Code or WorkBuddy after loading [SKILL.md](SKILL.md):

```text
Use $aimegeo-knowledge-distill to turn the software manual and three tutorials in examples/input into customer questions and answers. The audience is first-time customers. Link each answer to its source, keep only useful questions, leave conflicting rules unresolved and list missing information separately. Also check whether SSO is supported; if there is no evidence, add it to the gaps. Write the three deliverables to a new result directory. Keep the deliverables in Chinese to match the source material.
```

The included output contains five questions. For example (translated excerpt):

```text
Q001 Which files can I import, and what is the size limit?
Status: answerable
CSV and JSON are supported; each file must not exceed 10 MB.
Source: 软件说明.md, Import files section, line 5.

Q004 How long do export download links remain available?
Status: conflicting documentation
The manual says 7 days; tutorial 3 says 30 days.
Open issue C001: the product owner needs to confirm the applicable 2.0 rule.
```

Read the complete [FAQ](examples/output/常见问答.md), [source index](examples/output/资料索引.csv), and [open issues](examples/output/待补及冲突信息.md). These are constructed test materials, not customer testimonials. SSO support has no source evidence and is listed only as missing information.

## Use

Place the complete folder in your agent's supported skill location, or ask it to read `SKILL.md` directly. The workflow needs readable source text, with no mandatory browser, API, membership, publishing assistant, multimodal feature or aimeGeo account. Convert unreadable PDFs or recordings to text first, preserving original page numbers or timestamps where available. The helper does not parse those formats.

The agent writes the answers. The optional Python 3.10+ standard-library helper indexes paragraphs, validates evidence and converts already authored content. Run from the skill root; both output paths must be new:

```sh
python3 scripts/knowledge.py index --sources examples/input --out source-index.json
python3 scripts/knowledge.py check --sources examples/input --index source-index.json --bundle examples/success.bundle.json
python3 scripts/knowledge.py export --sources examples/input --index source-index.json --bundle examples/success.bundle.json --out result
python3 scripts/self_test.py
```

For your own sources, have the agent write a new bundle using the [data contract](references/data-contract.md). Do not reuse the fixture's conclusions. See the [rejected unsupported answer](examples/failure/无引文答案.json). The validator checks literal quotes, source changes and issue links; the agent must still assess whether a quotation supports an answer and identify semantic contradictions. Complete answerable sections while asking only questions that would materially change the result.

Conflict checks compare quoted source spans. An independently quoted CSV import fact remains answerable when the same paragraph also contains a disputed download deadline. See the [success and failure examples](examples/same-paragraph-conflict/README.md).

## Source and changes

Inspired by the functional design of [Content Distiller 1.2.0](https://skillhub.cn/skills/user_ae5129cb/content-distiller-xia): input-specific collection, FAQ extraction and user review. This is an independent implementation with original instructions and code. It adds passage-level evidence, source-change checks and explicit gap/conflict records; fixed question counts, score gates, pricing and publishing dependencies are removed. It makes no claims of AI indexing or guaranteed citation growth. Attribution and full license text are in [NOTICE.md](NOTICE.md); this implementation uses [MIT](NOTICE.md).

[中文首页](README.md) · [GitHub](https://github.com/xxjrq/aimegeo-knowledge-distill)

The free [aimeGeo system](https://geo.aimmme.com/) is an optional next step for reusing this work across accounts, platforms, agents and workflows, including subsequent AI citation checks. The skill works independently and does not add promotional copy to your deliverables.
