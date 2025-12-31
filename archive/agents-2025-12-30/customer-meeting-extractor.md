# Agent: Customer Meeting Extractor

**Type**: Specialized Agent (Data Processing)
**Scope**: Customer Success Operations
**Phase**: Data Extraction & Intelligence Processing
**Status**: Design (Ready for Phase 2 Implementation)

---

## Purpose

Automatically retrieve customer meetings from Violet Meetings DB and extract structured intelligence using the 13-category CS Intelligence taxonomy. Generates interaction files, extraction files, and routes action items for approval.

---

## When Agent is Invoked

**Trigger 1: New Meeting Added to Violet Meetings DB**
```
IF meeting.status = "Complete"
AND meeting.has_transcript = true
THEN invoke_extractor()
```

**Trigger 2: Manual Invocation**
```
"Extract intelligence from [meeting link/ID]"
"Process all meetings for [customer name] since [date]"
"Extract meeting from Violet Meetings DB for [customer name] on [date]"
```

**Trigger 3: Batch Processing (Scheduled)**
- Daily at 6 AM: Process all meetings from previous day
- Weekly: Process all unprocessed meetings from past week

---

## Responsibilities

### 1. Query Violet Meetings Database

**What Agent Does**:
- Uses Notion MCP to query Violet Meetings DB
- Filters by customer name, date range, completion status
- Retrieves: transcript, attendees, meeting type, date, notes
- Verifies transcript exists (completeness requirement)

**Success Criteria**:
- ✅ Found meeting in database
- ✅ Has full transcript or detailed notes
- ✅ Can identify customer from meeting
- ✅ Know meeting type and date

**If Meeting Not Found**:
- Log error with context
- Notify user: "Meeting not found in Violet Meetings DB"
- Ask for alternative input (link, transcript, date range)

---

### 2. Store Raw Interaction

**What Agent Does**:
- Creates directory: `violet-brain/customers/{customer-slug}/interactions/`
- Writes interaction file: `{YYYY-MM-DD}-{meeting-type}.md`
- Includes: full transcript, attendees, purpose, summary
- Preserves original context and formatting

**File Examples**:
- `2025-12-10-kickoff-call.md`
- `2025-12-05-vitkac-retro.md`
- `2025-12-08-email-thread.md`

**What Agent Checks**:
- ✅ File created successfully
- ✅ All transcript content present
- ✅ Metadata (date, type, attendees) included
- ✅ File is readable and properly formatted

---

### 3. Extract Intelligence (13 Categories)

**What Agent Does**:
- Reads full meeting transcript
- Applies 13-category taxonomy (see skill: customer-meeting-extraction)
- Extracts for each category:
  - **Sentiment**: Overall tone, signals, quotes
  - **Action Items**: With owners, priorities, deadlines
  - **Feature Requests**: With business justification
  - **Bugs**: With severity, reproduction steps, impact
  - **Documentation**: Gaps identified, audience, priority
  - **Process Improvements**: Current issues, proposed solutions
  - **Technical Context**: Stack, integrations, scale, constraints
  - **Stakeholder Context**: Names, roles, relationships
  - **Timeline & Commitments**: Dates, promises, milestones
  - **Expansion Signals**: Growth opportunities, budget mentions
  - **Competitor Intelligence**: Alternatives mentioned, comparisons
  - **Questions & Answers**: Q&A captured, unanswered questions
  - **Key Insights**: Learnings, surprises, validation needs

**How Agent Extracts**:
- Read transcript multiple times for different categories
- Use natural language understanding to identify implicit items
- Extract direct quotes for sentiment and key moments
- Infer ownership when not explicitly stated
- Cross-reference dates and commitments for consistency

**Quality Gates**:
- ✅ Sentiment supported by evidence
- ✅ Action items have clear owners (or marked "TBD")
- ✅ Severity assigned to bugs
- ✅ Business justification for features
- ✅ Risk level matches signals
- ✅ No placeholder text remains

---

### 4. Create Extraction File

**What Agent Does**:
- Creates file: `customers/{customer}/extractions/meetings/{date}-{type}.md`
- Adds YAML front-matter with searchable metadata
- Populates all 13 categories with findings
- Generates routing recommendations
- Includes quality checklist
- Marks status: "Ready for Brenna review"
- Updates `extractions/index.md` and `extractions/meetings/index.md` indices

**File Structure**:
```
[1. Sentiment]
[2. Action Items] → Table with owner/priority/urgency
[3. Feature Requests] → Table with justification
[4. Bugs] → Table with severity
[5. Documentation] → Gaps with audience
[6. Process Improvements] → Current/proposed solutions
[7. Technical Context] → Prose + details
[8. Stakeholder Context] → Names, roles, relationships
[9. Timeline & Commitments] → Dates and promises
[10. Expansion Signals] → Opportunities + potential value
[11. Competitor Intelligence] → Competitors mentioned
[12. Questions & Answers] → Q&A table
[13. Key Insights] → Learnings + surprises

Routing & Next Actions → Approval decisions
Quality Checklist → All items verified
```

