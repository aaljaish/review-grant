# /Review Grant

An instruction-based skill for Codex and Claude Code that assesses the reviewability of randomized controlled trial (RCT) grant applications before submission.

The skill asks whether a thoughtful reviewer without deep expertise in the clinical or health-system area can understand the question, rationale, design, assumptions, and intended decision after one careful read. It identifies weaknesses, explains why they matter, and proposes concrete revisions. It does not rewrite the full application or predict a funding outcome.

## The four questions

| Domain | Reviewer question |
| --- | --- |
| Importance | Is an RCT needed to answer an unresolved question? |
| Coherence | Do the trial's intent, design, estimand, outcomes, sample size, and analysis align? |
| Feasibility | Can the trial be delivered as planned, with credible operational evidence? |
| Usefulness | Will beneficial, null, or inconclusive findings inform a meaningful decision? |

Reviewability includes the ability to assess the trial's underlying logic and assumptions. Clear wording cannot compensate for a design inconsistency or an unsupported delivery target.

## Quick start

1. Install the complete `skills/review-grant` folder using the instructions below.
2. Supply the near-final application and any relevant appendices, sponsor criteria, protocol, statistical analysis plan, timeline, or budget justification.
3. Invoke the skill and identify the files and requested scope.
4. Review the findings with the study team and relevant specialist and patient or community reviewers.

Codex example:

```text
$review-grant Review the attached near-final RCT application and protocol for
pre-submission reviewability. Use the supplied sponsor criteria. Identify the
five highest-priority revisions and flag any sections you cannot assess.
```

Claude Code example:

```text
/review-grant Review application.pdf and protocol.pdf for pre-submission
reviewability. Focus on what a thoughtful non-specialist reviewer could not
judge confidently after one careful read.
```

The display title is **/Review Grant**. The portable technical identifier is **`review-grant`**. Claude Code uses **`/review-grant`**; Codex uses **`$review-grant`**. Typing `/Review Grant` with a space is not the installation's command syntax. See the official [Codex skill guide](https://learn.chatgpt.com/docs/build-skills) and [Claude Code skill guide](https://code.claude.com/docs/en/skills).

## Installation

Download this repository or clone it, then copy the entire `skills/review-grant` folder, including `references` and `agents`, to one supported location. No Python, Node.js, API key, executable script, or external service is required by this skill. Your chosen AI application must be able to read the files you provide.

| Host and scope | Destination |
| --- | --- |
| Codex, personal | `~/.agents/skills/review-grant/` |
| Codex, project | `<project>/.agents/skills/review-grant/` |
| Claude Code, personal | `~/.claude/skills/review-grant/` |
| Claude Code, project | `<project>/.claude/skills/review-grant/` |

