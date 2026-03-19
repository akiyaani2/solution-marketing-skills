---
name: competitive-scan
description: Structured competitive intelligence scanning — compare your skilling, events, hackathon, and content programs against AWS, GCP, and other cloud competitors. Use when preparing competitive briefs, positioning content, or justifying program investment.
---

# Competitive Scan

Structured competitive analysis for solution marketing programs. Compare your programs against AWS, GCP, and other cloud/tech competitors.

## Trigger Phrases

- `/competitive-scan`
- `/competitive-scan --aws`
- `/competitive-scan --hackathons`
- `/competitive-scan --certifications`
- `what's AWS doing in skilling`
- `competitive landscape for [topic]`
- `how do we compare to GCP on [area]`

## What This Skill Does

### 1. Full Competitive Landscape (`/competitive-scan`)

Scan across all competitive dimensions and produce a structured brief:

**Research these areas** (use web search and Microsoft Learn MCP):

| Dimension | What to Compare |
|-----------|----------------|
| **Skilling programs** | Learning platforms, free tiers, certification paths, lab environments |
| **Hackathons & events** | Developer hackathons, partner hacks, community events, conference presence |
| **Certifications** | New certs launched, pricing changes, free cert promotions, pass rates |
| **Content** | Blog cadence, video series, social presence, developer advocacy |
| **Partnerships** | NVIDIA, Databricks, Snowflake, MongoDB — who's partnering with whom |
| **Community** | Discord/Slack communities, GitHub presence, Stack Overflow activity |
| **Developer tools** | Free tiers, SDKs, CLI tools, playground environments |

### 2. Per-Competitor Deep Dive (`/competitive-scan --aws` or `--gcp`)

**AWS Skill Builder focus areas:**
- New learning paths and courses
- Free tier changes
- AWS GameDay / Jam events
- re:Invent and Summit skilling tracks
- AWS Community Builder program changes
- Partner skilling (AWS Partner Network training)

**GCP Skills Boost focus areas:**
- Google Cloud Skills Boost updates
- Cloud Digital Leader and Professional cert changes
- Google Cloud Next skilling tracks
- Google Developer Groups (GDG) events
- Innovators Plus subscription changes
- Gemini-related skilling content

### 3. Domain-Specific Scan (`/competitive-scan --hackathons`)

Focus on one domain across all competitors:

**Hackathon comparison:**
| Dimension | Your Team | AWS | GCP | Others |
|-----------|-----------|-----|-----|--------|
| Active hack programs | | | | |
| Annual hack count | | | | |
| Average participation | | | | |
| Platform used | | | | |
| Prize structure | | | | |
| Partner involvement | | | | |
| Follow-up engagement | | | | |

### 4. Competitive Brief (`/competitive-scan --brief`)

Generate a 1-page competitive brief for leadership:

```markdown
## Competitive Brief — [Date]

### Market Position Summary
[1 paragraph — where we stand overall]

### Key Moves This Quarter
| Competitor | Move | Impact on Us | Recommended Response |
|-----------|------|-------------|---------------------|

### Opportunities We Should Exploit
1. [Competitor gap we can fill]
2. [Market trend we're positioned for]
3. [Partner opportunity]

### Threats to Monitor
1. [Competitor strength]
2. [Market shift]

### Recommendation
[What should we do differently based on this scan?]
```

## Research Method

1. **Start with web search** for recent announcements (last 90 days):
   - "[competitor] developer skilling 2026"
   - "[competitor] hackathon program"
   - "[competitor] certification changes"
   - "[competitor] developer events"

2. **Check official docs** via Microsoft Learn MCP to verify our own claims:
   - Ensure we're comparing accurately
   - Identify our features that competitors don't have

3. **Check community signals**:
   - Stack Overflow trends for technology adoption
   - GitHub trending repos for developer interest
   - Reddit/HackerNews sentiment

4. **Synthesize into actionable intelligence** — not just "here's what they're doing" but "here's what we should do about it"

## Output Format

```markdown
## Competitive Scan — [Date]

### Executive Summary
[3-5 sentences on competitive landscape]

### Detailed Comparison
| Dimension | Us | AWS | GCP | Assessment |
|-----------|-----|-----|-----|-----------|

### Recent Competitor Moves (Last 90 Days)
| Date | Competitor | Move | Impact |
|------|-----------|------|--------|

### Our Competitive Advantages
1. [advantage with evidence]

### Our Competitive Gaps
1. [gap with recommendation]

### Recommended Actions
1. [action] — [owner] — [timeline]
```

## Cadence

- **Quarterly**: Full competitive scan across all dimensions
- **Monthly**: Quick scan for recent competitor moves
- **Ad hoc**: Before major decisions, program launches, or leadership presentations

## Gotchas

- Competitive intelligence goes stale fast. Always include the scan date and note when findings may be outdated.
- Don't just list what competitors do — explain what it means for your team and what to do about it.
- Be honest about gaps. "We don't have an equivalent to [competitor feature]" is more useful than pretending it doesn't matter.
- Verify your own claims. Sometimes our marketing says things our product doesn't yet deliver.
- Focus on programs, not products. This is for solution marketing teams, not product teams.
- Use Microsoft Learn MCP to ground Azure-specific claims in official documentation.
