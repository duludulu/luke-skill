---
name: defi-competitive-analysis
description: |
  A UI/UX-focused competitive analysis workflow. Covers the full process from confirming the target product and competitors, collecting interface information, to outputting structured comparison tables and Markdown files.

  **Trigger conditions (any of the following should trigger this skill):**
  - User mentions "competitive analysis," "competitor comparison," "benchmarking," "UI comparison," or "UX comparison"
  - User wants to analyze a product's interface design or compare UI/UX with other products
  - User provides multiple product URLs and asks to compare a specific feature module's design or interaction
  - User says "benchmark us against XX," "how does our UI compare to XX," or "analyze the competitor's design"
  - User uploads product screenshots for competitive design comparison

  **This skill covers UI/UX competitive analysis only — it does not include PRD generation, prototyping, or implementation work.**
---

# UI/UX Competitive Analysis Workflow

This skill has **4 steps** that must be executed in order. **Steps 1 and 2 confirmation stages must not be skipped.**

---

## Workflow Overview

```
Step 1: Confirm analysis subjects (wait for user reply)
    ↓
Step 2: Collect interface information (prompt for screenshots; continue regardless of upload)
    ↓
Step 3: Multi-dimensional comparative analysis
    ↓
Step 4: Output Markdown file
```

---

## Step 1 · Confirm Analysis Subjects

**Upon receiving the task, the first thing to do is send the following confirmation message and wait for a reply.**

---

> **Before starting the analysis, please confirm the following:**
>
> **① Our Product** (the subject of analysis)
> - Product name / URL / page screenshot?
> - Which specific feature module? (e.g., login page, Swap trading page, Dashboard homepage)
>
> **② Competitor List** (2–4 recommended)
> - Competitor A: name / URL
> - Competitor B: name / URL
> - Competitor C: name / URL (optional)
>
> **③ Analysis Goal**
> - Quick overview / Design review / Iterative improvement / Other?

---

After the user replies, **confirm once more** before proceeding:

```
✅ Subject: [Product Name] — [Feature Module]
✅ Competitors: [Competitor A], [Competitor B], [Competitor C]
✅ Goal: [Goal]

Confirmed. Proceeding to collect interface information for each product.
```

---

## Step 2 · Collect Interface Information

After confirming the analysis subjects, **send the following prompt** to request screenshots, **then wait approximately 30 seconds**. If the user does not upload anything, proceed directly to Step 3 without prompting again.

---

> **📸 Uploading screenshots of each product is recommended (optional, but significantly improves analysis quality)**
>
> Screenshots are the most accurate source for UI analysis. If possible, please provide:
>
> - **Our product**: Full page screenshot of the [feature module]
> - **Each competitor**: Full page screenshot of the same feature module
>
> **Screenshot guidelines:** Desktop (1280px wide), covering default state and key interaction states
>
> **No screenshots? No problem** — I'll analyze using URL fetching and public information, and note the source in the report.

---

### Information Collection Strategy (by priority)

Regardless of whether screenshots are provided, collect information in the following order:

| Priority | Method | Best For |
|---|---|---|
| ★★★ | User-uploaded screenshots | Most accurate; direct UI detail visibility |
| ★★★ | User-uploaded HTML source | Extracts accurate color values, fonts, spacing |
| ★★ | `web_fetch(url)` | Effective for static sites; React/Vue SPAs often return empty shells |
| ★★ | `image_search(product name + feature module)` | Supplements with publicly available screenshots |
| ★ | `web_search(product name + design blog)` | Supplements with design articles and product documentation |

**When information is insufficient:**
- Dimensions with observable data → analyze normally
- Dimensions that cannot be confirmed → mark the cell as "Insufficient data, pending review" — do not speculate
- Note each product's information source at the top of the report (screenshot / URL fetch / public info)

---

## Step 3 · Multi-Dimensional Comparative Analysis

Once sufficient information has been collected, analyze the following dimensions in order. **Dimensions can be flexibly adjusted based on the actual context.**

### UI-Level Dimensions

| # | Dimension | Analysis Points |
|---|---|---|
| U1 | **Color Scheme** | Primary / secondary / background / functional colors (success/warning/error); contrast ratio; dark/light mode |
| U2 | **Typography** | Font family; heading/body/caption size hierarchy; line height; font weight usage |
| U3 | **Icon Style** | Outlined / filled / mixed; icon size standards; pairing with text |
| U4 | **Layout Structure** | Single / dual / grid column; max width; spacing rhythm (padding/gap); content priority arrangement |
| U5 | **Navigation Design** | Nav type (top/side/bottom); active state indication; number and organization of feature entries |
| U6 | **Interaction Feedback** | Button hover/active/disabled states; loading animation; toast/notification style; state change clarity |
| U7 | **Responsive Adaptation** | Mobile layout changes; breakpoint strategy; touch target minimum size (≥44px) |
| U8 | **Motion & Animation** | Page transitions; component animations; whether excessive or absent; match with interaction rhythm |
| U9 | **Loading States** | Skeleton screen / spinner / progress bar; first-screen loading experience; empty state placeholder design |
| U10 | **Accessibility** | Color contrast (WCAG AA/AAA); focus style visibility; keyboard operability |

