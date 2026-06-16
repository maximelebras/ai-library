# AI library — what is it

Welcome to my space where I’m gathering the AI resources I find useful. I’m new to this and don’t claim to be an expert. When I feel lost or behind, it helps to tidy my own room first.

So welcome to this very simple library. It will likely evolve over time.

## Community

- [Takomu](https://www.linkedin.com/company/takomu/posts/?feedView=all) - The first French-speaking community (🇨🇭🇫🇷🇧🇪🇨🇦) dedicated to applied AI in recruiting!

## Newsletters
- 🇫🇷 [Intelligence Humaine — HR and AI newsletter](https://intelligencehumaine.substack.com/)
- 🇫🇷 [Le Comptoir IA](https://nicoguyon.substack.com/)
- 🇬🇧 [Claude Cowork](https://substack.com/@claudedesktop?r=dguo6&shareImageVariant=light)
- 🇬🇧 [Ethan Mollick](https://www.oneusefulthing.org/)

## Podcasts
- 🇫🇷 GDIY — [Mathias Frachon](https://www.gdiy.fr/podcast/mathias-frachon/)

## Resources

- [From ScalAI] [10 Agents Claude Cowork  qui remplacent des heures de travail](https://app.notion.com/p/10-Agents-Claude-Cowork-qui-remplacent-des-heures-de-travail-344e139a5c548083bfebda2eae291425)
- [From Move] [60+ Claude Prompts for TA Leaders](https://claudeplaybookforta.netlify.app/)
- [From Colin Dargent](https://www.linkedin.com/in/colindargent/) — [100+ Claude use cases](https://docs.google.com/spreadsheets/d/1Sg7Y49a8bEz37-Nt3SENRzmFCff0yiOmE3D4rgKrHGw/edit?gid=0#gid=0)
- [From Guillaume Alexandre](https://www.linkedin.com/in/guillaumealexandre/) — [AI recruitment diagnostic](https://www.jarvi.tech/fr/ai-recruitment-diagnostic/)
- [From Hugo Dollfus](https://www.linkedin.com/in/hugo-dollfus/) — [AI skills library](https://2ab22955.sibforms.com/serve/MUIFAMfS1smXE8QVdz8TKLKDKhdtQE9cYZ5roRxcbG5UAXo5DBUNxH5spn0phqWuDLzEEUjnKS8B9xFWd6oTX5r43sEObtVhqZaIhRmZ0GyIqIqzoEgdSL1rLLV5xx4-vz7Mu8F5B6Z81nFqBJLr5whcEM2O3CkLYzfaVAEVr_RcLc6zMLcPrUQ6RReKC4T2dOImFu5PQ8yeEZGgGQ==)

# AI-Native Talent — Assessment Forms

Two ready-to-run Google Forms for measuring how AI-native a Talent function is:

| Form | Who fills it | What it measures |
|------|-------------|------------------|
| **Individual Self-Assessment** | Each team member | 5 personal AI-fluency dimensions (prompting, tools, automation, judgment, learning velocity) |
| **Function Scorecard** | Lead + anyone with full visibility | 8 function-level capability dimensions (sourcing → AI infrastructure), each with a Q4 target and top blocker |

Each dimension uses behavioral anchors at 4 levels. **Scoring rule:** you score at the *lowest* level where *all* anchors are met — partial doesn't count.

Suggested sequence: run the individual form first (introspection), then the function form (collective view), then a 30-min session comparing gaps. For the function form, divergence between scorers is the most useful signal.

---

## How to create a form (≈2 minutes)

Each form is a Google Apps Script that builds the form automatically — no manual question-by-question setup.

1. Go to **[script.google.com](https://script.google.com)** → **New project**
2. Delete the default code, paste the full script below (copy from the toggle)
3. Click **▶ Run**
4. **Authorize** when prompted (Google needs permission to create Forms on your behalf)
5. Open **View → Execution log** — it prints the shareable form URL

The form lands in your Google Drive at [drive.google.com](https://drive.google.com).

> The full scripts are also in this repo as `.gs` files if you'd rather download than copy-paste.

---

## 1. Individual Self-Assessment

<details>
<summary><b>Click to expand — copy the full script</b></summary>

```javascript
/**
 * AI-Native Talent — Individual Self-Assessment Form
 *
 * HOW TO RUN:
 * 1. Go to https://script.google.com
 * 2. Click "New project"
 * 3. Paste this entire script (replace the default code)
 * 4. Click the ▶ Run button (or press Ctrl+R / Cmd+R)
 * 5. Authorize when prompted (Google needs permission to create Forms on your behalf)
 * 6. Check the Execution log — it will print the form's shareable URL
 *
 * The form is created in your Google Drive. Find it at drive.google.com.
 */

function createSelfAssessmentForm() {

  var form = FormApp.create('AI-Native Talent — Individual Self-Assessment');

  form.setDescription(
    'Score yourself honestly on each of the 5 dimensions below.\n\n' +
    'Scoring rule: a dimension scores at the LOWEST level where ALL anchors are met — ' +
    'partial doesn\'t count. If you do 3 out of 4 things at Level 3, you\'re at Level 2.\n\n' +
    'This takes ~10 minutes. Fill it in independently before calibrating with your lead.'
  );

  form.setCollectEmail(true);
  form.setShowLinkToRespondAgain(false);
  form.setConfirmationMessage(
    'Done — your responses are saved. ' +
    'Your lead will review them before your next 1:1. ' +
    'The goal is to identify where to focus, not to judge where you are.'
  );

  // ── About you ────────────────────────────────────────────────────────────────

  form.addTextItem()
    .setTitle('Your name')
    .setRequired(true);

  // ── A. Prompting & Instruction Craft ────────────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('A. Prompting & Instruction Craft')
    .setHelpText(
      'How well do you write instructions that reliably get AI to produce what you actually want — ' +
      'without excessive back-and-forth?'
    );

  form.addMultipleChoiceItem()
    .setTitle('Select the highest level where ALL anchors describe you today')
    .setRequired(true)
    .setChoiceValues([
      '1 — Vague prompts, generic output. Need many iterations to get something usable.',
      '2 — Structured prompts with context, constraints, and a clear output format. Fewer iteration cycles.',
      '3 — Uses XML tags, persona framing, chain-of-thought. Breaks complex tasks into steps. Knows when to be prescriptive vs. open-ended.',
      '4 — Writes reusable instructions others can run (skills, SKILL.md). Diagnoses why a prompt underperforms and fixes it. Actively teaches others.'
    ]);

  form.addTextItem()
    .setTitle('A — One concrete thing you will change in the next 30 days')
    .setHelpText('Optional, but recommended. "I will..." format works best.')
    .setRequired(false);

  // ── B. Tool & Skill Fluency ───────────────────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('B. Tool & Skill Fluency')
    .setHelpText(
      'Do you know which tools and skills exist, and do you actually use them — ' +
      'or do you default to doing things manually?'
    );

  form.addMultipleChoiceItem()
    .setTitle('Select the highest level where ALL anchors describe you today')
    .setRequired(true)
    .setChoiceValues([
      '1 — Uses Claude chat only. Haven\'t explored skills or MCP connectors.',
      '2 — Uses 2–3 skills regularly (e.g. interview-assessment, standup). Knows what MCPs are connected.',
      '3 — Uses all relevant Talent skills. Knows which MCPs unlock which capabilities. Can chain tools in a session. Knows what\'s missing and can articulate the gap.',
      '4 — Creates or modifies skills. Evaluates skill quality, spots failure modes. Contributes to the alan-skills repo.'
    ]);

  form.addTextItem()
    .setTitle('B — One concrete thing you will change in the next 30 days')
    .setHelpText('Optional.')
    .setRequired(false);

  // ── C. Workflow Automation ────────────────────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('C. Workflow Automation')
    .setHelpText(
      'Have you actually reduced manual toil through scheduled tasks, artifacts, and multi-step flows — ' +
      'or does every task still require you to trigger it manually?'
    );

  form.addMultipleChoiceItem()
    .setTitle('Select the highest level where ALL anchors describe you today')
    .setRequired(true)
    .setChoiceValues([
      '1 — No automated workflows. Everything triggered manually every time.',
      '2 — Has set up at least one scheduled task (e.g. morning digest). Can describe exactly what it does.',
      '3 — Automates recurring reports, summaries, and updates. Uses Cowork artifacts for live views. Runs multi-step workflows without supervision.',
      '4 — Designs agentic workflows spanning multiple tools. Identifies new automation opportunities proactively. Builds automation for the team, not just themselves.'
    ]);

  form.addTextItem()
    .setTitle('C — One concrete thing you will change in the next 30 days')
    .setHelpText('Optional.')
    .setRequired(false);

  // ── D. AI Judgment & Calibration ─────────────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('D. AI Judgment & Calibration')
    .setHelpText(
      'Do you know when AI output is trustworthy vs. when to override it? ' +
      'Have you mapped the failure modes in your domain?'
    );

  form.addMultipleChoiceItem()
    .setTitle('Select the highest level where ALL anchors describe you today')
    .setRequired(true)
    .setChoiceValues([
      '1 — Accepts AI output uncritically OR distrusts it entirely. Binary, unexamined relationship with AI.',
      '2 — Reviews output before acting. Catches obvious errors. Knows to verify factual claims.',
      '3 — Knows which tasks AI does well vs. poorly in their specific domain. Calibrates trust by task type. Writes prompts that anticipate failure modes.',
      '4 — Uses AI to stress-test their own reasoning. Can articulate systematic failure modes (hallucination, sycophancy, overconfidence) and designs workflows that account for them.'
    ]);

  form.addTextItem()
    .setTitle('D — One concrete thing you will change in the next 30 days')
    .setHelpText('Optional.')
    .setRequired(false);

  // ── E. Learning Velocity ───────────────────────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('E. Learning Velocity')
    .setHelpText(
      'How fast do you pick up new AI capabilities and bring them back to the team — ' +
      'not just for yourself, but as a multiplier?'
    );

  form.addMultipleChoiceItem()
    .setTitle('Select the highest level where ALL anchors describe you today')
    .setRequired(true)
    .setChoiceValues([
      '1 — Waits for training. Adopts tools only when required to.',
      '2 — Explores new tools when suggested by others. Shares findings occasionally in #ai_assisted_development.',
      '3 — Actively experiments with new capabilities without being prompted. Runs own mini-evals. Consistently shares signal and practice in the AI channel.',
      '4 — Spots new capabilities before they\'re announced internally. Proposes new skills or workflows proactively. Sets the pace for the rest of the team.'
    ]);

  form.addTextItem()
    .setTitle('E — One concrete thing you will change in the next 30 days')
    .setHelpText('Optional.')
    .setRequired(false);

  // ── Done ──────────────────────────────────────────────────────────────────

  var url = form.getPublishedUrl();
  var editUrl = form.getEditUrl();

  Logger.log('✅ Form created successfully!');
  Logger.log('');
  Logger.log('Share this URL with your team:');
  Logger.log(url);
  Logger.log('');
  Logger.log('Edit the form here:');
  Logger.log(editUrl);

  return url;
}
```

</details>

---

## 2. Function Scorecard

<details>
<summary><b>Click to expand — copy the full script</b></summary>

```javascript
/**
 * AI-Native Talent — Function-Level Scorecard
 *
 * HOW TO RUN:
 * 1. Go to https://script.google.com
 * 2. Click "New project"
 * 3. Paste this entire script (replace the default code)
 * 4. Click the ▶ Run button (or press Ctrl+R / Cmd+R)
 * 5. Authorize when prompted
 * 6. Check the Execution log — it will print the form's shareable URL
 *
 * Intended for: the Talent function lead + anyone on the team who has enough
 * visibility to score the function as a whole. Run independently, then compare
 * scores — divergences are the most useful signal.
 */

function createFunctionScorecardForm() {

  var form = FormApp.create('AI-Native Talent — Function Scorecard');

  form.setDescription(
    'Score the Talent function as a whole on each of the 8 dimensions below.\n\n' +
    'Scoring rule: a dimension scores at the LOWEST level where ALL anchors are met. ' +
    'If one person does it but the rest don\'t, it doesn\'t count as a function-level capability.\n\n' +
    'Fill this in independently. If multiple people score it, compare results — ' +
    'disagreements reveal blind spots.\n\n' +
    'This takes ~15 minutes.'
  );

  form.setCollectEmail(true);
  form.setShowLinkToRespondAgain(false);
  form.setConfirmationMessage(
    'Saved. Results will be reviewed by the Talent lead. ' +
    'The goal is to identify the highest-leverage gaps, not to benchmark against others.'
  );

  // ── About you ────────────────────────────────────────────────────────────────

  form.addTextItem()
    .setTitle('Your name')
    .setRequired(true);

  form.addMultipleChoiceItem()
    .setTitle('Your role')
    .setRequired(true)
    .setChoiceValues([
      'Talent Lead',
      'Recruiter / Talent Partner',
      'Sourcer',
      'Talent Coordinator',
      'Other'
    ]);

  // ── 1. Sourcing & Pipeline Generation ────────────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('1. Sourcing & Pipeline Generation')
    .setHelpText(
      'How systematically does the function generate pipeline — with or without a recruiter manually driving each search?'
    );

  form.addMultipleChoiceItem()
    .setTitle('Select the highest level where ALL anchors describe the function today')
    .setRequired(true)
    .setChoiceValues([
      '1 — Manual LinkedIn search per role. Job posts written from scratch. Outreach is opportunistic.',
      '2 — AI drafts outreach messages. Templates reused across similar roles. Some channels tracked.',
      '3 — Multi-channel campaigns at scale via LinkedIn Messenger skill or Edge API. Messages personalized per profile. Ashby pipeline updated automatically post-outreach. Response rate and pipeline conversion tracked.',
      '4 — AI identifies target profiles proactively from market signals. Outreach quality/volume optimized in a feedback loop. Pipeline per role forecasted from historical funnel data. Sourcers spend >80% of time on judgment and relationships, not mechanics.'
    ]);

  form.addMultipleChoiceItem()
    .setTitle('Target level by end of Q4')
    .setRequired(true)
    .setChoiceValues(['1', '2', '3', '4']);

  form.addTextItem()
    .setTitle('Top blocker today')
    .setHelpText('What is the single biggest thing stopping you from reaching the next level?')
    .setRequired(false);

  // ── 2. Screening & Interview Quality ─────────────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('2. Screening & Interview Quality')
    .setHelpText(
      'Are interviews structured, scored consistently, and automatically captured — ' +
      'or does quality depend on who runs the interview?'
    );

  form.addMultipleChoiceItem()
    .setTitle('Select the highest level where ALL anchors describe the function today')
    .setRequired(true)
    .setChoiceValues([
      '1 — Manual CV review. Interview questions improvised. Notes taken by hand or lost.',
      '2 — AI summarizes CVs. Interviewers use AI to prep questions. Some scorecard templates exist.',
      '3 — Structured scorecards auto-generated from JD. Transcripts analyzed via interview-assessment skill. Scorecards submitted to Ashby automatically. Pass rates tracked by interviewer and stage. Monthly calibration sessions run using AI-generated divergence report.',
      '4 — Calibration patterns analyzed across interviewers over time. Bias flags surfaced (halo effect, inconsistent level anchors). Predictive signal identified — which early signals correlate with 12-month performance.'
    ]);

  form.addMultipleChoiceItem()
    .setTitle('Target level by end of Q4')
    .setRequired(true)
    .setChoiceValues(['1', '2', '3', '4']);

  form.addTextItem()
    .setTitle('Top blocker today')
    .setRequired(false);

  // ── 3. Hiring Decision Process ────────────────────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('3. Hiring Decision Process')
    .setHelpText(
      'Are hiring decisions made consistently, documented in full, and stored in a way that builds institutional knowledge?'
    );

  form.addMultipleChoiceItem()
    .setTitle('Select the highest level where ALL anchors describe the function today')
    .setRequired(true)
    .setChoiceValues([
      '1 — Debrief by Slack or email. Decision form filled manually and incompletely.',
      '2 — AI summarizes individual feedback before the debrief. Some consistency in form-filling.',
      '3 — Decision meeting transcript → auto-filled Ashby form via decision-meeting-feedback skill. Level calibration suggested by AI. Decision rationale stored and searchable.',
      '4 — Decision consistency tracked across panels and time. AI flags divergence from stated bar. Offer acceptance predicted from pipeline data.'
    ]);

  form.addMultipleChoiceItem()
    .setTitle('Target level by end of Q4')
    .setRequired(true)
    .setChoiceValues(['1', '2', '3', '4']);

  form.addTextItem()
    .setTitle('Top blocker today')
    .setRequired(false);

  // ── 4. Candidate Experience ───────────────────────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('4. Candidate Experience')
    .setHelpText(
      'Does every candidate get a fast, personalized, respectful experience — ' +
      'regardless of recruiter workload?'
    );

  form.addMultipleChoiceItem()
    .setTitle('Select the highest level where ALL anchors describe the function today')
    .setRequired(true)
    .setChoiceValues([
      '1 — Manual email comms. Delays common. No tracking of response times.',
      '2 — AI drafts rejection and update emails. Templates used for common scenarios.',
      '3 — Automated status updates via Gmail MCP. Interview prep resources auto-sent at scheduling. Response time SLA tracked. Rejections personalized with specific feedback.',
      '4 — Candidate NPS collected and fed back to interviewers. AI personalizes every touchpoint. Time-to-decision minimized and measured.'
    ]);

  form.addMultipleChoiceItem()
    .setTitle('Target level by end of Q4')
    .setRequired(true)
    .setChoiceValues(['1', '2', '3', '4']);

  form.addTextItem()
    .setTitle('Top blocker today')
    .setRequired(false);

  // ── 5. Interview Scheduling ───────────────────────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('5. Interview Scheduling')
    .setHelpText(
      'How much coordinator time does it take to get an interview loop on the calendar — ' +
      'and how often does it slip?'
    );

  form.addMultipleChoiceItem()
    .setTitle('Select the highest level where ALL anchors describe the function today')
    .setRequired(true)
    .setChoiceValues([
      '1 — Manual email back-and-forth to find slots. Panel coordination by hand. Loops take 3–5 days. Rescheduling requires full restart.',
      '2 — Calendly links sent to candidates. Panel templates exist. Reminders sent manually. Some reduction in coordinator time, but exceptions still require manual handling.',
      '3 — AI checks all interviewer availability via GCal MCP, proposes optimal slots, books automatically, sends prep materials and reminders without human touch. Time-to-schedule tracked per role and stage. Reschedules handled automatically.',
      '4 — Panel composition optimized by availability, role fit, and interviewer load simultaneously. Full loop completed in <48h from greenlight. Scheduling data identifies capacity bottlenecks and peak/off-peak patterns.'
    ]);

  form.addMultipleChoiceItem()
    .setTitle('Target level by end of Q4')
    .setRequired(true)
    .setChoiceValues(['1', '2', '3', '4']);

  form.addTextItem()
    .setTitle('Top blocker today')
    .setRequired(false);

  // ── 6. Talent Analytics & Forecasting ────────────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('6. Talent Analytics & Forecasting')
    .setHelpText(
      'Can the function answer "where is the bottleneck?" and "will we hit our hiring plan?" ' +
      'in real time — or only after a manual spreadsheet update?'
    );

  form.addMultipleChoiceItem()
    .setTitle('Select the highest level where ALL anchors describe the function today')
    .setRequired(true)
    .setChoiceValues([
      '1 — Monthly manual headcount spreadsheet. No funnel data. Time-to-hire estimated, not measured.',
      '2 — AI generates pipeline summaries from Ashby exports. Some metrics tracked manually.',
      '3 — Live hiring funnel artifact in Cowork: pass rates, time-to-hire, interviewer load updated automatically. Capacity planning done with data, not intuition.',
      '4 — Headcount forecasting by function and level. Attrition signals detected early. Hiring ROI measured (cost per hire, quality of hire at 6 months).'
    ]);

  form.addMultipleChoiceItem()
    .setTitle('Target level by end of Q4')
    .setRequired(true)
    .setChoiceValues(['1', '2', '3', '4']);

  form.addTextItem()
    .setTitle('Top blocker today')
    .setRequired(false);

  // ── 7. Employer Brand & Content ───────────────────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('7. Employer Brand & Content')
    .setHelpText(
      'Is the function producing consistent, on-brand content that drives inbound — ' +
      'or is it ad hoc and dependent on one person\'s motivation?'
    );

  form.addMultipleChoiceItem()
    .setTitle('Select the highest level where ALL anchors describe the function today')
    .setRequired(true)
    .setChoiceValues([
      '1 — Ad hoc LinkedIn posts. Content fully manual. No cadence.',
      '2 — AI drafts posts and articles. Published manually after light edit.',
      '3 — Engineering blog posts generated from raw material via alan-engineering-blog-draft skill. LinkedIn posts in brand voice via linkedin-post-fr skill. Minimum 2 posts/month tracked for reach. Source attribution active in Ashby.',
      '4 — Content calendar managed by AI from engineering activity signals. Impact tracked back to pipeline quality. A/B testing of messages across segments.'
    ]);

  form.addMultipleChoiceItem()
    .setTitle('Target level by end of Q4')
    .setRequired(true)
    .setChoiceValues(['1', '2', '3', '4']);

  form.addTextItem()
    .setTitle('Top blocker today')
    .setRequired(false);

  // ── 8. AI Infrastructure & Skills Adoption ───────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('8. AI Infrastructure & Skills Adoption')
    .setHelpText(
      'Is AI embedded in how the function works — or is it each person\'s personal side project?'
    );

  form.addMultipleChoiceItem()
    .setTitle('Select the highest level where ALL anchors describe the function today')
    .setRequired(true)
    .setChoiceValues([
      '1 — No shared AI tools or practices. Each person experiments alone.',
      '2 — Some shared prompts and templates. Skills known but used inconsistently. MCPs connected but not systematically used.',
      '3 — Skill library actively used across all workflows. New Talent-specific skills created as needed. Maturity reviewed quarterly. New joiners onboarded to AI tooling in week 1.',
      '4 — Skills are the primary workflow layer, not an optional add-on. Talent contributes to alan-skills repo. Function can evaluate skill quality and diagnose failures.'
    ]);

  form.addMultipleChoiceItem()
    .setTitle('Target level by end of Q4')
    .setRequired(true)
    .setChoiceValues(['1', '2', '3', '4']);

  form.addTextItem()
    .setTitle('Top blocker today')
    .setRequired(false);

  // ── Overall ───────────────────────────────────────────────────────────────────

  form.addSectionHeaderItem()
    .setTitle('Overall')
    .setHelpText('');

  form.addTextItem()
    .setTitle('The single highest-leverage thing the function could do in the next 90 days to move toward AI-native')
    .setRequired(false);

  // ── Done ──────────────────────────────────────────────────────────────────────

  var url = form.getPublishedUrl();
  var editUrl = form.getEditUrl();

  Logger.log('✅ Form created successfully!');
  Logger.log('');
  Logger.log('Share this URL with your team:');
  Logger.log(url);
  Logger.log('');
  Logger.log('Edit the form here:');
  Logger.log(editUrl);

  return url;
}
```

</details>

---

## Repo contents

- `create_self_assessment_form.gs` — individual form generator
- `create_function_scorecard_form.gs` — function scorecard generator
- `README.md` — this file
