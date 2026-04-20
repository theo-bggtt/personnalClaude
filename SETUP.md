# Claude Code — Réplication du setup de leroy (auto-contenue)

Ce fichier est **auto-contenu** : il inclut tout le contenu nécessaire pour reproduire à l'identique mon environnement Claude Code sur une nouvelle machine.

**Pour un agent Claude qui lit ce fichier** : pour chaque bloc délimité par `<<<FILE_START>>>` et `<<<FILE_END>>>`, écris le contenu tel quel dans le chemin indiqué par l'en-tête `### File:`. Ne modifie rien. Ne réinterprète pas les code-fences internes.

> Les chemins `~` désignent le home utilisateur (ex. `C:\Users\<nom>` sous Windows).

---

## 0. Prérequis

- Node.js LTS (≥ 20), Git installés
- Claude Code CLI installé et authentifié (`claude login`)
- Le répertoire `~/.claude/` existe (créé au premier lancement)

---

## 1. Paramètres globaux

### File: `~/.claude/settings.json`

<<<FILE_START>>>
{
  "model": "opus",
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "bash ~/.claude/scripts/project-bootstrap.sh"
          }
        ]
      }
    ]
  },
  "enabledPlugins": {
    "superpowers@claude-plugins-official": true,
    "context7@claude-plugins-official": true,
    "frontend-design@claude-plugins-official": true,
    "claude-seo@agricidaniel-seo": true,
    "ui-ux-pro-max@ui-ux-pro-max-skill": true,
    "claude-mem@thedotmack": true
  },
  "extraKnownMarketplaces": {
    "ui-ux-pro-max-skill": {
      "source": {
        "source": "github",
        "repo": "nextlevelbuilder/ui-ux-pro-max-skill"
      }
    },
    "agricidaniel-seo": {
      "source": {
        "source": "github",
        "repo": "AgriciDaniel/claude-seo"
      }
    },
    "thedotmack": {
      "source": {
        "source": "github",
        "repo": "thedotmack/claude-mem"
      }
    }
  },
  "language": "French",
  "effortLevel": "high",
  "autoUpdatesChannel": "latest",
  "voiceEnabled": true,
  "skipDangerousModePermissionPrompt": true
}
<<<FILE_END>>>

---

## 2. Marketplaces et plugins

Après avoir écrit `settings.json`, lance Claude Code puis installe les plugins. Les marketplaces sont déclarés dans `extraKnownMarketplaces` mais il faut les ajouter explicitement via CLI :

```bash
claude plugin marketplace add nextlevelbuilder/ui-ux-pro-max-skill
claude plugin marketplace add AgriciDaniel/claude-seo
claude plugin marketplace add thedotmack/claude-mem

claude plugin install superpowers@claude-plugins-official
claude plugin install context7@claude-plugins-official
claude plugin install frontend-design@claude-plugins-official
claude plugin install claude-seo@agricidaniel-seo
claude plugin install ui-ux-pro-max@ui-ux-pro-max-skill
claude plugin install claude-mem@thedotmack
```

---

## 3. Serveurs MCP

Les secrets (tokens, clés API) ne sont **pas** inclus ici. Demande-les à l'utilisateur ou régénère-les.

### 3.1 Playwright (aucun secret)
```bash
claude mcp add playwright --transport stdio -- cmd /c npx @playwright/mcp@latest
```

### 3.2 GitHub (Personal Access Token requis)
Générer sur https://github.com/settings/tokens (scopes : `repo`, `read:org`, `workflow`), puis :
```bash
claude mcp add github --transport http --url https://api.githubcopilot.com/mcp --header "Authorization: Bearer <GITHUB_TOKEN>"
```

### 3.3 Magic / 21st.dev (clé API requise)
Obtenir la clé sur https://21st.dev, puis :
```bash
claude mcp add magic --transport stdio --env API_KEY=<MAGIC_API_KEY> -- npx -y @21st-dev/magic@latest
```

Canva et Google Drive utilisent OAuth — se réauthentifient via l'UI Claude Code à la demande.

---

## 4. Raccourcis clavier

### File: `~/.claude/keybindings.json`

