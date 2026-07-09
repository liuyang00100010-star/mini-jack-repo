# 小柳总视频笔记

品牌战略咨询报告库。使用 brand-strategy skill 对目标品牌进行全链路分析，输出 HTML 格式报告。

## Environment & Tooling
- This is a **Windows environment** with a known Windows Store Python placeholder issue. Do NOT attempt to use `python` or `pip` directly for PDF/HTML rendering. Use **Node.js** (available) for any file conversion or rendering tasks.
- If an approach fails 3 times, STOP and explain the blocker to the user rather than continuing to retry variations of the same failed approach. Present 2-3 alternative approaches for the user to choose from before continuing.

## General Approach
- When a task involves browsing external websites, FIRST confirm the site is accessible via a quick test fetch before committing to a full analysis workflow. If the test fails, tell the user immediately what the error is and suggest alternatives (Playwright MCP, manual data paste, etc.) rather than retrying the same approach.
- Break complex tasks into verified checkpoints: confirm each prerequisite works before proceeding to dependent steps.

## Web Access Constraints
- **Xiaohongshu (小红书)** is inaccessible via WebFetch/WebSearch due to anti-scraping protections. Do NOT attempt direct access.
- **GitHub** domain access is restricted in this environment. Use `git clone` via Bash instead of WebFetch for repos.
- For sites requiring JavaScript rendering or login, Playwright MCP must be pre-configured AND confirmed working before starting a task. Always check with a test navigation first.
- Update this section whenever a new domain is found to be inaccessible.

## Content Access Fallback Chain
When analyzing content from a URL/domain, do NOT get stuck on a single access method. Use this priority chain:
1. Try WebFetch on the target URL. If it returns blocked/empty/error →
2. Try WebFetch on the Wayback Machine archived version at `https://web.archive.org/web/2025/[URL]`. If that fails →
3. Use Playwright MCP to launch a real browser, navigate to the page, wait for content to load, and extract text. If Playwright fails →
4. Search the web for `'[DOMAIN] site content analysis'` or `'[ACCOUNT NAME] profile summary'` to find third-party coverage or cached copies.
- At each step, if you get content, save it and proceed. If all 4 methods fail, explicitly document what each method returned so the user can troubleshoot.
- Never spend more than 2 attempts per method before moving to the next fallback.

## Skill Development Workflow
- When reviewing or refining skill documents, always re-read the CURRENT file version before making claims about its content. Do not rely on cached/previous versions.
- After adversarial review, verify each flagged issue still exists before presenting it to the user.

## Brand Strategy Workflow Templates

### Multi-Brand Testing
```bash
# Test brand strategy skill on multiple brands:
for brand in "Nike" "Apple" "Tesla"; do
  claude -p "/brand-strategy $brand" --allowedTools "Bash,Read,Write,Edit,WebSearch,WebFetch"
done
```

### Setup Session Checklist
When starting a new session or onboarding, verify all capabilities:
1. Verify all MCP servers are working
2. Test Playwright can browse a JavaScript-heavy site
3. Confirm Node.js can do HTML-to-PDF conversion
4. Document all working capabilities in CLAUDE.md so future sessions don't repeat setup

### 3-Agent Parallel Research Workflow
For a brand strategy analysis for `[COMPANY]`:
1. Launch 3 parallel research agents:
   - Agent 1: Research competitive landscape and market positioning → save to `analysis_temp/agent_1.md`
   - Agent 2: Analyze brand perception and customer sentiment from public sources → save to `analysis_temp/agent_2.md`
   - Agent 3: Evaluate pricing strategy and distribution channels → save to `analysis_temp/agent_3.md`
2. After all agents complete, read all three files and synthesize a unified brand strategy report with executive summary, SWOT analysis, and 5 strategic recommendations.
3. If any agent fails on web access, have it retry with alternative sources or note gaps explicitly rather than hallucinating.

### Self-Improving Skill Workflow
1. Read the current skill file at the skill path
2. Run it against a test brand (e.g. 'Nike') and generate a brand strategy report
3. Score the report against this rubric:
   - Min 5 sections
   - Each section has data-backed claims
   - No generic filler paragraphs
   - Actionable recommendations with timeline
   - Competitive differentiation matrix included
4. For any rubric item that fails, identify the specific section and rewrite JUST that section to meet the criteria
5. Re-score and repeat until all items pass or you've iterated 3 times
6. Save the final polished report and the iteration log showing what changed at each step

## 项目结构

```
小柳总视频笔记/
├── CLAUDE.md                    # 本文件
├── README.md                    # 项目说明
├── docs/                        # 文档
│   └── brand-analysis-guide.md  # 品牌分析方法论
├── *_品牌战略咨询报告.html       # 最终交付报告（HTML）
└── *_品牌战略咨询报告.md         # 报告 Markdown 源文件
```

## 核心规则

- **输出格式**：最终交付物必须是 HTML（含嵌入式 CSS、@page 打印规则、封面页），不接受 MD 作为最终交付
- **分析深度**：分析框架部分必须展示完整分析路径（4 步：框架选择 → 分析步骤 → 推理过程 → 结论），不能只抛结论
- **信息源**：使用 360搜索 + 搜狗搜索 + 360百科 作为主要信息渠道（WebSearch/WebFetch 在当前环境不可用）
- **工具链**：httpx + BeautifulSoup 做网页抓取，不依赖 urllib
- **隐藏前提优先**：分析前先识别行业/品类的隐藏前提，再进入正式框架分析

## 命名规范

- 报告文件：`{品牌中文名}_品牌战略咨询报告.{html|md}`
- HTML 为交付版，MD 为源文件

## 深入文档

| 文档 | 说明 |
|------|------|
| [docs/brand-analysis-guide.md](docs/brand-analysis-guide.md) | 品牌分析方法论与框架清单 |
