# Output Formatting Rules

## Document Structure

Every output document follows this structure:
1. **Title** — `# [Skill]: [Project Name]`
2. **TLDR** — Always first. 3-5 bullets. The PM should get 80% of the value from this alone.
3. **Body sections** — Per skill template
4. **Tables** — For structured data (objection matrices, risk registers, milestones)

## Formatting Conventions

### Bold Lead-In
Every paragraph in body sections starts with a bold phrase that summarizes the point:

```markdown
**Market timing favors early movers**: The window for entering AI coding tools is
12-18 months based on adoption curves from similar developer tool categories...
```

Not:
```markdown
The window for entering AI coding tools is 12-18 months...
```

### Source Quality Tags
All web-sourced data gets tagged inline:

| Tag | Meaning | Example |
|-----|---------|---------|
| `[Primary Source]` | Official company data, SEC filings, published research | GitHub's annual report |
| `[Industry Report]` | Analyst reports, market research firms | Gartner, IDC, McKinsey |
| `[Blog/Opinion]` | Blog posts, opinion pieces, social media | TechCrunch article, Twitter thread |
| `[Unverified]` | Cannot confirm source reliability | Forum post, unattributed claim |

Format: `"AI coding tools market is $5.2B" [Industry Report — IDC 2025]`

### Tables
Use markdown tables for:
- Objection Response Matrix
- Risk registers
- Milestones
- Trade-off comparisons
- Any data with 3+ columns

Always include headers. Align columns for readability.

### Confidence Indicators
Use this scale consistently:

| Level | Meaning |
|-------|---------|
| **High** | Multiple primary sources agree. Quantitative evidence. Low sensitivity to assumptions. |
| **Medium** | Mix of primary and secondary sources. Some assumptions required. Key variables identified. |
| **Low** | Primarily opinion/blog sources. Heavy assumptions. High sensitivity to key variables. |

### Section Separators
Use `---` between major sections for visual separation.

### Charts and Visualizations (Standard+ DS Output)
When the DS agent produces matplotlib charts:
- Save as PNG to `./pm-workflow/{project-name}/charts/`
- Reference in the markdown: `![Chart description](charts/filename.png)`
- Always include a text summary below the chart for accessibility

## Anti-Patterns to Avoid

- No filler phrases ("It's worth noting that...", "Interestingly...")
- No hedging without specificity ("This could potentially..." — say what conditions would make it true)
- No buzzwords (see bad strategy detector)
- No walls of text — break into scannable sections with bold lead-ins
- No unsourced quantitative claims — every number needs a source tag