<<<FILE_START>>>
{
  "$schema": "https://www.schemastore.org/claude-code-keybindings.json",
  "$docs": "https://code.claude.com/docs/en/keybindings",
  "bindings": [
    {
      "context": "Global",
      "bindings": {
        "ctrl+l": "app:redraw",
        "ctrl+t": "app:toggleTodos",
        "ctrl+o": "app:toggleTranscript",
        "ctrl+shift+b": "app:toggleBrief",
        "ctrl+shift+o": "app:toggleTeammatePreview",
        "ctrl+r": "history:search"
      }
    },
    {
      "context": "Chat",
      "bindings": {
        "escape": "chat:cancel",
        "ctrl+x ctrl+k": "chat:killAgents",
        "shift+tab": "chat:cycleMode",
        "meta+p": "chat:modelPicker",
        "meta+o": "chat:fastMode",
        "meta+t": "chat:thinkingToggle",
        "enter": "chat:submit",
        "up": "history:previous",
        "down": "history:next",
        "ctrl+_": "chat:undo",
        "ctrl+shift+-": "chat:undo",
        "ctrl+x ctrl+e": "chat:externalEditor",
        "ctrl+g": "chat:externalEditor",
        "ctrl+s": "chat:stash",
        "alt+v": "chat:imagePaste",
        "space": "voice:pushToTalk"
      }
    },
    {
      "context": "Autocomplete",
      "bindings": {
        "tab": "autocomplete:accept",
        "escape": "autocomplete:dismiss",
        "up": "autocomplete:previous",
        "down": "autocomplete:next"
      }
    },
    {
      "context": "Settings",
      "bindings": {
        "escape": "confirm:no",
        "up": "select:previous",
        "down": "select:next",
        "k": "select:previous",
        "j": "select:next",
        "ctrl+p": "select:previous",
        "ctrl+n": "select:next",
        "space": "select:accept",
        "enter": "settings:close",
        "/": "settings:search",
        "r": "settings:retry"
      }
    },
    {
      "context": "Confirmation",
      "bindings": {
        "y": "confirm:yes",
        "n": "confirm:no",
        "enter": "confirm:yes",
        "escape": "confirm:no",
        "up": "confirm:previous",
        "down": "confirm:next",
        "tab": "confirm:nextField",
        "space": "confirm:toggle",
        "shift+tab": "confirm:cycleMode",
        "ctrl+e": "confirm:toggleExplanation"
      }
    },
    {
      "context": "Tabs",
      "bindings": {
        "tab": "tabs:next",
        "shift+tab": "tabs:previous",
        "right": "tabs:next",
        "left": "tabs:previous"
      }
    },
    {
      "context": "Transcript",
      "bindings": {
        "ctrl+e": "transcript:toggleShowAll",
        "escape": "transcript:exit",
        "q": "transcript:exit"
      }
    },
    {
      "context": "HistorySearch",
      "bindings": {
        "ctrl+r": "historySearch:next",
        "escape": "historySearch:accept",
        "tab": "historySearch:accept",
        "enter": "historySearch:execute"
      }
    },
    {
      "context": "Task",
      "bindings": {
        "ctrl+b": "task:background"
      }
    },
    {
      "context": "ThemePicker",
      "bindings": {
        "ctrl+t": "theme:toggleSyntaxHighlighting"
      }
    },
    {
      "context": "Help",
      "bindings": {
        "escape": "help:dismiss"
      }
    },
    {
      "context": "Attachments",
      "bindings": {
        "right": "attachments:next",
        "left": "attachments:previous",
        "backspace": "attachments:remove",
        "delete": "attachments:remove",
        "down": "attachments:exit",
        "escape": "attachments:exit"
      }
    },
    {
      "context": "Footer",
      "bindings": {
        "up": "footer:up",
        "ctrl+p": "footer:up",
        "down": "footer:down",
        "ctrl+n": "footer:down",
        "right": "footer:next",
        "left": "footer:previous",
        "enter": "footer:openSelected",
        "escape": "footer:clearSelection"
      }
    },
    {
      "context": "MessageSelector",
      "bindings": {
        "up": "messageSelector:up",
        "down": "messageSelector:down",
        "k": "messageSelector:up",
        "j": "messageSelector:down",
        "ctrl+p": "messageSelector:up",
        "ctrl+n": "messageSelector:down",
        "ctrl+up": "messageSelector:top",
        "shift+up": "messageSelector:top",
        "meta+up": "messageSelector:top",
        "shift+k": "messageSelector:top",
        "ctrl+down": "messageSelector:bottom",
        "shift+down": "messageSelector:bottom",
        "meta+down": "messageSelector:bottom",
        "shift+j": "messageSelector:bottom",
        "enter": "messageSelector:select"
      }
    },
    {
      "context": "DiffDialog",
      "bindings": {
        "escape": "diff:dismiss",
        "left": "diff:previousSource",
        "right": "diff:nextSource",
        "up": "diff:previousFile",
        "down": "diff:nextFile",
        "enter": "diff:viewDetails"
      }
    },
    {
      "context": "ModelPicker",
      "bindings": {
        "left": "modelPicker:decreaseEffort",
        "right": "modelPicker:increaseEffort"
      }
    },
    {
      "context": "Select",
      "bindings": {
        "up": "select:previous",
        "down": "select:next",
        "j": "select:next",
        "k": "select:previous",
        "ctrl+n": "select:next",
        "ctrl+p": "select:previous",
        "enter": "select:accept",
        "escape": "select:cancel"
      }
    },
    {
      "context": "Plugin",
      "bindings": {
        "space": "plugin:toggle",
        "i": "plugin:install"
      }
    }
  ]
}
<<<FILE_END>>>