**Agent Verifies**:
- ✅ All 13 categories populated (none empty)
- ✅ Tables properly formatted
- ✅ Direct quotes included for sentiment
- ✅ Action items have owners assigned
- ✅ Bugs have severity levels
- ✅ Timeline dates are correct
- ✅ No placeholder text ("TODO", "[ADD]", etc.)
- ✅ Cross-links to interaction file work

---

### 5. Create Notion Entries

**What Agent Does**:
- Creates entry in **CS Extraction Log DB**:
  - ID, date, customer, source type, sentiment, risk level
  - Counts: actions, features, bugs, docs
  - Link to markdown extraction
  - Status: "Pending Review"

- Creates N entries in **CS Approval Queue DB** (1 per action item):
  - Customer name
  - Action type (Task | Bug | Feature | Doc | Process)
  - Description (from extraction)
  - Priority, Urgency (from extraction)
  - Proposed owner (from extraction)
  - Link back to extraction
  - Status: "Pending Approval"

**What Agent Checks**:
- ✅ All Notion entries created successfully
- ✅ Links between entries work
- ✅ Data matches extraction file
- ✅ Extraction Log shows all action items counted
- ✅ Approval Queue ready for Brenna review

---

### 6. Notify Team

**What Agent Does**:
- Sends Slack message or Notion notification to Brenna:
  - Customer name
  - Meeting type and date
  - Quick summary (1-2 sentences)
  - Overall sentiment and risk level
  - Count of action items identified
  - Link to extraction for review
  - Link to approval queue for actions

**Sample Notification**:
```
🤖 Meeting Extraction Ready for Review

**Customer**: Lyst
**Meeting**: Vitkac Integration Retro (2025-12-05)
**Sentiment**: Mixed (constructive lessons-learned)
**Risk Level**: Medium
**Action Items**: 6 identified

→ [Review Extraction](link)
→ [Approve Actions](link)
```

---

### 7. Update Customer Profile

**What Agent Does** (after Brenna approves):
- Updates `customers/{customer}/profile.md`:
  - Adds meeting to interaction history
  - Updates "Last Interaction" date
  - Updates health score if sentiment changed
  - Adds new contacts if discovered
  - Updates open items counts

**Example Update**:
```markdown
## Interaction History

**Total Meetings**: 12
**Last Meeting**: 2025-12-05 - Vitkac Integration Retro

**Recent Interactions** (Last 5):
1. 2025-12-05 - Retrospective - Vitkac integration lessons-learned - [Link]
2. 2025-11-28 - Kickoff Call - Acme Corp onboarding - [Link]
3. ...
```

**What Agent Checks**:
- ✅ Profile file updated
- ✅ Dates are correct
- ✅ New contacts added if discovered
- ✅ Health score reflects sentiment
- ✅ Interaction history properly formatted

---

## Approval Workflow (Brenna's Part)

Agent prepares everything; Brenna makes decisions:

**Brenna Reviews** (in Notion or Slack):
- ✅ **Approve as-is**: Agent creates Linear issues immediately
- ✏️ **Approve with edits**: Agent updates extraction + creates issues
- ❌ **Reject**: Agent logs feedback for learning

**Brenna Can**:
- Change priority of action items
- Change ownership
- Add context or notes
- Reject items that don't align with strategy
- Ask for additional information

**Agent Then**:
- Updates extraction file if modified
- Creates Linear issues for approved items
- Sends confirmation to team
- Logs feedback for future improvements

---

## Implementation Requirements

### Phase 2 Implementation

**Before Agent Can Run**:
- [ ] Identify Violet Meetings DB location and structure
- [ ] Get Notion MCP access to query this database
- [ ] Set up Notion CS Approval Queue DB (if not exists)
- [ ] Get Linear MCP access for creating issues
- [ ] Define customer name mapping (Notion → file system)
- [ ] Test Notion and Linear MCP connections

**Agent Capabilities Needed**:
- [ ] Query Notion databases (Notion MCP)
- [ ] Read structured meeting data (transcript, metadata)
- [ ] Create markdown files (file system)
- [ ] Perform NLP extraction on 13 categories
- [ ] Create Notion database entries (Notion MCP)
- [ ] Create Linear issues (Linear MCP)
- [ ] Send Slack messages (Slack MCP - optional)
- [ ] Handle errors and incomplete data gracefully

### Data Requirements

**From Violet Meetings DB**:
- Meeting title
- Date/time
- Full transcript (required)
- Attendees (names, roles)
- Meeting type/category
- Customer name or ID
- Any summary or notes
- Product(s) involved
- Status (Complete/Draft/Pending)

**From Customer Directory**:
- Customer name (canonical)
- Existing profile.md
- Existing interactions/
- Existing extractions/

### Configuration

**Environment Variables**:
```bash
NOTION_API_KEY=secret_xxxxx
VIOLET_MEETINGS_DB_ID=xxxxx
CS_EXTRACTION_LOG_DB_ID=xxxxx
CS_APPROVAL_QUEUE_DB_ID=xxxxx
LINEAR_API_KEY=secret_xxxxx
SLACK_BOT_TOKEN=xoxb-xxxxx (optional)
```

