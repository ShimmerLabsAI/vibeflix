# VibeFlix - AI Coding Session Review Platform

## Project Overview

VibeFlix is a web-based platform for reviewing and analyzing AI-assisted coding sessions (specifically Claude Code sessions). It provides a visual timeline that maps each prompt to its corresponding code changes and visual output, enabling non-technical managers and technical reviewers to evaluate prompting techniques, provide feedback, and learn from successful vibe coding sessions.

## The Problem We're Solving

### Current Pain Points
1. **No Visual Iteration Tracking**: Claude Code session exports (Markdown transcripts) only show prompts and responses—not what the website/app looked like after each iteration
2. **Tedious Manual Review Process**: Technical reviewers must manually:
   - Read through chat transcripts
   - Check out each Git commit
   - Run code locally for each version
   - Compare visual output to prompts
   - Go back and forth repeatedly
3. **Inaccessible to Non-Technical Reviewers**: Managers who need to evaluate team members' prompting skills can't run code locally or use Git
4. **Remote Collaboration Gap**: Teams can't easily review sessions together in real-time
5. **No Learning Repository**: No way to share exemplar sessions for educational purposes

### Who This Is For
- **Non-technical managers** reviewing designers/team members who vibe code
- **Technical managers** who want to evaluate prompting techniques efficiently
- **Learners** wanting to study successful AI coding sessions
- **Teams** collaborating remotely on AI-assisted development
- **Educators** teaching AI-assisted development

## Core Concept: Prompt → Visual Output Timeline

The fundamental innovation is **automatically linking each prompt to its visual result**, creating a scrubable timeline where you can:
- See exactly what the website looked like after prompt #3
- Compare visual outputs between iterations
- Understand which prompts led to which visual changes
- Leave comments on specific prompt-output pairs
- Share sessions with team members

## Key Features

### MVP (Version 1) - 3-4 weeks
**Goal**: Prove the concept works for web development sessions

1. **Session Import**
   - Upload Claude Code session export (Markdown file)
   - Parse prompts, responses, and file changes
   
2. **Git Integration**
   - Connect to GitHub repository
   - Match each iteration to a Git commit
   - Extract code diffs per prompt

3. **Automated Visual Capture** (using Playwright MCP)
   - Automatically screenshot the deployed website after each commit
   - Trigger on Git commit → wait for deployment → capture screenshot
   - Store screenshots linked to prompt IDs

4. **Timeline Interface**
   - Horizontal scrubber showing all iterations
   - Click any point to see: Prompt + Code Diff + Screenshot
   - Side-by-side view of prompt (left) and visual output (right)

5. **Basic Navigation**
   - Previous/Next iteration buttons
   - Jump to specific prompt number
   - Search prompts by keyword

### Version 2 - Collaboration Features
**Goal**: Make it a team review tool

1. **Commenting System**
   - Leave comments on specific prompts (like Figma comments)
   - Mention team members
   - Thread conversations

2. **Multiplayer Features** (using Liveblocks)
   - See who else is viewing the session (presence)
   - Live cursors showing where teammates are looking
   - Real-time comment updates
   - Collaborative review sessions

3. **Session Sharing**
   - Generate shareable links
   - Public/private/team visibility controls
   - Embed sessions in documentation

### Version 3 - Learning Platform
**Goal**: Build a community learning resource

1. **Public Session Library**
   - Browse exemplar sessions by category
   - "How to build X with Claude Code" curated collections
   - Upvote/favorite sessions

2. **Session Analytics**
   - Prompt effectiveness scores
   - Iteration efficiency metrics
   - Context usage analysis
   - Identify prompting patterns

3. **Educational Features**
   - Annotate sessions with teaching notes
   - Create guided walkthroughs
   - Course integration

### Version 4 - Advanced Automation
**Goal**: Zero-friction session capture

1. **Automatic Session Capture** (Ben Newton-inspired)
   - Custom `/vibeflix-log` slash command for Claude Code
   - Automatically triggers after feature completion
   - No manual export needed

