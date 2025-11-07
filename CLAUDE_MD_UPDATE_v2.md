# Update for ~/.claude/CLAUDE.md

## Replace the entire "Linear Integration (linctl)" section with this:

---

### Linear Integration (linctl)
**Status**: ✅ Use linctl CLI for all operations; ❌ Linear MCP disabled to save context

**Tool Selection Strategy:**
Use linctl CLI for all Linear operations. For reads in agents, prefer `--json`.

**Common Operations (linctl)**

**Issues** (Epic 2: Enhanced with label management)
```bash
# Create issue (with optional project assignment and labels)
linctl issue create --title "Fix bug" --team "$LINEAR_TEAM" --priority 3
linctl issue create --title "New feature" --team RAE --project PROJECT-UUID
linctl issue create --title "Critical bug" --team RAE --label "bug,urgent,backend"

# Update issue (including project assignment)
linctl issue update RAE-123 --state "In Progress"
linctl issue update RAE-123 --project PROJECT-UUID
linctl issue update RAE-123 --project ""  # Remove from project

# Label Management (Epic 2 - INTELLIGENT LOOKUP)
# Set labels exactly (replaces all existing labels)
linctl issue update RAE-123 --label "bug,urgent"
linctl issue update RAE-123 --label ""  # Clear all labels

# Incremental label operations
linctl issue update RAE-123 --add-label "backend"           # Add without removing others
linctl issue update RAE-123 --remove-label "frontend"       # Remove specific label
linctl issue update RAE-123 --add-label "api,database"     # Add multiple
linctl issue update RAE-123 --remove-label "deprecated,old" # Remove multiple

# Precedence: if --label is provided, --add-label and --remove-label are ignored
linctl issue update RAE-123 --label "bug" --add-label "urgent"  # Only "bug" is set

# Label lookup features:
# - Accepts label names or IDs
# - Trims whitespace automatically
# - Deduplicates labels
# - Suggests closest matches if not found

# Comment
linctl comment create RAE-123 --body "Status update"

# List
linctl issue list --assignee me --newer-than 2_weeks_ago
```

**Projects** (Epic 1: Complete project lifecycle management)
```bash
# List projects
linctl project list
linctl project list --team RAE --state started
linctl project list --include-completed

# Get project details
linctl project get PROJECT-UUID
linctl project get PROJECT-UUID --json

# Create project (ENHANCED - NEW FIELDS)
linctl project create --name "Q1 Backend" --team RAE
linctl project create --name "Launch" --team PROD --state planned --priority 2
linctl project create --name "Mobile App" --team APP --lead john@company.com --summary "iOS app redesign"
linctl project create --name "Infrastructure" --team ENG \
  --lead me --priority 1 \
  --summary "Scale backend for growth" \
  --description "Focus on performance and reliability" \
  --labels "backend,infrastructure" \
  --start-date 2025-11-01 --target-date 2025-12-31

# Update project (ENHANCED - MULTI-FIELD SUPPORT)
linctl project update PROJECT-UUID --name "New Name"
linctl project update PROJECT-UUID --state started --priority 1
linctl project update PROJECT-UUID --description "Updated description"
linctl project update PROJECT-UUID --lead jane@company.com --labels "backend,api"

# Archive project
linctl project archive PROJECT-UUID
```

**Milestones** (Epic 1 Story 1.2: Project milestone tracking)
```bash
# Create milestone
linctl milestone create --project PROJECT-UUID --name "Phase 1" --target-date 2025-12-31
linctl milestone create --project PROJECT-UUID --name "MVP Release" --description "Core features complete"

# List milestones
linctl milestone list PROJECT-UUID
linctl milestone list PROJECT-UUID --include-archived

# Get milestone details
linctl milestone get MILESTONE-UUID
linctl milestone get MILESTONE-UUID --json

# Update milestone
linctl milestone update MILESTONE-UUID --name "Updated Name"
linctl milestone update MILESTONE-UUID --target-date 2025-12-15
linctl milestone update MILESTONE-UUID --description "New description"

# Delete milestone (archives it)
linctl milestone delete MILESTONE-UUID
```

**Project Updates** (Epic 1 Story 1.3: Status communication with health tracking)
```bash
# Create project update post with health tracking
linctl project update-post create PROJECT-UUID --body "Completed authentication module" --health onTrack
linctl project update-post create PROJECT-UUID --body "Blocked by API delays" --health atRisk
linctl project update-post create PROJECT-UUID --body "Sprint complete, ahead of schedule" --health onTrack

# Health values: onTrack | atRisk | offTrack

# List project updates
linctl project update-post list PROJECT-UUID
linctl project update-post list PROJECT-UUID --json

# Get specific update
linctl project update-post get UPDATE-UUID
```

**Helper Functions**
```bash
# Create issue + attach to project by name (helper in ~/.zshrc)
lnewp "My new issue" "Agent Customisation" --priority 2 --assign-me
```

**Issue State Workflow**:
```
Backlog → Todo → In Progress → In Review → Done
```

**Project State Values**:
```
planned → started → paused → completed | canceled
```

**Priority Levels** (only set when explicitly requested):
- 0 = No priority (default)
- 1 = Urgent
- 2 = High
- 3 = Normal
- 4 = Low

**Health Status** (for project updates):
- onTrack = Project is on schedule
- atRisk = Potential delays or issues
- offTrack = Behind schedule, needs attention

**Label Management Features** (Epic 2):
- Intelligent name-to-ID lookup with fuzzy matching
- Automatic whitespace trimming and deduplication
- Suggestions for closest matches when label not found
- Three operation modes:
  - `--label`: Set labels exactly (replace all)
  - `--add-label`: Add labels incrementally
  - `--remove-label`: Remove labels incrementally
- Clear precedence: `--label` overrides `--add-label`/`--remove-label`

**Communication Strategy**:
- Progress updates → Linear comments (not docs)
- Test results → Linear comments (not docs)
- Implementation findings → Linear comments (not docs)
- Project health → Project update posts with health indicator
- Code changes → Commit messages + Linear issue link

**Git Integration**:
- Link commits: `RAE-123: Description`
- Update Linear state after commits
- Close issues when merged to main

---
