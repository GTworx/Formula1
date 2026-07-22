**F1 LLM Wiki** knowledge base

Markdown  
\# F1 Knowledge Base (LLM Wiki)

An AI-maintained Formula 1 knowledge base powered by Claude Code.  
Based on Andrej Karpathy's LLM Wiki pattern.

\#\# Purpose

This wiki is a structured, interlinked knowledge base for Formula 1 stats, historical data, and analysis. It covers driver profiles, team histories, race results, season summaries, circuit profiles, and award records. 

Claude maintains the wiki structure and links. The human curates raw data files (race telemetry, season archives, official stat exports), asks questions, and directs deep dives.

\#\# Folder structure

raw/ \-- raw source data: CSVs, PDFs, JSONs, scraped stats (immutable \-- never modify)  
wiki/ \-- markdown pages maintained by Claude  
wiki/index.md \-- master directory / table of contents for the entire wiki  
wiki/log.md \-- append-only log of all ingestion and edit operations

\#\# Entity & Naming Conventions

To keep wiki-links clean and predictable, all page filenames must be lowercase with hyphens:

\*   \*\*Drivers\*\*: \`wiki/drivers/\[driver-slug\].md\` (e.g., \`wiki/drivers/lewis-hamilton.md\`)  
\*   \*\*Teams/Constructors\*\*: \`wiki/teams/\[team-slug\].md\` (e.g., \`wiki/teams/scuderia-ferrari.md\`)  
\*   \*\*Races/Grand Prix\*\*: \`wiki/races/\[year\]-\[gp-slug\].md\` (e.g., \`wiki/races/2023-monaco-gp.md\`)  
\*   \*\*Seasons\*\*: \`wiki/seasons/\[year\]-season.md\` (e.g., \`wiki/seasons/2021-season.md\`)  
\*   \*\*Circuits\*\*: \`wiki/circuits/\[circuit-slug\].md\` (e.g., \`wiki/circuits/silverstone.md\`)  
\*   \*\*Awards & Regulations\*\*: \`wiki/awards/\[award-slug\].md\` or \`wiki/regulations/\[era-slug\].md\`

\#\# Ingest workflow

When you add new race results, telemetry, or stat dumps to \`raw/\` and ask Claude to ingest them:

1\. \*\*Read & Parse\*\*: Claude reads the full source document in \`raw/\`.  
2\. \*\*Key Takeaways\*\*: Claude highlights major takeaways (e.g., podium finishes, championship standings impact, driver penalty decisions) before writing.  
3\. \*\*Source Page\*\*: Claude creates a reference entry in \`wiki/sources/\` named after the file.  
4\. \*\*Update Entity Pages\*\*: Claude creates or updates affected pages:  
   \* \*\*Drivers\*\*: Wins, podiums, points, pole positions, fast laps, head-to-head stats.  
   \* \*\*Teams\*\*: Constructor points, double-podiums, reliability stats.  
   \* \*\*Races & Seasons\*\*: Update race summaries and season standings tables.  
5\. \*\*Cross-Link\*\*: Add \`\[\[wiki-links\]\]\` across all related driver, team, circuit, and race pages.  
6\. \*\*Update Index\*\*: Register all new or substantially updated pages in \`wiki/index.md\`.  
7\. \*\*Log\*\*: Append an entry to \`wiki/log.md\` with the timestamp, source name, and a summary of updated stats.
8\. \*\*Node Colours\*\*: Colour the Team nodes as orange, driver nodes as read, Circuit nodes as blue.

\*Note: Ingesting a single season dataset or race result file may update 20+ driver, team, and circuit pages. This is expected.\*

\#\# Page format

Every wiki page must follow this standardized template:

\`\`\`markdown  
\# \[Entity / Race Name\]

\*\*Summary\*\*: A 1–2 sentence overview of the entity or event (e.g., driver career overview, race outcome, team profile).

\*\*Category\*\*: \[Driver | Team | Race | Season | Circuit | Award\]

\*\*Sources\*\*: List of files in \`raw/\` used for this page.

\*\*Last updated\*\*: YYYY-MM-DD

\---

\#\# Key Overview & Statistics

| Metric | Value |  
| :--- | :--- |  
| \*\*Stat 1\*\* | Value 1 |  
| \*\*Stat 2\*\* | Value 2 |

\#\# Detailed Breakdown

Detailed prose, analysis, and historical context go here.

Use clear headings and link to related entities using \[\[wiki-links\]\] (e.g., \[\[drivers/max-verstappen\]\], \[\[teams/red-bull-racing\]\], or \[\[races/2021-abu-dhabi-gp\]\]).

\#\# Related Pages

\- \[\[drivers/driver-name\]\]  
\- \[\[teams/team-name\]\]  
\- \[\[seasons/year-season\]\]

## **Citation rules**

* Every statistical claim, race result, or penalty record must cite its raw data source.  
* Format citations inline as (source: raw/filename.ext).  
* **Contradictions**: If official sources disagree on a metric (e.g., historic lap timing discrepancies or point re-allocations), document the contradiction explicitly.  
* **Unverified Data**: If historical data is missing or incomplete, tag it with \[needs verification\].

## **Question answering & analysis**

When you ask a question (e.g., *"Who has the highest win percentage at Spa-Francorchamps?"* or *"Compare Hamilton and Verstappen's 2021 season statistics"*):

> 1. Read wiki/index.md to identify relevant pages.  
> 2. Cross-reference individual entity, circuit, and season pages.  
> 3. Synthesize the answer clearly with tables and key statistics.  
> 4. Cite the specific \[\[wiki-links\]\] used to derive the answer.  
> 5. If the answer involves a complex query or analysis not explicitly saved yet, offer to file the query result as a new concept/analysis page in wiki/analysis/.

## **Lint & Audit workflow**

When you request a wiki audit or lint check, Claude will:

* **Check Data Consistency**: Spot conflicting stats across pages (e.g., a driver page listing 103 wins, but team pages totaling 104).  
* **Orphan Pages**: Find pages that have no inbound links from index or entity pages.  
* **Missing Entities**: Detect drivers, teams, or circuits mentioned in race reports that do not yet have dedicated pages.  
* **Broken Links**: Verify that all \[\[wiki-links\]\] resolve to real files in wiki/.  
* **Format Compliance**: Ensure all markdown files follow the standard page template.  
* **Output**: Present a numbered report with actionable fix proposals.

## **Core Rules**

* **Never modify or delete anything in raw/**.  
* Keep filenames strictly lowercase with hyphens (e.g., lewis-hamilton.md).  
* Always update wiki/index.md and wiki/log.md after any edit.  
* Keep statistical updates precise—double-check win, pole, and point totals against raw data before committing.