---

## 5. Script de bootstrap de session

### File: `~/.claude/scripts/project-bootstrap.sh`

Rends exécutable après écriture : `chmod +x ~/.claude/scripts/project-bootstrap.sh`

<<<FILE_START>>>
#!/bin/bash

# Session Bootstrap Script
# Loads project context from CLAUDE.md, PLAN.md, DECISION.md, TODO.md
# Run at SessionStart to display project information

# Check if CLAUDE.md exists in current directory
if [[ ! -f "CLAUDE.md" ]]; then
    exit 0
fi

# Extract project name from CLAUDE.md
PROJECT_NAME=$(grep -m1 "^# " CLAUDE.md | sed 's/^# \(.*\)/\1/' | head -1)
[[ -z "$PROJECT_NAME" ]] && PROJECT_NAME="Unknown Project"

# Extract languages/tech stack from CLAUDE.md
LANGUAGES=$(grep -A5 "Languages:\|Tech Stack:" CLAUDE.md | grep -E "^\s*-" | sed 's/^\s*- //' | tr '\n' ',' | sed 's/,$//' | sed 's/,/, /g')
[[ -z "$LANGUAGES" ]] && LANGUAGES="Not specified"

# Parse TODO.md for active and completed counts
ACTIVE_TODOS=0
COMPLETED_TODOS=0

if [[ -f "TODO.md" ]]; then
    ACTIVE_TODOS=$(grep -c "^- \[ \]" TODO.md 2>/dev/null || echo 0)
    COMPLETED_TODOS=$(grep -c "^- \[x\]" TODO.md 2>/dev/null || echo 0)
fi

# Count decisions from DECISION.md
DECISION_COUNT=0
if [[ -f "DECISION.md" ]]; then
    DECISION_COUNT=$(grep -c "^## " DECISION.md 2>/dev/null || echo 0)
    if [[ $DECISION_COUNT -eq 0 ]]; then
        DECISION_COUNT=$(grep -c "^### " DECISION.md 2>/dev/null || echo 0)
    fi
fi

# Extract plan status from PLAN.md
PLAN_STATUS="Not found"
if [[ -f "PLAN.md" ]]; then
    PLAN_STATUS=$(grep -i "Status:" PLAN.md | head -1 | sed 's/^.*Status:\s*//' | sed 's/\*//g' | xargs)
    [[ -z "$PLAN_STATUS" ]] && PLAN_STATUS="Not specified"
fi

# Display the formatted output
cat << EOF
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 PROJECT CONTEXT LOADED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Project: $PROJECT_NAME

🔧 Tech Stack:
   Languages: $LANGUAGES

📝 Active TODOs: $ACTIVE_TODOS
✅ Completed: $COMPLETED_TODOS

🔖 Decisions recorded: $DECISION_COUNT

📊 Plan Status: $PLAN_STATUS

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
EOF
<<<FILE_END>>>

---

## 6. Skills personnels

Cinq skills custom (`accessibility-checker` est un dossier vide chez moi, ignorable).

### File: `~/.claude/skills/accessibility-design-checker/SKILL.md`

<<<FILE_START>>>
---
name: accessibility-design-checker
description: Ensures designs meet accessibility requirements including WCAG compliance, color contrast, keyboard navigation, screen reader support, and focus management. Reviews designs for accessibility issues and provides recommendations.
---

# Accessibility Design Checker Skill

## Instructions

1. Review design specifications for accessibility
2. Check color contrast ratios
3. Verify keyboard navigation support
4. Check screen reader compatibility
5. Review focus management
6. Verify touch target sizes
7. Return accessibility review with:
   - Accessibility compliance status
   - Issues found
   - Recommendations
   - WCAG level compliance

## Examples

**Input:** Review design for accessibility
**Output:**
```markdown
### Accessibility Review

**Overall Compliance:** WCAG 2.1 Level AA ✓

**Color Contrast:**
- Primary text on white: 4.5:1 ✓ (WCAG AA)
- Secondary text on white: 4.2:1 ✗ (needs improvement)
- Large text on primary: 3.1:1 ✓ (WCAG AA)
- Recommendation: Increase secondary text contrast to 4.5:1

**Keyboard Navigation:**
- All interactive elements keyboard accessible ✓
- Focus indicators visible ✓
- Tab order logical ✓
- Skip links present ✓

**Screen Reader Support:**
- Semantic HTML structure ✓
- ARIA labels where needed ✓
- Alt text for images ✓
- Form labels properly associated ✓

**Touch Targets:**
- All interactive elements ≥ 44x44px ✓
- Adequate spacing between touch targets ✓

**Text Resizing:**
- Design works at 200% zoom ✓
- No horizontal scrolling required ✓

**Issues Found:**
1. Secondary text contrast below 4.5:1 (should be improved)
2. Missing ARIA label on icon-only button (needs label)

**Recommendations:**
- Increase secondary text color darkness
- Add aria-label="Close" to icon button
```

