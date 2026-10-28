# 来源取舍与英文论文 AIGC 检测分析

整理日期：2026-10-28。仓库内容和检测产品可能继续变化。以下为实际技能文件、部分脚本与官方/论文来源的综合分析；没有运行这些写作工具生成实验稿，也没有进行 Turnitin/iThenticate 对照检测。风险判断不能当作命中率实验结果。

## 整合来源

| 来源 | 吸收的能力 | 未作为默认规则的部分 |
|---|---|---|
| [K-Dense Scientific Agent Skills](https://github.com/K-Dense-AI/scientific-agent-skills/blob/main/skills/scientific-writing/SKILL.md) | 证据与主张对应、数字与单位核对、观察和解释分离 | 不把较重的全文流程强加给段落润色；各学科清单只在适用时使用 |
| [Master-cai Research-Paper-Writing-Skills](https://github.com/Master-cai/Research-Paper-Writing-Skills/blob/main/research-paper-writing/SKILL.md) | 反向提纲、段落主要信息、摘要与实验的对应 | [摘要模板](https://github.com/Master-cai/Research-Paper-Writing-Skills/blob/main/research-paper-writing/references/abstract.md)和[引言句型](https://github.com/Master-cai/Research-Paper-Writing-Skills/blob/main/research-paper-writing/references/introduction.md)作为选项，不套用 ML/CV/NLP 的技术贡献叙事 |
| [Nature-Paper-Skills](https://github.com/Boom5426/Nature-Paper-Skills/blob/main/skills/core/paper-workflow/SKILL.md) | 先论证再修辞、章节职责、上游结论变化后的定向检查 | [章节参考形状](https://github.com/Boom5426/Nature-Paper-Skills/blob/main/skills/core/paper-workflow/references/section-contracts.md)不作为句段数量配额；[标点预算](https://github.com/Boom5426/Nature-Paper-Skills/blob/main/skills/core/scientific-prose-style/SKILL.md)不作为通用科学写作规则 |
| [PaperSpine](https://github.com/WUBING2023/PaperSpine/blob/main/dist/codex/skills/paper-spine/SKILL.md) | [证据台账](https://github.com/WUBING2023/PaperSpine/blob/main/dist/codex/skills/paper-spine/references/scientific-evidence-ledger.md)、作者术语、写作理由、原改稿对照 | 工作台配置和产品流程不移入本轻量技能；尊重其反固定模板原则 |
| [academic-writing-skills](https://github.com/bahayonghang/academic-writing-skills/blob/main/academic-writing-skills/latex-thesis-zh/SKILL.md) | LaTeX/引用保护、逐单元修改、事实和主张漂移复核 | 中文学位论文骨架、不同期刊模式及文风评分不当作英文实证论文通则 |
| [Claude Scientific Writer](https://github.com/K-Dense-AI/claude-scientific-writer/blob/main/skills/scientific-writing/SKILL.md) | 可作为同类原则的软件执行入口 | 与 Scientific Agent Skills 的同名写作模块高度重叠，二者择一维护；其[检索模块](https://github.com/K-Dense-AI/claude-scientific-writer/blob/main/skills/research-lookup/SKILL.md)的默认 60 条文献目标不作为质量门槛 |
| [Anti-Defensive Writing](https://github.com/Adkid-Zephyr/anti-defensive-writing-Skill)及本地 evidence-aware-writing | 删除无信息辩解、让有效发现清楚可见 | 不将“突出优势”解释为删除负结果、弱化必要边界或选择性报告 |

本草案重新组织与表述这些原则，不打包上游代码、原文或安装脚本。若后续分发原仓库文件，需另行遵守对应许可证；其中 academic-writing-skills 的 README 声明限学术使用。

## 两项实现层面的边界

1. K-Dense 的 [audit_claims.py](https://github.com/K-Dense-AI/scientific-agent-skills/blob/main/skills/scientific-writing/scripts/audit_claims.py)能辅助检查主张/证据 ID、登记状态、文本哈希和数字标记等，不能据此声称已判断文献是否真正支持主张。
2. academic-writing-skills 的 [deai 模块](https://github.com/bahayonghang/academic-writing-skills/blob/main/academic-writing-skills/latex-thesis-zh/references/modules/deai.md)含内部文风指标，同时明确其模式集不是作者身份或检测分数规则。另有 [core.py](https://github.com/bahayonghang/academic-writing-skills/blob/main/academic-writing-skills/paper-writing-studio/scripts/core.py) 的 text_compact 实际采取前部字符截取，不应当作语义压缩能力整合。

## Turnitin 与 iThenticate 实际能说明什么

Turnitin 官方说明使用训练的分类器；研究检测方法还包括概率与曲率等路线。因此，“所有检测器都只查困惑度和句长变化”不成立。公开资料也不足以给出这些商业模型所有特征的权重或一个必然命中的词表。[官方 Trust Center](https://helpcenter.turnitin.com/hc/en-us/articles/49295931087373-Turnitin-Trust-Center)、[DetectGPT](https://arxiv.org/abs/2301.11305)、[Fast-DetectGPT](https://arxiv.org/abs/2310.05130)

Turnitin 的 AI 百分比是符合条件的连续正文中被模型标记为可能 AI 生成或改写的文本比例；它与相似度分数独立。30% 不表示“作者有 30% 概率用了 AI”，也不证明实际代写了 30%。官方承认误报，并因较低区间的误报风险隐藏 1%–19% 的具体分数与高亮。这个产品显示规则不是允许使用 AI 的比例或投稿阈值。[Turnitin 报告说明](https://guides.turnitin.com/hc/en-us/articles/22774058814093-Using-the-AI-Writing-Report)

iThenticate 的 AI 报告同样引用 Turnitin 检测模型，并与相似度报告区分；具体账户需启用该功能。两者不能被当作两套完全独立的作者身份鉴定。[iThenticate 官方说明](https://guides.ithenticate.com/hc/en-us/articles/27835303398541-AI-writing-detection-in-the-Similarity-Report)

## 将可能的问题分开看

下表的“技能风险”是从规则和输出机制做出的推断，不是已知商业检测规则。仅凭最终文本，无法可靠判断具体用了哪个技能。

| 值得核查的问题 | 这些技能可能怎样引入 | 应如何解释 |
|---|---|---|
| 局部措辞与句法分布趋同 | 同一种模型持续润色，反复套摘要与引言句型 | 学习式分类器可能利用的统计模式；不能由某个连接词、长句、破折号或三项排比单独判 AI |
| 段落推进高度同构 | 每段都采用重要性、缺口、发现、意义的同一骨架 | 可读性与模板化问题；尚无本次技能输出对 Turnitin 的实测结果 |
| 形式成熟但研究信息空泛 | 未提供证据，仍要求生成完整讨论或扩写 | 人工阅读可查研究对象、比较、测量和解释是否具体；不宜称为检测器已知事实核验功能 |
| 章节文风或术语漂移 | 多个技能接力翻译、润色，或过量替换同义词 | 编辑一致性问题，不能单独证明作者身份 |
| 结论大于证据 | 过度追求创新性、果断表达或一个完美故事 | 审稿问题：核对零效应、混杂、比较对象和机制证据；普通 AIGC 分类器不等于科学事实审查 |
| 引文、统计和图表对不上 | 先写论点再事后补引文，或把登记校验当内容核验 | 需要作者、导师或审稿人复核，不应期待 AIGC 分数替代它 |

科学文本检测研究曾使用词频、句长、段落等特征，但单项特征不足，且实验体裁有限，不能将早年特定样本上的准确率套到当前英文农业论文。[Desaire 等，2023](https://pmc.ncbi.nlm.nih.gov/articles/PMC10328544/)

大规模词频研究能观察群体层面的 LLM 文风变化；不能据一个词或一句话鉴定一篇论文。[Kobak 等，Science Advances 2025](https://arxiv.org/abs/2406.07016)

历史研究发现部分检测器对非母语英语有误报风险；多领域、多语言基准也显示迁移局限。这些结果支持审慎解释检测报告，不支持“当前所有系统都无效”或“语言流畅必然被判 AI”。[Liang 等，Patterns 2023](https://doi.org/10.1016/j.patter.2023.100779)、[M4，EACL 2024](https://aclanthology.org/2024.eacl-long.83/)

## 对英文土壤与作物论文的建议

以作者现有数据、解释和目标期刊为输入，选一个主写作流程；文献检索、引用核对、绘图和排版按需调用。优先检查氮浓度/吸收量/效率的概念、Ca 与 Mg 的分别证据、改良剂同时影响 pH 与养分时的归因，以及盆栽或单季结果的外推范围。

例如，若一个处理同时改变土壤 pH 和 Ca 供给，单凭其产量响应就写“证实 Ca 限制是唯一机制”，可能超出该设计能区分的因果范围。这是待根据真实对照和测量判断的研究问题，不是靠换词能解决的文风问题。

编辑质量依据内容准确、证据对应和表达清楚评估。保留真实原稿、数据分析、引用笔记和实际修改记录；如目标期刊要求，依据实际使用方式说明 AI 辅助。采用技能名称、内部去 AI 分数或商业分数都不能替代这些工作，也不能保证另一模型或未来版本的检测结果。

## 草案验证

已通过 skill-creator 的 quick_validate.py 格式校验。另由独立子代理只读使用本技能，处理一个假设的盆栽 Results 段落：所给记录包含处理同时改变 pH 和 Ca、产量增加 12%（P=0.03）、N 吸收量增加 8%（P=0.12）、未计算 N 利用效率、缺少 pH 匹配对照。交付正确保留了数值及不显著结果，删除“Ca 是唯一原因”和未经计算的效率主张，且保持两句及 Results 范围。该案例用于验证一次可观察行为，不代表全面写作质量评估，也不提供 AIGC 检测效果证据。