### UX-Level Dimensions (based on core interaction flows)

| # | Dimension | Analysis Points |
|---|---|---|
| X1 | **Steps to Complete Task** | How many steps to complete the core task; any redundant steps |
| X2 | **Interaction Path Length** | Number of clicks for key actions; information hierarchy depth; cognitive load |
| X3 | **Page Navigation Count** | Whether users leave the current page mid-task; single-page vs. multi-page flow |
| X4 | **Feedback Timeliness** | Speed of feedback after action; real-time vs. delayed; whether users know the system is responding |
| X5 | **Error Handling Flow** | Error message clarity; guidance for user correction; availability of recovery paths |
| X6 | **Form Complexity** | Number of required fields; input assistance (autocomplete / suggestions / format hints); validation timing |
| X7 | **Confirmation / Undo Options** | Confirmation step before destructive actions; undo or back navigation support |
| X8 | **Shortcut Support** | Bulk actions / quick buttons; keyboard shortcuts; gesture support |
| X9 | **Edge State Guidance** | Empty state design; network error / timeout handling; guidance for edge cases |
| X10 | **Core Feature Entry Visibility** | Primary action button visibility; whether entry points match user habits and mental models |

### "Insights" Column Writing Guidelines

This is the most valuable column in the table. The following rules are mandatory:

- ✅ Synthesize strengths from all competitors and give **specific, actionable recommendations for the target product**
- ✅ Recommendations must be concrete — vague suggestions are not acceptable:
  - ❌ "Improve color contrast"
  - ✅ "Body text color `#8A9A97` has a contrast ratio of ~3.2:1, failing WCAG AA (4.5:1 required). Recommend darkening to `#5F7370`"
- ✅ Clearly distinguish between "must improve" and "worth referencing"
- ✅ Unique strengths of the target product should be explicitly labeled "recommend keeping" in the Insights column

---

## Step 4 · Output Markdown File

After completing the analysis, write the full report to a `.md` file and make it available for download.

### File Structure Template

````markdown
# [Target Product Name] · UI/UX Competitive Analysis Report

**Analysis Date**: YYYY-MM-DD
**Subject**: [Product Name] — [Feature Module]
**Competitors**: [Competitor A], [Competitor B], [Competitor C]
**Goal**: [Goal]
**Information Sources**: [Screenshots / HTML source / URL fetch / Public info]

---

## I. UI-Level Comparison

| Dimension | [Target Product] | [Competitor A] | [Competitor B] | [Competitor C] | Insights |
|---|---|---|---|---|---|
| Color Scheme | | | | | |
| Typography | | | | | |
| Icon Style | | | | | |
| Layout Structure | | | | | |
| Navigation Design | | | | | |
| Interaction Feedback | | | | | |
| Responsive Adaptation | | | | | |
| Motion & Animation | | | | | |
| Loading States | | | | | |
| Accessibility | | | | | |

---

## II. UX-Level Comparison (Core Flow: [Flow Name])

| Dimension | [Target Product] | [Competitor A] | [Competitor B] | [Competitor C] | Insights |
|---|---|---|---|---|---|
| Steps to Complete Task | | | | | |
| Interaction Path Length | | | | | |
| Page Navigation Count | | | | | |
| Feedback Timeliness | | | | | |
| Error Handling Flow | | | | | |
| Form Complexity | | | | | |
| Confirmation / Undo Options | | | | | |
| Shortcut Support | | | | | |
| Edge State Guidance | | | | | |
| Core Feature Entry Visibility | | | | | |

---

## III. Key Findings

### Target Product Unique Strengths (recommend keeping)
- [Strength 1]
- [Strength 2]

### Core Improvement Opportunities

**High Impact** (affects core conversion path or user trust)
- [Improvement]

**Medium Impact** (improves interaction smoothness)
- [Improvement]

**Low Impact** (visual detail polish)
- [Improvement]

---

*This report was generated by Claude based on collected interface screenshots and publicly available information.*
````

### File Output Code

```python
# Generate filename using product name and date
filename = f"competitive-analysis-{product_name}-{date}.md"
output_path = f"/mnt/user-data/outputs/{filename}"

with open(output_path, 'w', encoding='utf-8') as f:
    f.write(report_content)

# Provide download
present_files([output_path])
```

---

## Common Edge Cases

| Scenario | Handling |
|---|---|
| No screenshots provided, only URLs | Try `web_fetch` + `image_search`; note source in report; proceed without prompting for screenshots again |
| Insufficient data for a dimension | Fill cell with "Insufficient data, pending review" — do not speculate |
| More than 4 competitors | Suggest selecting the 3–4 most representative; maintain table readability |
| User requests additional dimensions | Append rows to the table; not limited to default dimensions |
| User requests fewer dimensions | Reduce flexibly based on analysis goal and data availability |
| Mixed sources (some with screenshots, some without) | Note each product's information source at the top of the table; distinguish confidence levels |