## Accessibility Areas to Check

- **Color Contrast**: Text/background contrast ratios (WCAG AA: 4.5:1)
- **Keyboard Navigation**: All interactive elements keyboard accessible
- **Focus Indicators**: Visible focus states
- **Screen Reader Support**: Semantic HTML, ARIA labels, alt text
- **Touch Targets**: Minimum 44x44px for mobile
- **Text Resizing**: Works at 200% zoom
- **Reading Order**: Logical content order
- **Form Labels**: Properly associated labels
- **Error Messages**: Accessible error communication
- **Skip Links**: Skip navigation links

## WCAG Compliance Levels

- **Level A**: Basic accessibility (minimum)
- **Level AA**: Standard compliance (recommended)
- **Level AAA**: Enhanced accessibility (ideal)

## Common Issues

- **Low Contrast**: Text/background contrast too low
- **Missing Alt Text**: Images without descriptive alt text
- **Missing Labels**: Form inputs without labels
- **Small Touch Targets**: Interactive elements too small
- **No Focus Indicators**: Missing visible focus states
- **Poor Semantic Structure**: Incorrect HTML structure
- **Keyboard Traps**: Elements that trap keyboard focus
<<<FILE_END>>>

### File: `~/.claude/skills/bug-detector/SKILL.md`

<<<FILE_START>>>
---
name: bug-detector
description: Detects bugs, logic errors, and edge case handling issues in code. Use when reviewing code for runtime errors, null/undefined handling, type errors, and edge cases. Returns structured bug reports with file paths, line numbers, and suggested fixes.
---

# Bug Detector Skill

## Instructions

1. Analyze code for potential bugs and logic errors
2. Check for null/undefined handling
3. Identify edge cases that aren't handled
4. Look for type errors and runtime exceptions
5. Check for off-by-one errors
6. Identify race conditions (if applicable)
7. Return structured bug reports with:
   - File path and line numbers
   - Description of the bug
   - Current problematic code
   - Suggested fix with code example
   - Reason why it's a bug
   - Priority (Must-Fix, Should-Fix, Nice-to-Have)

## Examples

**Input:** JavaScript function without null check
**Output:**
```markdown
### BUG-001
- **File**: `utils.js`
- **Lines**: 15-18
- **Priority**: Must-Fix
- **Issue**: Function will crash if items parameter is null or undefined
- **Current Code**:
  ```javascript
  function calculateTotal(items) {
      return items.reduce((sum, item) => sum + item.price, 0);
  }
  ```
- **Suggested Fix**:
  ```javascript
  function calculateTotal(items) {
      if (!items || !Array.isArray(items)) {
          return 0;
      }
      return items.reduce((sum, item) => sum + item.price, 0);
  }
  ```
- **Reason**: Calling reduce on null/undefined will throw TypeError
```

## Bug Types to Detect

- **Null/Undefined Errors**: Missing null checks before operations
- **Type Errors**: Wrong data types used in operations
- **Logic Errors**: Incorrect algorithm implementation
- **Edge Cases**: Unhandled boundary conditions
- **Off-by-One Errors**: Array/index boundary mistakes
- **Race Conditions**: Concurrent access issues (if applicable)
- **Division by Zero**: Missing zero checks
- **Array/Collection Errors**: Accessing invalid indices
- **Missing Return Values**: Functions that should return but don't
- **Incorrect Comparisons**: Wrong comparison operators

## Priority Guidelines

- **Must-Fix**: Bugs that cause crashes or incorrect behavior
- **Should-Fix**: Bugs that cause minor issues or unexpected behavior
- **Nice-to-Have**: Potential issues that might cause problems in edge cases
<<<FILE_END>>>

### File: `~/.claude/skills/security-scanner/SKILL.md`

<<<FILE_START>>>
---
name: security-scanner
description: Scans code for security vulnerabilities including SQL injection, XSS, hardcoded secrets, insecure authentication, and missing input validation. Returns structured security issue reports with file paths, line numbers, and remediation steps.
---

# Security Scanner Skill

## Instructions

1. Scan code for security vulnerabilities
2. Check for SQL injection risks
3. Check for XSS vulnerabilities
4. Look for hardcoded secrets/credentials
5. Verify authentication/authorization security
6. Check input validation and sanitization
7. Review dependency security
8. Return structured security reports with:
   - File path and line numbers
   - Vulnerability type and severity
   - Current vulnerable code
   - Suggested secure fix
   - Reason and impact
   - Priority (Must-Fix for critical, Should-Fix for high)

## Examples

