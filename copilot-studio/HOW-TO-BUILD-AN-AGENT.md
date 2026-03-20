# How to Build a Copilot Studio Agent

A one-pager for anyone on the team who wants to create or customize an AI agent in Microsoft Copilot Studio.

---

## What Is a Copilot Studio Agent?

An agent is an AI assistant that your team can message in Microsoft Teams. It follows instructions you give it, answers questions using knowledge sources you connect, and takes actions through tools you configure.

**Think of it as:** A smart team member that's always available, knows your documents, and can do things like draft emails, analyze data, or post updates — all through a Teams chat.

---

## The 5 Building Blocks

Every Copilot Studio agent is made of 5 things:

```
┌─────────────────────────────────────────────────┐
│                YOUR AGENT                        │
│                                                  │
│  1. INSTRUCTIONS ─── What the agent does         │
│  2. KNOWLEDGE ────── What the agent knows        │
│  3. TOOLS ────────── What the agent can do       │
│  4. TRIGGERS ─────── When the agent acts alone   │
│  5. CHANNELS ─────── Where people use it         │
│                                                  │
└─────────────────────────────────────────────────┘
```

### 1. Instructions (Required)

Plain-text directions telling the agent its role, tone, and behavior. Up to 8,000 characters.

**What to include:**
- What the agent helps with ("You help the team build presentations and draft emails")
- When to use specific knowledge ("When asked to build a deck, follow the deck-builder knowledge source")
- Tone and style ("Be direct and concise. Lead with the answer, not background.")
- What NOT to do ("Never send emails without user review")

**Where:** Overview page > Instructions > Edit

### 2. Knowledge (Required)

Documents, files, and data the agent can reference when answering questions.

| Knowledge Type | Good For | How to Add |
|---------------|---------|-----------|
| **Uploaded files** (.md, .txt, .pdf, .docx) | SKILL.md files, style guides, process docs | Knowledge > Add > Upload Files |
| **SharePoint sites** | Team documents, templates, past reports | Knowledge > Add > SharePoint |
| **Public websites** | Microsoft Learn, product documentation | Knowledge > Add > Public Websites |
| **Dataverse tables** | CRM data, program records, OKRs | Knowledge > Add > Dataverse |
| **Copilot connectors** | External systems (Jira, Salesforce, etc.) | Knowledge > Add > Copilot Connector |

**Tip:** Upload SKILL.md files from this repo as knowledge. Each one teaches the agent a specific capability.

### 3. Tools (Optional but Powerful)

Actions the agent can take — not just generate text, but actually DO things.

| Tool Type | Examples | How to Add |
|-----------|---------|-----------|
| **Agent Flow** (Power Automate) | Send email, post to Teams, write to Excel, create calendar event | Tools > Add > Agent Flow > New |
| **MCP Server** | Query GitHub, search Microsoft Learn, access Dataverse | Tools > Add > Model Context Protocol |
| **Custom Connector** | Call any REST API | Tools > Add > Custom Connector |
| **Prompt** | Generate specific content using a template | Tools > Add > Prompt |
| **Computer Use** (Preview) | Interact with desktop applications | Tools > Add > Computer Use |

**When you need tools:** If your agent just answers questions → knowledge is enough. If your agent needs to take action (send, post, create, update) → you need tools.

### 4. Triggers (Optional)

Make the agent act autonomously — without someone messaging it.

| Trigger Type | Example |
|-------------|---------|
| **Schedule** | Every Monday at 9am, generate a standup and post to Teams |
| **Email received** | When an email from a partner arrives, summarize and flag |
| **Dataverse record changed** | When a program status changes, notify the team |
| **SharePoint file uploaded** | When a new doc appears, analyze and summarize |

**Where:** Topics > Add > Trigger (then edit in Power Automate for conditions)

### 5. Channels (Required for Deployment)

Where people interact with the agent.

| Channel | Audience | How to Deploy |
|---------|----------|--------------|
| **Microsoft Teams** | Your team | Publish > Deploy to Teams |
| **Web chat** | External users | Publish > Demo Website or embed |
| **Microsoft 365 Copilot** | M365 Copilot users | Publish > Microsoft 365 Copilot |