2. **Smart Session Detection**
   - Auto-detect when developer finishes a feature
   - Suggest optimal screenshot capture points
   - Batch process multiple commits

3. **Development Log Integration**
   - Structured session summaries (like Ben Newton's `/log-dev`)
   - Accomplishments, architecture changes, next steps
   - Link to VibeFlix timeline

4. **CI/CD Integration**
   - Hook into deployment pipelines
   - Auto-capture previews from Vercel/Netlify
   - Support for PR-based workflows

## Technical Architecture

### Tech Stack (Recommended)
**Frontend**
- Next.js (framework)
- Tailwind CSS (styling)
- Shadcn (components)
- Liveblocks (multiplayer)

**Backend & Database**
- Supabase (database + auth + storage)
- PostgreSQL (session data, comments, users)

**Deployment & Hosting**
- Vercel (frontend hosting)
- Cloudflare R2 (screenshot storage)

**Integrations**
- GitHub API (Git integration)
- Playwright (screenshot automation)
- Anthropic API (session parsing AI assistance)

### Key Components

1. **Session Parser**
   - Parse Claude Code Markdown exports
   - Extract prompts, responses, timestamps
   - Identify file changes and tool uses

2. **Git Connector**
   - Link session iterations to Git commits
   - Fetch code diffs per commit
   - Handle branch/PR workflows

3. **Visual Capture Engine** (Playwright)
   - Deploy code to preview environment
   - Wait for deployment completion
   - Capture full-page screenshots
   - Handle dynamic content loading

4. **Timeline Renderer**
   - Interactive scrubber UI
   - Lazy load screenshots
   - Smooth transitions between iterations
   - Mobile-responsive design

5. **Collaboration Layer** (Liveblocks)
   - Real-time presence
   - Comment synchronization
   - Cursor tracking
   - Notifications

### Database Schema (Initial)

```
sessions
- id
- name
- github_repo_url
- created_at
- user_id
- visibility (public/private/team)

iterations
- id
- session_id
- prompt_number
- prompt_text
- response_text
- commit_sha
- screenshot_url
- timestamp

comments
- id
- iteration_id
- user_id
- content
- position_x
- position_y
- created_at

users
- id
- email
- name
- avatar_url
```

## Inspiration & Prior Art

### Existing Tools (Gaps VibeFlix Fills)
1. **claude-code-viewer** - Shows chat + diffs, but NO visual output
2. **ClaudeDesk** - Web UI with tool timeline, but NO visual output
3. **claude-code-log** - Converts sessions to HTML, but NO visual output
4. **Ben Newton's /log-dev** - Captures screenshots + logs, but:
   - Manual command execution
   - No timeline UI
   - Not designed for review/collaboration
   - Stores locally, not web platform

### What VibeFlix Does Differently
- **Automatic prompt-to-visual mapping** (the core innovation)
- **Web-based review platform** (not local logs)
- **Collaborative features** (multiplayer, comments)
- **Educational focus** (public library, learning resource)
- **Non-technical friendly** (no Git/CLI required for reviewers)

## Use Cases

### 1. Manager Reviewing Designer's Prompting Skills
**Scenario**: Rob (non-technical manager) needs to review Miguel's (designer learning to code) VibeFest website work.

**Workflow**:
- Miguel exports his Claude Code session → uploads to VibeFlix
- VibeFlix automatically links to GitHub repo, captures screenshots
- Rob opens the session, sees timeline of all 24 iterations
- Clicks prompt #3, sees Miguel's prompt was too vague
- Leaves comment: "Need more context here about desired layout"
- Miguel gets notification, improves technique for next session

### 2. Technical Manager Evaluating AI Coding Practices
**Scenario**: Engineering manager wants to ensure team follows best practices.

**Workflow**:
- Team members' Claude Code sessions auto-upload to VibeFlix
- Manager reviews weekly, spot-checks random sessions
- Identifies patterns: junior dev not using enough context
- Shares exemplar session from senior dev as learning resource
- Tracks improvement over time via session analytics

### 3. Learning Platform for AI Coding Education
**Scenario**: Developer wants to learn how to build landing pages with Claude Code.

**Workflow**:
- Browses VibeFlix public library
- Finds "Build a SaaS landing page with Claude Code" session
- Watches prompt-to-visual progression
- Sees effective prompting techniques in action
- Comments with questions, gets answers from community

### 4. Team Collaboration on Design Iterations
**Scenario**: Design team reviewing AI-generated UI options together.

**Workflow**:
- Designer runs multiple Claude Code sessions with different approaches
- Team joins VibeFlix session in real-time
- See each other's cursors, discuss options via comments
- Vote on preferred direction
- Designer continues with chosen approach

## Success Metrics

### MVP Validation
- Can successfully import and parse Claude Code sessions
- Screenshots accurately capture visual state
- Timeline is usable for reviewing 20+ iterations
- At least 2 beta users (Rob + Miguel) use it weekly

### Growth Metrics
- Number of sessions uploaded
- Active reviewers (people viewing sessions)
- Comments per session (engagement)
- Time saved vs. manual review process
- Public session library size

### Educational Impact
- Sessions marked as "learning resources"
- User-reported skill improvement
- Session replays (learning engagement)

## Development Phases (PSB Method)

### Phase 1: PLAN (Days 1-2)
- Answer Avthar's 2 questions (what/why, milestones)
- Create detailed product requirements (this doc!)
- Define technical requirements
- Choose tech stack
- Provision infrastructure (GitHub, Supabase, Vercel)

### Phase 2: SETUP (Days 3-5)
- Create GitHub repo
- Set up CLAUDE.md with project context
- Configure Playwright MCP for screenshots
- Set up automated documentation
- Install necessary plugins
- Create custom slash commands

### Phase 3: BUILD MVP (Days 6-21)
- Week 1: Session parser + Git integration
- Week 2: Screenshot automation + storage
- Week 3: Timeline UI + deployment

### Phase 4: ITERATE (Ongoing)
- Test with Rob + Miguel
- Gather feedback
- Fix bugs
- Add Version 2 features

## Open Questions for Planning

1. **Deployment Automation**: How do we handle different deployment targets (Vercel, Netlify, custom)?
2. **Screenshot Timing**: How long should we wait after deployment before capturing?
3. **Session Size Limits**: What's reasonable max for iterations/screenshots per session?
4. **Privacy**: How do we handle private repos/sensitive code?
5. **Pricing Model**: Free tier? Pay per session? Team plans?

## Next Steps

1. Take this document + Avthar's transcript to Claude Chat
2. Voice brainstorm to refine:
   - MVP scope (what can we cut?)
   - Technical unknowns
   - Risk areas
3. Create final PROJECT_SPEC.md for Claude Code
4. Begin Phase 2 setup

---

## Appendix: Influences & Resources

### Ben Newton's /log-dev System
**What we're borrowing**:
- Automatic screenshot capture via Playwright
- Structured session logging format
- Command-driven approach (user controls when to log)
- Visual progress documentation

**What we're adding**:
- Web platform (vs. local markdown logs)
- Timeline scrubber UI
- Collaborative review features
- Session library

### Avthar's PSB Framework
Using his complete system:
- Phase 1: Plan (2 questions, project spec)
- Phase 2: Setup (7-step checklist)
- Phase 3: Build (research-plan-implement-test)

Following his recommendations:
- GitHub repo from day one
- CLAUDE.md for project memory
- Automated documentation
- Plan mode for complex features
- Issue-based development

### Similar Tools (What Exists)
- **Inspector** (Google AI Studio) - Timeline + scrubbing UI inspiration
- **ClaudeDesk** - Web UI patterns
- **claude-code-log** - Session export format

## Target Timeline

**Week 1**: Planning + Setup
**Weeks 2-4**: Build MVP
**Week 5**: Test with Rob + Miguel, iterate
**Week 6+**: Add collaboration features (Version 2)

**First usable version: ~3-4 weeks**