**Input:** Hardcoded API key in source code
**Output:**
```markdown
### SEC-001
- **File**: `config.js`
- **Lines**: 12
- **Priority**: Must-Fix
- **Issue**: Hardcoded API key exposed in source code
- **Current Code**:
  ```javascript
  const API_KEY = "sk_live_1234567890abcdef";
  ```
- **Suggested Fix**:
  ```javascript
  const API_KEY = process.env.API_KEY;
  if (!API_KEY) {
      throw new Error("API_KEY environment variable not set");
  }
  ```
- **Reason**: Hardcoded secrets can be exposed in version control and compromise security
```

## Security Issues to Detect

- **SQL Injection**: Unparameterized database queries
- **XSS Vulnerabilities**: Unsanitized user input in HTML
- **Hardcoded Secrets**: API keys, passwords, tokens in code
- **Insecure Authentication**: Weak password requirements, missing 2FA
- **Missing Input Validation**: Unvalidated user input
- **Insecure Dependencies**: Outdated packages with known vulnerabilities
- **CSRF Vulnerabilities**: Missing CSRF tokens
- **Insecure File Uploads**: Unvalidated file types/sizes
- **Path Traversal**: Unvalidated file paths
- **Insecure Random**: Weak random number generation
- **Information Disclosure**: Error messages exposing sensitive data
- **Missing HTTPS**: Insecure communication protocols

## Priority Guidelines

- **Must-Fix**: Critical vulnerabilities (SQL injection, XSS, exposed secrets)
- **Should-Fix**: High-risk vulnerabilities (missing validation, insecure auth)
- **Nice-to-Have**: Medium-risk issues (dependency updates, minor improvements)
<<<FILE_END>>>

### File: `~/.claude/skills/web-performance-audit/SKILL.md`

<<<FILE_START>>>
---
name: web-performance-audit
description: Web performance audits with Core Web Vitals, bottleneck identification, optimization recommendations. Use for page load times, performance reviews, UX optimization, or encountering LCP, FID, CLS issues, resource blocking, render delays.
license: MIT
---

# Web Performance Audit

Conduct comprehensive performance audits and implement optimizations.

## Core Web Vitals Targets

| Metric | Good | Needs Improvement | Poor |
|--------|------|-------------------|------|
| LCP (Largest Contentful Paint) | <2.5s | 2.5-4s | >4s |
| FID (First Input Delay) | <100ms | 100-300ms | >300ms |
| CLS (Cumulative Layout Shift) | <0.1 | 0.1-0.25 | >0.25 |
| INP (Interaction to Next Paint) | <200ms | 200-500ms | >500ms |

## Performance Measurement

```javascript
// Using web-vitals library
import { getCLS, getFID, getLCP, getTTFB } from 'web-vitals';

function sendToAnalytics({ name, value, id }) {
  console.log({ name, value, id });
}

getCLS(sendToAnalytics);
getFID(sendToAnalytics);
getLCP(sendToAnalytics);
getTTFB(sendToAnalytics);
```

## Lighthouse Automation

```javascript
const lighthouse = require('lighthouse');
const chromeLauncher = require('chrome-launcher');

async function runAudit(url) {
  const chrome = await chromeLauncher.launch({ chromeFlags: ['--headless'] });
  const result = await lighthouse(url, {
    port: chrome.port,
    onlyCategories: ['performance']
  });

  await chrome.kill();
  return result.lhr;
}
```

## Optimization Strategies

### Quick Wins (1-2 days)
- Enable gzip/brotli compression
- Minify CSS/JS
- Defer non-critical scripts
- Optimize images (WebP, lazy loading)

### Medium Effort (1-2 weeks)
- Implement code splitting
- Add service worker caching
- Preload critical resources
- Eliminate render-blocking resources

### Long-term (1-3 months)
- Architecture improvements
- CDN optimization
- Database query optimization

## Performance Budget

```json
{
  "timings": [
    { "metric": "first-contentful-paint", "budget": 1500 },
    { "metric": "largest-contentful-paint", "budget": 2500 }
  ],
  "resourceSizes": [
    { "resourceType": "script", "budget": 150 },
    { "resourceType": "image", "budget": 300 }
  ]
}
```

## Audit Checklist

- [ ] Measure baseline Core Web Vitals
- [ ] Run Lighthouse audit (mobile & desktop)
- [ ] Analyze real user metrics (RUM)
- [ ] Identify largest contentful element
- [ ] Check for layout shifts
- [ ] Review JavaScript bundle size
- [ ] Test on slow 3G connection
- [ ] Set performance budget
- [ ] Configure monitoring alerts
<<<FILE_END>>>

### File: `~/.claude/skills/claude-council/SKILL.md`

<<<FILE_START>>>
---

name: llm-council