**Mappings**:
```yaml
customer_name_mapping:
  Lyst: lyst
  Vitkac: vitkac
  Acme Corp: acme-corp
  # ... etc
```

---

## Failure Modes & Recovery

| Issue | What Agent Does | Recovery |
|-------|-----------------|----------|
| Meeting not found in DB | Log error, ask for link/ID | User provides alternative input |
| Transcript incomplete/missing | Skip extraction, notify user | User uploads transcript |
| Can't identify customer | Ask for clarification | User specifies customer name |
| Notion write fails | Log error, retry with exponential backoff | Manual creation as fallback |
| Linear issue creation fails | Log error but mark as processed | Brenna creates issues manually |
| Profile not found | Create new profile from template | Template population automated |
| Sentinel spam (too many actions) | Flag for review, ask if realistic | Brenna approves or rejects items |

---

## Learning & Improvement

**Agent Tracks**:
- Brenna's approvals vs rejections
- Edits made to extractions
- Feedback on category accuracy
- Action item approval rates
- Processing time per meeting

**Uses This Data To**:
- Improve extraction accuracy
- Adjust category definitions
- Better prioritize action items
- Reduce number of items per meeting
- Speed up processing

**Monthly Review**:
- Quality metrics with Brenna
- Categories that need refinement
- Patterns in rejections
- Ready for more autonomy?

---

## Inputs & Outputs

### Inputs (Agent Receives)

**Option 1: Meeting ID/Link**
```
"Extract intelligence from Violet Meetings DB meeting [ID]"
→ Agent queries DB and retrieves meeting
```

**Option 2: Manual Submission**
```
"Process this meeting:
- Customer: Lyst
- Type: Retro
- Date: 2025-12-05
- Transcript: [paste content or file link]"
→ Agent stores interaction and extracts
```

**Option 3: Batch Processing**
```
"Process all meetings for Lyst since 2025-11-01"
→ Agent queries DB for all matching meetings
```

### Outputs (Agent Produces)

**Primary**:
- ✅ `customers/{customer}/interactions/{date}-{type}.md` (raw meeting)
- ✅ `customers/{customer}/extractions/meetings/{date}-{type}.md` (13-category extraction with front-matter)
- ✅ Updated extraction indices: `extractions/index.md` and `extractions/meetings/index.md`

**Secondary**:
- ✅ 1 entry in CS Extraction Log DB
- ✅ N entries in CS Approval Queue DB (1 per action)
- ✅ Slack/Notion notification to Brenna

**Tertiary**:
- ✅ Updated `customers/{customer}/profile.md` (after approval)
- ✅ Created Linear issues (after approval)

---

## Success Criteria

### Extraction Quality
- ✅ Sentiment supported by direct quotes
- ✅ All action items have owners (or "TBD")
- ✅ All bugs have severity levels
- ✅ Feature requests have business justification
- ✅ Risk level matches signals found

### Processing Speed
- ✅ < 5 minutes from meeting input to extraction file
- ✅ < 10 minutes from extraction to Notion entries
- ✅ < 15 minutes total for full processing

### Approval Workflow
- ✅ Brenna can review and approve in <5 minutes
- ✅ 80%+ approval rate (means agent accurate)
- ✅ <20% edit rate (means good quality)
- ✅ <5% reject rate (means alignment)

### Accuracy Metrics
- ✅ Zero missed critical action items
- ✅ Zero missed bugs
- ✅ Correct severity assignments 95%+ of time
- ✅ Correct owner assignments 90%+ of time

---

## Integration with Larger System

```
Violet Meetings DB
    ↓ (Meeting recorded)
    ↓
Customer Meeting Extractor Agent (this agent)
    ├─ Retrieve meeting from DB
    ├─ Store interaction file
    ├─ Extract 13 categories
    ├─ Create extraction file
    ├─ Create Notion entries
    └─ Notify Brenna
    ↓
Brenna (Human Review)
    ├─ Approve actions
    ├─ Modify priorities/owners
    └─ Provide feedback
    ↓
Action Routing Agent (Phase 2)
    ├─ Create Linear issues
    ├─ Route to teams
    ├─ Update customer health
    └─ Generate reports
```

---

## Version History

| Version | Date | Status | Changes |
|---------|------|--------|---------|
| 1.1 | 2025-12-11 | Design | Updated for organized extraction subdirectories (meetings/, roadmaps/, analyses/, reports/) |
| 1.0 | 2025-12-11 | Design | Initial agent definition, ready for Phase 2 implementation |

---

## Related Skills & Documents

- **Skill**: `shared/skills/customer-meeting-extraction.md` - Detailed extraction process
- **Skill**: `shared/skills/customer-success-extraction.md` - 13-category taxonomy reference
- **Agent**: `agents/references/customer-success-intelligence-agent.md` - Larger CS Agent
- **Workflow**: `docs/cs-approval-workflow-brenna.md` - Brenna's approval process
- **Template**: `customers/_templates/extraction-template.md` - Output template