---

## Build Your First Agent in 15 Minutes

### Step 1: Create (2 minutes)
1. Go to [copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)
2. Click **Agents** > **New Agent**
3. Describe what you want: "An assistant that helps my marketing team build presentations and prep for meetings"
4. Copilot Studio generates a name, description, and instructions for you
5. Review and adjust

### Step 2: Add Knowledge (5 minutes)
1. Go to **Knowledge** > **Add Knowledge** > **Upload Files**
2. Upload SKILL.md files from this repo (start with deck-builder, exec-summary, talking-points)
3. Wait for each to show **Ready** status

### Step 3: Write Instructions (3 minutes)
1. Go to **Overview** > **Instructions** > **Edit**
2. Copy instructions from [`copilot-studio/agent-instructions.md`](agent-instructions.md)
3. Customize for your team

### Step 4: Test (3 minutes)
1. Click **Test** in the top-right corner
2. Try: "Build me a stakeholder briefing deck on our Q3 results"
3. Try: "Prep me for my meeting with my director tomorrow"
4. Adjust instructions if the responses aren't right

### Step 5: Deploy (2 minutes)
1. Click **Publish** > **Publish**
2. Go to **Channels** > **Microsoft Teams**
3. Share with your team

---

## Adding Power Automate Flows (Tools)

When your agent needs to DO something (not just answer), build a flow:

### Example: "Post to Teams Channel" Flow

1. In your agent, go to **Tools** > **Add Tool** > **Agent Flow** > **New**
2. Name: "Post to Teams Channel"
3. **Trigger:** When called by the agent
4. **Inputs:** `channel_name` (text), `message_content` (text)
5. **Action:** Microsoft Teams > Post Message in a Channel
   - Team: [Your Team]
   - Channel: [Selected from input]
   - Message: [From input]
6. Save and publish the flow
7. The agent can now post to Teams when instructed

### Example: "Log to Excel" Flow

1. **Trigger:** When called by the agent
2. **Inputs:** `spreadsheet_name` (text), `row_data` (text)
3. **Action:** Excel Online > Add a Row to a Table
   - Location: SharePoint
   - Document Library: [Your library]
   - File: [From input]
   - Row: [From input]
4. Save and publish

---

## Connecting MCP Servers

MCP servers give your agent live access to external data and tools.

### To connect an MCP server:
1. Go to **Tools** > **Add Tool** > **Model Context Protocol**
2. Follow the MCP onboarding wizard
3. Select the server and authorize

### Recommended MCP servers for marketing teams:
1. **Microsoft Learn** — Ground content in official documentation
2. **GitHub** — Access issues, project boards, and code
3. **Dataverse** — Query business data and CRM records
4. **Power BI** — Pull dashboard metrics and run DAX queries

---

## Security Checklist

Before deploying any agent:

- [ ] **Authentication:** Agent uses Microsoft Entra ID (default for Teams)
- [ ] **No confidential data in knowledge:** Don't upload salary data, HR files, or NDA-protected content
- [ ] **Tool permissions scoped:** Flows only access what they need (least privilege)
- [ ] **Test before publish:** Run 5-10 real queries in the test pane
- [ ] **No external sharing:** Agent deployed to internal Teams only (not public web)
- [ ] **Data stays in tenant:** MCP servers and flows operate within your M365 tenant

---

## Need Help?

- **Copilot Studio documentation:** [learn.microsoft.com/microsoft-copilot-studio](https://learn.microsoft.com/microsoft-copilot-studio)
- **Agent deployment checklist:** [M365 agents checklist](https://learn.microsoft.com/copilot/microsoft-365/agent-essentials/m365-agents-checklist)
- **Power Automate flows:** [learn.microsoft.com/power-automate](https://learn.microsoft.com/power-automate)
- **MCP server setup:** [MCP onboarding wizard in Copilot Studio](https://learn.microsoft.com/microsoft-copilot-studio/mcp-add-existing-server-to-agent)
- **Questions about this skills repo or agent setup:** Contact your team lead

---

*This guide is part of the [Solution Marketing Skills](https://github.com/akiyaani2/solution-marketing-skills) repo.*