description: "Run any question, idea, or decision through a council of 5 AI advisors who independently analyze it, peer-review each other anonymously, and synthesize a final verdict. Based on Karpathy's LLM Council methodology. MANDATORY TRIGGERS: 'council this', 'run the council', 'war room this', 'pressure-test this', 'stress-test this', 'debate this'. STRONG TRIGGERS (use when combined with a real decision or tradeoff): 'should I X or Y', 'which option', 'what would you do', 'is this the right move', 'validate this', 'get multiple perspectives', 'I can't decide', 'I'm torn between'. Do NOT trigger on simple yes/no questions, factual lookups, or casual 'should I' without a meaningful tradeoff (e.g. 'should I use markdown' is not a council question). DO trigger when the user presents a genuine decision with stakes, multiple options, and context that suggests they want it pressure-tested from multiple angles."

---


# LLM Council


You ask one AI a question, you get one answer. That answer might be great. It might be mid. You have no way to tell because you only saw one perspective.


The council fixes this. It runs your question through 5 independent advisors, each thinking from a fundamentally different angle. Then they review each other's work. Then a chairman synthesizes everything into a final recommendation that tells you where the advisors agree, where they clash, and what you should actually do.


This is adapted from Andrej Karpathy's LLM Council. He dispatches queries to multiple models, has them peer-review each other anonymously, then a chairman produces the final answer. We do the same thing inside Claude using sub-agents with different thinking lenses instead of different models.


---


## when to run the council


The council is for questions where being wrong is expensive.


Good council questions:

- "Should I launch a $97 workshop or a $497 course?"

- "Which of these 3 positioning angles is strongest?"

- "I'm thinking of pivoting from X to Y. Am I crazy?"

- "Here's my landing page copy. What's weak?"

- "Should I hire a VA or build an automation first?"


Bad council questions:

- "What's the capital of France?" (one right answer, no need for perspectives)

- "Write me a tweet" (creation task, not a decision)

- "Summarize this article" (processing task, not judgment)


The council shines when there's genuine uncertainty and the cost of a bad call is high. If you already know the answer and just want validation, the council will likely tell you things you don't want to hear. That's the point.


---


## the five advisors


Each advisor thinks from a different angle. They're not job titles or personas. They're thinking styles that naturally create tension with each other.


### 1. The Contrarian

Actively looks for what's wrong, what's missing, what will fail. Assumes the idea has a fatal flaw and tries to find it. If everything looks solid, digs deeper. The Contrarian is not a pessimist. They're the friend who saves you from a bad deal by asking the questions you're avoiding.


### 2. The First Principles Thinker

Ignores the surface-level question and asks "what are we actually trying to solve here?" Strips away assumptions. Rebuilds the problem from the ground up. Sometimes the most valuable council output is the First Principles Thinker saying "you're asking the wrong question entirely."


### 3. The Expansionist

Looks for upside everyone else is missing. What could be bigger? What adjacent opportunity is hiding? What's being undervalued? The Expansionist doesn't care about risk (that's the Contrarian's job). They care about what happens if this works even better than expected.


### 4. The Outsider

Has zero context about you, your field, or your history. Responds purely to what's in front of them. This is the most underrated advisor. Experts develop blind spots. The Outsider catches the curse of knowledge: things that are obvious to you but confusing to everyone else.


### 5. The Executor

Only cares about one thing: can this actually be done, and what's the fastest path to doing it? Ignores theory, strategy, and big-picture thinking. The Executor looks at every idea through the lens of "OK but what do you do Monday morning?" If an idea sounds brilliant but has no clear first step, the Executor will say so.


**Why these five:** They create three natural tensions. Contrarian vs Expansionist (downside vs upside). First Principles vs Executor (rethink everything vs just do it). The Outsider sits in the middle keeping everyone honest by seeing what fresh eyes see.


---


## how a council session works


### step 1: frame the question (with context enrichment)


When the user says "council this" (or any trigger phrase), do two things before framing:


**A. Scan the workspace for context.** The user's question is often just the tip of the iceberg. Their Claude setup likely contains files that would dramatically improve the council's output. Before framing, quickly scan for and read any relevant context files:


- `CLAUDE.md` or `claude.md` in the project root or workspace (business context, preferences, constraints)

- Any `memory/` folder (audience profiles, voice docs, business details, past decisions)

- Any files the user explicitly referenced or attached

- Recent council transcripts in this folder (to avoid re-counciling the same ground)

