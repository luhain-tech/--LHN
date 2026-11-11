# A股量化项目对比与Bot验证思路

**更新时间**：2026-11-11

本文档整理主流A股量化开源项目，并与本项目（`--LHN`）及本地 Bot 技能的验证思路进行对比。

## 推荐关注的开源项目

| 项目 | Stars | 核心特点 | 链接 |
|------|-------|----------|------|
| **a-stock-data** | ~11.7k | A股全栈数据工具包（行情/K线/研报/资金/新闻/财务/公告等），专为 AI Agent 设计 | https://github.com/simonlin1212/a-stock-data |
| **Sequoia-X** | ~8.6k | A股自动选股系统，技术形态扫描，akshare/baostock | https://github.com/sngyai/Sequoia-X |
| **tick-stock-panel** | ~7.5k | 自托管 A 股「策略+监控+回测」工作台，LLM 驱动 | https://github.com/shy3130/tick-stock-panel |
| **UZI-Skill** | ~7.5k | 游资/机构视角 Skills，多维数据 + 量化规则 | https://github.com/wbh604/UZI-Skill |
| **adata** | ~5.3k | 免费开源 A 股量化交易数据库，多数据源融合 | https://github.com/1nchaos/adata |
| **TradingAgents-astock** | ~3.9k | A 股多 Agent 投研框架（龙虎榜/游资/解禁等） | https://github.com/simonlin1212/TradingAgents-astock |
| **Ashare** | ~3.9k | 极简免费 A 股行情接口（新浪/腾讯双源） | https://github.com/mpquant/Ashare |
| **akquant** | ~2.5k | 高性能 Rust+Python 回测框架（本项目选用的核心引擎） | https://github.com/akfamily/akquant |
| **CNEquity** | ~1k | 开源 A 股本地数据湖，PIT、复权、行级溯源，MCP 原生 | https://github.com/rootSunc/CNEquity |

## 本项目 Bot 的验证思路

结合本地 skills（stock-analysis / multi-agent-analysis / a-share-integration 等）与仓库现有文档：

1. **证据优先的最小路径**  
   只依据云端 Linux 实际安装、启动、跑测的已验收结果（证据等级 L2）。当前推荐：`akquant==0.3.84` + SQLite 3.46.1 + `cn_stock_t1_low_fee` 模板。默认不加重型中间件。研究运行时与券商运行时分离。

2. **三维度强制框架**  
   基本面、技术面、消息面必须独立成块输出。

3. **多源交叉验证（中国大陆优先）**  
   东方财富、雪球、同花顺/10jqka 等优先。关键指标必须二次搜索确认，标注来源与时间。至少两个不相关来源交叉核对。

4. **多智能体编排**  
   分解为技术、基本面、消息面、交叉验证、综合 Agent。并行取数后合成，明确共识与分歧。

5. **透明与克制**  
   结构化中文报告 + 可点击来源 + 强制免责声明。不编造数据。

## 对比总结

| 维度 | 主流开源项目 | 本项目 Bot 思路 | 匹配度 |
|------|--------------|-----------------|--------|
| **数据层** | 多数封装 akshare/tushare，强调多源自动降级与本地落库 | 同样依赖 akshare 系，更强调实时网页二次校验与来源标注 | 高（互补） |
| **分析框架** | 有的偏技术形态选股，有的偏多 Agent LLM 研报 | 强制三维度 + 多 Agent 交叉验证，结构化输出 | 很高 |
| **回测/环境** | 完整回测引擎、桌面/Web 工作台、Docker 一键部署 | 刻意极简：akquant + SQLite，证据驱动 | 中高 |
| **验证风格** | 多数重功能覆盖与易用性 | 重「已验收证据」+「多源实时交叉」+「研究/交易分离」 | 差异化明显 |
| **目标用户** | 个人量化开发者、想快速搭系统的人 | 想做严谨研究记录、AI Agent 辅助决策、控制环境复杂度的人 | 互补 |

**结论**：本项目 Bot 的验证思路与当前热门的「多 Agent + 多数据源」方向高度一致，但更强调**最小可验证路径**和**实时交叉校验的严谨性**。适合作为「研究层」的严格记录与 Agent 编排，而把数据获取、回测引擎等交给成熟开源项目来补齐。

本文档仅作研究记录，不构成投资建议。
