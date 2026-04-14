---
name: brain-to-deck
description: "One-click transformation of raw thoughts, uploads, and brain dumps into polished visual deliverables (HTML, PDF, PPTX, images). Use this skill whenever a user wants to turn messy inputs — text notes, images, docx, PDFs, scattered ideas, brainstorms, meeting notes, or any combination — into a cohesive presentation or visual material. Also use when the user uploads a PPTX, PDF, or webpage and asks to extract, learn, or follow its visual style. Trigger on: 'organize my thoughts', 'turn this into a presentation', 'make this presentable', 'visualize my ideas', 'structure this', 'create a deck from these', 'one-pager', 'summarize and visualize', 'use this style', 'learn this template', 'extract the design style', mixed file uploads needing unified output, or raw unstructured text. Also trigger for '展示材料', '整合', '结构化', '可视化', '按这个风格', '提取风格', '学习这个模板'. The bridge between 'stuff in my head' and 'something I can show people'."
---

# Brain-to-Deck: Raw Thoughts → Polished Deliverables

Turn any combination of messy inputs into structured, beautiful output materials. This skill is the bridge between chaotic brain dumps and presentation-ready deliverables.

## Philosophy

People's ideas don't arrive in neat outlines. They come as scattered notes, half-formed thoughts, random screenshots, meeting recordings, PDFs from research, docs from collaborators — a mess of signal mixed with noise. This skill's job is to **extract the signal, find the structure, and produce something beautiful** — all in one shot.

## Workflow Overview

```
[Raw Inputs] → [Ingest & Parse] → [Analyze & Structure] → [Design & Render] → [Output]
```

### Step 1: Ingest Everything

Read ALL uploaded files and user-provided text. Use the right tool for each:

| Input Type | How to Read |
|-----------|-------------|
| Plain text / markdown in chat | Already in context — just use it |
| `.docx` | Read the docx skill: `/mnt/skills/public/docx/SKILL.md` |
| `.pdf` | Read the pdf-reading skill: `/mnt/skills/public/pdf-reading/SKILL.md` |
| `.pptx` | `python -m markitdown file.pptx` (install markitdown if needed) |
| `.xlsx` / `.csv` | Read with pandas or openpyxl |
| Images (`.png`, `.jpg`, etc.) | View them directly — describe what you see, extract any text |
| `.txt` / `.md` | `cat` or `view` tool |

Check `/mnt/user-data/uploads/` for all uploaded files. Don't skip anything — every piece of input matters.

### Step 2: Analyze & Structure

This is the most critical intellectual step. You are not just reformatting — you are **thinking**.

1. **Extract key themes**: What are the 3-7 core ideas across all inputs?
2. **Find the narrative**: What story connects these ideas? What's the logical flow?
3. **Identify hierarchy**: What's the big picture vs. supporting details?
4. **Spot data**: Are there numbers, comparisons, timelines, or relationships that deserve visualization?
5. **Catalog visuals**: Which uploaded images should be included? What new visuals should be created?

Present your proposed structure to the user before rendering, unless they've said "just do it" or the intent is obvious. A quick outline like:

```
Here's how I'd structure this:
1. [Title/Hook] — ...
2. [Core Problem/Context] — ...
3. [Key Insight A] — ...
4. [Key Insight B] — ...
5. [Data/Evidence] — ...
6. [Conclusion/Call to Action] — ...

Should I proceed, or would you like to adjust?
```

### Step 3: Choose Output Format

If the user specified a format, use it. Otherwise, recommend based on context:

| Use Case | Recommended Format | Why |
|----------|-------------------|-----|
| Sharing in meetings, presenting to team | **PPTX** | Standard presentation format, editable |
| Sharing via link, interactive content | **HTML** | Rich interactivity, works everywhere |
| Printing, formal documents, reports | **PDF** | Print-ready, professional |
| Social media, quick visual summary | **Image (HTML→screenshot)** | Single visual, easy to share |
| User says "展示材料" / "展示" without specifics | **HTML** first (richest output) | Most visual impact |

### Step 4: Design & Render

Read the appropriate downstream skill BEFORE generating:

- **PPTX** → Read `/mnt/skills/public/pptx/SKILL.md` (and its sub-references like `pptxgenjs.md`)
- **PDF** → Read `/mnt/skills/public/pdf/SKILL.md`
- **HTML** → Read `/mnt/skills/public/frontend-design/SKILL.md`
- **DOCX** → Read `/mnt/skills/public/docx/SKILL.md`

Then **ALWAYS read the active design reference file** before generating ANY output:

→ **Default: Read `references/bain-style.md`** (relative to this skill's directory)
→ **If the user has created a custom style and specified to use it: Read `references/[custom-name]-style.md` instead**

The `references/` directory can contain multiple style files. `bain-style.md` is the built-in default and should never be modified or deleted. When a user extracts a new style (see "Style Extraction" below), it is saved as a separate file alongside `bain-style.md`. The user chooses which style to use by saying things like "用我的 [name] 风格" or "switch to [name] style". If the user doesn't specify, always default to `bain-style.md`.

The active style file is the single source of truth for all visual decisions. Do not deviate from it.

Key principles from the design reference (read the full file for specifics):

- **Action titles, not topic titles** — every slide title is a complete sentence stating the conclusion
- **One message per slide/section** — if you have two insights, make two slides
- **Pyramid Principle** — lead with the answer, then supporting arguments, then evidence
- **Bain Red (`CC2229`) as accent only** — white background, grayscale dominant, red to spotlight
- **Arial everywhere** — no decorative fonts, no emoji, no icons
- **Charts: column/bar, waterfall, line** — never pie charts, never 3D, never donut
- **Callout boxes on charts** — annotate the key insight directly on the chart
- **Source lines on every data slide** — bottom-left, 7-8pt gray
- **No decoration** — no gradients, shadows, glow, rounded corners, or animated transitions

### Step 5: Quality Check

After generating:

1. **Content check**: Does the output capture ALL the key ideas from the inputs? Did anything get lost?
2. **Flow check**: Does the narrative make sense from start to finish?
3. **Visual check**: Convert to images and inspect. Fix any overlaps, cut-off text, or alignment issues.
4. **Fidelity check**: Does it sound like the user, or did it drift into generic corporate speak?

## Special Handling

### Mixed-Language Content
If inputs contain multiple languages, produce the output in the dominant language unless the user specifies otherwise. Preserve key terms in their original language when they're domain-specific.

### Data-Heavy Inputs
When inputs contain lots of numbers/data (spreadsheets, reports with stats):
- Create dedicated data visualization slides/sections
- Use chart types appropriate to the data (comparison → bar chart, trend → line chart, composition → pie/donut, relationship → scatter)
- Always include the actual numbers alongside visualizations

### Image-Heavy Inputs
When the user uploads many images:
- Create a visual-forward layout (gallery, grid, or full-bleed images)
- Use images as section backgrounds or hero elements
- Add captions extracted from context

### Minimal Input ("Just some ideas")
When the user gives you very little to work with:
- Ask 1-2 clarifying questions about audience and purpose
- Expand on their ideas with reasonable assumptions
- Note your assumptions in the output so they can correct

## Style Extraction: Learn a Visual Style from Reference Material

This is a separate workflow from the main brain-to-deck flow. It triggers when the user uploads a PPTX, PDF, or webpage screenshot and asks you to "learn this style", "use this template's style", "follow this format", or similar. The goal is to **reverse-engineer a complete visual design specification** from the reference material and save it as a new style reference file.

### When to trigger

- User uploads a `.pptx` and says "以后按这个风格来" / "用这个模板的风格"
- User uploads a `.pdf` and says "学一下这个排版" / "follow this style"
- User shares a URL or screenshot and says "我喜欢这个网页的风格"
- User says "帮我提取这个 PPT 的设计风格" / "识别一下这个模板的视觉规范"

### Step-by-step: Extracting a style

#### 1. Ingest the reference material

| Source | How to extract visuals |
|--------|----------------------|
| `.pptx` | Convert to images: `python scripts/office/soffice.py --headless --convert-to pdf file.pptx` then `pdftoppm -jpeg -r 200 file.pdf slide`. Also extract text with `python -m markitdown file.pptx` for content structure analysis. |
| `.pdf` | Convert to images: `pdftoppm -jpeg -r 200 file.pdf page`. Also extract text with `pdftotext -layout file.pdf`. |
| Image / screenshot | View the image directly. |
| URL (webpage) | Use `web_fetch` to get the HTML, or ask the user for a screenshot. |

**View at least 3-5 pages/slides** to identify consistent patterns vs. one-off choices.

#### 2. Analyze and extract these dimensions

Go through each dimension systematically. For every dimension, describe **what you observe** with specific values, not vague descriptions.

**Color Palette:**
- Identify the exact hex values used. Use a methodical approach: look at backgrounds, text, headings, accent elements, chart colors, table headers.
- Determine the roles: which color is the primary accent? Which is the background? How many distinct grays are used for text hierarchy?
- Note the ratio: is the palette warm or cool? High contrast or low contrast? How much white space vs. colored area?

**Typography:**
- Identify font families (or closest match if custom fonts are used). Look at headings vs. body vs. captions — are they the same family or different?
- Note exact sizes for each text level (title, subtitle, body, caption, footnote).
- Note weight usage: where is bold used? Where is regular? Is italic used anywhere?
- Note alignment: left-aligned? Center? Justified?
- For PPTX: measure approximate sizes from the slide images if text extraction doesn't reveal font metadata.

**Layout Patterns:**
- What is the slide/page anatomy? Where does the title sit? Where is the main content area?
- Are there accent bars, dividers, or decorative elements? Where and how large?
- What is the margin/padding system? Generous or tight?
- How are multi-element slides laid out? (chart+bullets side by side? Full-width chart? Grid of cards?)
- Is there a consistent header/footer structure?

**Chart & Data Visualization:**
- What chart types are used? (bar, line, pie, etc.)
- How are charts colored? What's the primary data series color vs. secondary?
- Are there data labels, callout boxes, annotations?
- Grid lines: visible or hidden? Color?
- Legend placement: bottom, right, or none?

**Tables:**
- Header row style: background color, text color, bold?
- Body row style: alternating stripes? Border style?
- Cell padding: tight or generous?

**Content Writing Style:**
- Are titles descriptive ("Market Analysis") or action-oriented ("Revenue declined 12%")?
- Bullet point density: few key bullets or dense text?
- Is there a source line / footnote convention?
- Page numbers: present? Position? Style?

**Decorative Elements:**
- Accent lines, shapes, icons, logos?
- Shadows, rounded corners, gradients?
- Background patterns or textures?

#### 3. Generate the style reference file

Produce a markdown file that follows the **exact same structure** as `references/bain-style.md`. Use it as a template — same sections, same table formats, same level of specificity. The output file must include:

1. **Core Philosophy** — a one-sentence summary of the style's personality
2. **Color Palette** — table with Role, Name, Hex, Usage for every color identified
3. **Typography** — tables for PPTX, HTML, and PDF with Element, Font, Size, Weight, Color
4. **Slide/Page Structure** — ASCII diagram showing the anatomy
5. **Charts & Data Visualization** — preferred types, color rules, formatting rules
6. **Layout Patterns** — named layouts with descriptions
7. **Tables** — header/body/border specifications
8. **Content Writing Rules** — title style, bullet conventions, source lines
9. **What to NEVER Do** — based on what the reference material avoids
10. **Format-Specific Notes** — implementation notes for PPTX, HTML, PDF

#### 4. Save as a new style (do NOT overwrite bain-style.md)

Save the new style file to `references/` with a descriptive name:
```
references/[style-name]-style.md
```

**Important:** `bain-style.md` is the built-in default and must never be overwritten or deleted. The new style file lives alongside it. The user can switch between styles at any time.

After saving, tell the user:

```
已从你的模板中提取了完整的视觉规范，保存为 references/[name]-style.md。

提取的核心要素：
- 配色：[primary] + [secondary] + ...
- 字体：[heading font] / [body font]
- 标题风格：[action title / topic title / ...]
- 图表：[preferred types]
- 特征元素：[accent bars / icons / ...]

现在你有两套风格可选：
1. bain-style（默认）— Bain 咨询风格
2. [name]-style（新建）— 基于你上传的模板

说"用 [name] 风格"即可切换。要调整细节可以直接告诉我。
```

### Tips for better extraction

- **Multiple pages are essential.** One slide might have a one-off layout. Look for what's *consistent* across 3+ slides.
- **Distinguish brand colors from data colors.** The accent color used in titles is different from the chart palette.
- **Note what's absent.** If the reference uses no icons, no shadows, no rounded corners — that's part of the style. Capture it in the "NEVER" list.
- **When fonts can't be identified precisely,** pick the closest widely-available match and note it: "Appears to be [X] or similar geometric sans-serif."
- **When extracting from a webpage,** inspect the CSS if possible (`web_fetch` the HTML) for exact values. Otherwise estimate from the screenshot.

## Output Delivery

Always save final output to `/mnt/user-data/outputs/` and present via `present_files`.

If you generated an HTML file, also mention they can open it in a browser for the full interactive experience.

If the user might want multiple formats, offer: "I've created this as [format]. Want me to also export it as [other format]?"

## Example Scenarios

**Scenario 1**: User pastes meeting notes + uploads a spreadsheet
→ Extract action items and data → Create a PPTX deck with meeting summary, key decisions, data charts, and next steps.

**Scenario 2**: User uploads 5 research PDFs + types "make me a literature review presentation"
→ Read all PDFs → Identify themes, agreements, contradictions → Create an HTML presentation with sections per theme, citation cards, and a synthesis conclusion.

**Scenario 3**: User uploads photos from a trip + types "make a travel memory book"
→ View all photos → Organize chronologically or thematically → Create a beautiful HTML page with photo gallery, captions, and story flow.

**Scenario 4**: User types a stream-of-consciousness brain dump about a startup idea
→ Extract the core value proposition, target market, competitive advantage → Create a one-pager PDF or pitch deck PPTX.

**Scenario 5**: User uploads a PPTX template + says "以后按这个风格来"
→ Convert to images → Analyze color palette, typography, layout patterns, chart styles → Generate a new style reference file → All future outputs automatically follow this style.