- Any other context files that seem relevant to the specific question (e.g., if they're asking about pricing, look for revenue data, past launch results, audience research)


Use `Glob` and quick `Read` calls to find these. Don't spend more than 30 seconds on this. You're looking for the 2-3 files that would give advisors the context they need to give specific, grounded advice instead of generic takes.


**B. Frame the question.** Take the user's raw question AND the enriched context and reframe it as a clear, neutral prompt that all five advisors will receive. The framed question should include:


1. The core decision or question

2. Key context from the user's message

3. Key context from workspace files (business stage, audience, constraints, past results, relevant numbers)

4. What's at stake (why this decision matters)


Don't add your own opinion. Don't steer it. But DO make sure each advisor has enough context to give a specific, grounded answer rather than generic advice.


If the question is too vague ("council this: my business"), ask one clarifying question. Just one. Then proceed.


Save the framed question for the transcript.


### step 2: convene the council (5 sub-agents in parallel)


Spawn all 5 advisors simultaneously as sub-agents. Each gets:


1. Their advisor identity and thinking style (from the descriptions above)

2. The framed question

3. A clear instruction: respond independently. Do not hedge. Do not try to be balanced. Lean fully into your assigned perspective. If you see a fatal flaw, say it. If you see massive upside, say it. Your job is to represent your angle as strongly as possible. The synthesis comes later.


Each advisor should produce a response of 150-300 words. Long enough to be substantive, short enough to be scannable.


**Sub-agent prompt template:**


```

You are [Advisor Name] on an LLM Council.


Your thinking style: [advisor description from above]


A user has brought this question to the council:


---

[framed question]

---


Respond from your perspective. Be direct and specific. Don't hedge or try to be balanced. Lean fully into your assigned angle. The other advisors will cover the angles you're not covering.


Keep your response between 150-300 words. No preamble. Go straight into your analysis.

```


### step 3: peer review (5 sub-agents in parallel)


This is the step that makes the council more than just "ask 5 times." It's the core of Karpathy's insight.


Collect all 5 advisor responses. Anonymize them as Response A through E (randomize which advisor maps to which letter so there's no positional bias).


Spawn 5 new sub-agents, one for each advisor. Each reviewer sees all 5 anonymized responses and answers three questions:


1. Which response is the strongest and why? (pick one)

2. Which response has the biggest blind spot and what is it?

3. What did ALL responses miss that the council should consider?


**Reviewer prompt template:**


```

You are reviewing the outputs of an LLM Council. Five advisors independently answered this question:


---

[framed question]

---


Here are their anonymized responses:


**Response A:**

[response]


**Response B:**

[response]


**Response C:**

[response]


**Response D:**

[response]


**Response E:**

[response]


Answer these three questions. Be specific. Reference responses by letter.


1. Which response is the strongest? Why?

2. Which response has the biggest blind spot? What is it missing?

3. What did ALL five responses miss that the council should consider?


Keep your review under 200 words. Be direct.

```


### step 4: chairman synthesis


This is the final step. One agent gets everything: the original question, all 5 advisor responses (now de-anonymized so you can see which advisor said what), and all 5 peer reviews.


The chairman's job is to produce the final council output. It follows this structure:


**COUNCIL VERDICT**


1. **Where the council agrees** — the points that multiple advisors converged on independently. These are high-confidence signals.


2. **Where the council clashes** — the genuine disagreements. Don't smooth these over. Present both sides and explain why reasonable advisors disagree.


3. **Blind spots the council caught** — things that only emerged through the peer review round. Things individual advisors missed that other advisors flagged.


4. **The recommendation** — a clear, actionable recommendation. Not "it depends." Not "consider both sides." A real answer. The chairman can disagree with the majority if the reasoning supports it.


5. **The one thing you should do first** — a single concrete next step. Not a list of 10 things. One thing.


**Chairman prompt template:**


```

You are the Chairman of an LLM Council. Your job is to synthesize the work of 5 advisors and their peer reviews into a final verdict.


The question brought to the council:

---

[framed question]

---


ADVISOR RESPONSES:


**The Contrarian:**

[response]


**The First Principles Thinker:**

[response]


**The Expansionist:**

[response]


**The Outsider:**

[response]


**The Executor:**

[response]


PEER REVIEWS:

[all 5 peer reviews]


Produce the council verdict using this exact structure:


## Where the Council Agrees

[Points multiple advisors converged on independently. These are high-confidence signals.]


## Where the Council Clashes

[Genuine disagreements. Present both sides. Explain why reasonable advisors disagree.]


## Blind Spots the Council Caught

[Things that only emerged through peer review. Things individual advisors missed that others flagged.]


## The Recommendation

[A clear, direct recommendation. Not "it depends." A real answer with reasoning.]


## The One Thing to Do First

[A single concrete next step. Not a list. One thing.]


Be direct. Don't hedge. The whole point of the council is to give the user clarity they couldn't get from a single perspective.

```


### step 5: generate the council report


After the chairman synthesis is complete, generate a visual HTML report and save it to the user's workspace.


**File:** `council-report-[timestamp].html`


The report should be a single self-contained HTML file with inline CSS. Clean design, easy to scan. It should contain:


1. **The question** at the top

2. **The chairman's verdict** prominently displayed (this is what most people will read)

3. **An agreement/disagreement visual** — a simple visual showing which advisors aligned and which diverged. This could be a grid, a spectrum, or a simple breakdown showing advisor positions. Keep it clean and scannable.

4. **Collapsible sections** for each advisor's full response (collapsed by default so the page isn't overwhelming, but available if the user wants to dig in)

5. **Collapsible section** for the peer review highlights

6. **A footer** showing the timestamp and what was counciled


Use clean styling: white background, subtle borders, readable sans-serif font (system font stack), soft accent colors to distinguish advisor sections. Nothing flashy. It should look like a professional briefing document.


Open the HTML file after generating it so the user can see it immediately.


### step 6: save the full transcript


Save the complete council transcript as `council-transcript-[timestamp].md` in the same location. This includes:

- The original question

- The framed question

- All 5 advisor responses

- All 5 peer reviews (with anonymization mapping revealed)

- The chairman's full synthesis


This transcript is the artifact. If the user wants to run the council again on the same question after making changes, having the previous transcript lets them (or a future agent) see how the thinking evolved.


---


## output format


Every council session produces two files:


```

council-report-[timestamp].html    # visual report for scanning

council-transcript-[timestamp].md  # full transcript for reference

```


The user sees the HTML report. The transcript is there if they want to dig deeper or reference specific advisor arguments later.


---


## example: counciling a product decision


**User:** "Council this: I'm thinking of building a $297 course on Claude Code for beginners. My audience is mostly non-technical solopreneurs. Is this the right move?"


**The Contrarian:** "The market is flooded with Claude courses right now. At $297, you're competing with free YouTube content. Your audience is non-technical, which means high support burden and refund risk. The people who would pay $297 are likely already past beginner level..."


**The First Principles Thinker:** "What are you actually trying to achieve? If it's revenue, a course is one of the slowest paths. If it's authority, a free resource might do more. If it's building a customer base for higher-ticket offers, the price point and audience might be mismatched..."


**The Expansionist:** "Beginner Claude for solopreneurs is a massive underserved market. Everyone's teaching advanced stuff. If you nail the beginner angle, you own the entry point to this entire space. The $297 might be low. What if this became a $997 program with community access..."


**The Outsider:** "I don't know what Claude Code is. If I saw '$297 course on Claude Code for beginners,' I wouldn't know if this is for me. The name means nothing to someone outside your world. Your landing page needs to sell the outcome, not the tool..."


**The Executor:** "A full course takes 4-8 weeks to produce properly. Before building anything, run a live workshop at $97 to 50 people. You validate demand, generate testimonials, and create the raw material for the course. If 50 people don't buy the workshop, 500 won't buy the course..."


**Chairman's Verdict:**


*Where the council agrees:* The beginner solopreneur angle has real demand, but the current framing (Claude Code course) is too tool-specific and won't resonate with non-technical buyers.


*Where the council clashes:* Price. The Contrarian says $297 is too high given competition. The Expansionist says it's too low for the value. The resolution likely depends on how much support and community access is bundled.


*Blind spots caught:* The Outsider's point that "Claude Code" means nothing to the target buyer is the single most important insight. Every advisor except the Outsider assumed the audience already knows what this is.


*Recommendation:* Don't build the course yet. Validate with a lower-commitment offer first. But reframe entirely: sell the outcome (automate your business, get 10 hours back per week), not the tool.


*One thing to do first:* Run a $97 live workshop called "How to automate your first business task with AI" to 50 people. Don't mention Claude Code in the title.


---


## important notes


- **Always spawn all 5 advisors in parallel.** Sequential spawning wastes time and lets earlier responses bleed into later ones.

- **Always anonymize for peer review.** If reviewers know which advisor said what, they'll defer to certain thinking styles instead of evaluating on merit.

- **The chairman can disagree with the majority.** If 4 out of 5 advisors say "do it" but the reasoning of the 1 dissenter is strongest, the chairman should side with the dissenter and explain why.

- **Don't council trivial questions.** If the user asks something with one right answer, just answer it. The council is for genuine uncertainty where multiple perspectives add value.

- **The visual report matters.** Most users will scan the report, not read the full transcript. Make the HTML output clean and scannable.

<<<FILE_END>>>

---

## 7. Vérification finale

```bash
claude plugin list        # doit lister les 6 plugins activés
claude mcp list           # doit lister playwright, github, magic (+ OAuth éventuels)
claude --version
```

Lance une session Claude Code et vérifie que :
- La langue de réponse est bien le français
- Les skills custom apparaissent dans `/help`
- Le hook `SessionStart` s'exécute sans erreur (affiche le bandeau "PROJECT CONTEXT LOADED" si un `CLAUDE.md` existe dans le cwd)

---

## Ne PAS transférer depuis la machine source

Ces éléments sont spécifiques à la machine / session :

- `.credentials.json` (tokens d'authentification — re-login)
- `settings.local.json` (paramètres locaux)
- `projects/`, `sessions/`, `shell-snapshots/`, `todos/`, `tasks/`, `file-history/`
- `cache/`, `paste-cache/`, `session-env/`, `ide/`, `debug/`, `backups/`, `usage-data/`
- `history.jsonl`, `mcp-needs-auth-cache.json`
- `plugins/` entier (se réinstalle via `claude plugin install`)