`~` means your home directory. These destinations follow the official [Codex discovery locations](https://learn.chatgpt.com/docs/build-skills) and [Claude Code discovery locations](https://code.claude.com/docs/en/skills). Some older Codex installations use `~/.codex/skills`; use the location supported by your installed version. Do not maintain multiple copies with the same name unless you intend to manage their precedence.

The installed structure must look like this:

```text
review-grant/
  SKILL.md
  LICENSE
  agents/
    openai.yaml
  references/
    review-framework.md
    report-template.md
```

### Windows PowerShell

Run from the downloaded repository root. This example installs for Codex personally and stops if the destination already exists:

```powershell
$reviewGrantDestination = Join-Path $HOME '.agents\skills\review-grant'
if (Test-Path -LiteralPath $reviewGrantDestination) {
    throw 'review-grant already exists. Compare the installed copy before updating.'
}
New-Item -ItemType Directory -Path (Split-Path $reviewGrantDestination) -Force | Out-Null
Copy-Item -LiteralPath '.\skills\review-grant' -Destination $reviewGrantDestination -Recurse
```

For Claude Code, change `.agents\skills\review-grant` to `.claude\skills\review-grant`. For a project installation, use that project's path instead of `$HOME`.

### macOS or Linux

Run from the downloaded repository root:

```sh
review_grant_destination="$HOME/.agents/skills/review-grant"
if [ -e "$review_grant_destination" ]; then
  printf '%s\n' 'review-grant already exists. Compare it before updating.'
else
  mkdir -p "$(dirname "$review_grant_destination")"
  cp -R skills/review-grant "$review_grant_destination"
fi
```

For Claude Code, change `.agents/skills/review-grant` to `.claude/skills/review-grant`. Restart the host if the newly installed skill does not appear. Invoke the technical name explicitly to confirm discovery.

### Other Claude surfaces

This package documents local Claude Code installation. A local `~/.claude/skills` folder does not by itself install a skill into Claude.ai, Cowork, or another cloud session. Use that surface's supported custom-skill upload or account installation process. Consult [Anthropic's skill documentation](https://code.claude.com/docs/en/skills) for current availability and account requirements. The core instructions remain readable as a standalone prompt when a host cannot load skills.

## Inputs and customization

The application is the essential input. Supporting materials improve the review but are optional. The skill reports what it could and could not assess rather than treating an omitted attachment as a proven flaw in the trial.

Useful context includes the sponsor or competition, study type, version, deadline, word limits, reviewer audience, requested emphasis, and whether to verify cited evidence externally. Provide sponsor criteria when you want the review mapped to them. The framework itself is not a sponsor scoring system.

Use an available model or setting suited to deeper analysis when appropriate. Output quality depends on the model, readable source material, context limits, and reasoning settings. There is no guaranteed model-independent result.

Examples of narrower requests:

```text
$review-grant Review only recruitment feasibility and consistency with the
sample size assumptions. Mark all other domains as outside this review.
```

```text
/review-grant This is a pilot RCT. Assess feasibility objectives, precision,
progression criteria, and how the results determine whether to proceed.
```

```text
$review-grant Compare application-v2.pdf with the prior review. Mark each
previous concern resolved, partly resolved, unresolved, or not assessable,
and identify any new contradictions in the revised material.
```

These examples are natural-language requests, not command-line flags. Non-randomized proposals require an explicit adaptation of the framework.

## What the review returns

- A scope statement and brief overall reviewability assessment.
- The three changes most helpful to reviewability, when supported.
- An assessment of importance, coherence, feasibility, and usefulness.
- A concern table with exactly three columns: **Potential reviewer concern**, **Why it matters**, and **Suggested revision**.
- Up to five highest-priority changes before submission, without inventing concerns to fill a quota.
- A short, clearly labeled hypothetical committee-discussion paragraph.
- Material unanswered questions and a reminder to adjudicate the findings with human reviewers.

Each concern identifies severity, its evidence location when available, and a concrete revision. **Major** concerns may materially affect fundability or prevent judgment of an essential claim. **Minor** concerns improve clarity or presentation without an identified substantive consequence. These categories are qualitative and unvalidated.

The skill distinguishes documented facts, unclear material, information not found in the supplied scope, and matters not assessed. It never treats missing material as permission to invent references, feasibility rates, trial details, or methodological choices.

See the [report template](skills/review-grant/references/report-template.md) and [fictional worked example](docs/worked-example.md).

## Evidence checking and confidentiality

The default review assesses the supplied materials. It does not claim that references, guidelines, or trial registries have been independently checked. If external verification is requested or required by the host, the review should cite the primary sources actually checked and state search dates and limits. Public searches must not disclose confidential application text.

Use confidential, identifiable, or sensitive material only in tools approved for that purpose by the relevant institution or sponsor. This skill does not configure encryption, retention, training use, access controls, institutional approvals, or sponsor compliance. Check those controls in the tool you choose. The package itself has no scripts, network calls, telemetry, or bundled model; host tools may still process data according to their settings.

Do not commit applications, patient data, private reviews, or confidential outputs to this public repository. The supplied example is fictional.

## Limits and human oversight

AI can miss a contradiction, misread a table, misunderstand a design, or recommend an inappropriate revision. Numerical plausibility checks do not establish statistical validity. A persuasive report does not establish the correctness of its recommendations.

The framework supports pre-submission discussion. It does not replace study-team judgment, methodological and statistical review, operational planning, patient or community input where relevant, or formal scientific, ethics, regulatory, or sponsor review. It offers no guarantee of funding and is not a validated fundability instrument.

## Troubleshooting

| Problem | Action |
| --- | --- |
| Skill is missing | Confirm the full folder is in a supported discovery location and `SKILL.md` has the exact capitalization shown. Restart the host if necessary. |
| Wrong skill runs | Check for another installed `review-grant` and inspect the host's skill list. |
| References cannot be opened | Copy the complete folder, not just `SKILL.md`. |
| PDF or scan is unreadable | Provide accessible text or an OCR-processed copy in an approved environment. Require the review to disclose unreadable sections. |
| Report sounds generic | Provide the full application and supporting evidence; request concern-by-concern source locations. |
| Review claims unperformed searches | Ask for the exact sources checked and a corrected evidence-status statement. |
| Proposal is not randomized | Ask for an adaptation and check which RCT-specific questions no longer apply. |

## Package documentation

| File | Purpose |
| --- | --- |
| [SKILL.md](skills/review-grant/SKILL.md) | Portable agent entry point and essential constraints |
| [Review framework](skills/review-grant/references/review-framework.md) | Detailed domain questions and common weaknesses |
| [Report template](skills/review-grant/references/report-template.md) | Review deliverables and concern-table structure |
| [Worked example](docs/worked-example.md) | Fictional source excerpt and a sample review |
| [Validation guide](docs/validation.md) | Packaging checks and realistic acceptance scenarios |
| [Source and credits](docs/source-and-credits.md) | Manuscript provenance and adaptation notes |
| [LICENSE](LICENSE) | CC0 1.0 Universal legal text |

To update an installation, compare your local changes with the new package and replace the complete folder deliberately. To uninstall, remove only the `review-grant` folder from the location where you installed it. No other configuration is required by this package.

## License

The skill and documentation are dedicated to the public domain under **CC0 1.0 Universal**, to the extent permitted by law, with a fallback license. You may copy, modify, redistribute, and use them commercially without an attribution requirement. Attribution to the source authors is welcome but optional. See [LICENSE](LICENSE) and the [official CC0 explanation](https://creativecommons.org/publicdomain/zero/1.0/).

CC0 applies to the repository materials, not to users' grant applications, confidential reviews, third-party publications, trademarks, or other rights the contributors do not hold. The work is provided as-is, without warranty.
