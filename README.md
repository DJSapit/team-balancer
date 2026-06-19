![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)
![Dependencies: None](https://img.shields.io/badge/dependencies-none-brightgreen.svg)

# Team Balancer

A browser-based tool for splitting a group of people into balanced teams based on skill, prior groupings, and custom constraints.

**[🚀 Live demo](https://djsapit.github.io/team-balancer/)** · [How it works](#how-it-works) · [Getting started](#getting-started) · [Settings reference](#settings-reference)

---

## 🚀 How to Run This Locally (No Installation Required)

If you don't want to use the live demo web link and prefer to keep a copy of this tool on your own computer, you can download it directly:

1. Click on the **`index.html`** file in the list above.
2. Look at the top right corner of the file viewer and click the **"Download raw file"** button (it looks like a small arrow pointing down into a bracket, or a plain "Download" button depending on your view).
3. Once downloaded, double-click the `index.html` file on your computer. It will open safely right in your web browser (Chrome, Safari, Edge, or Firefox) and is ready to use instantly!

## Overview

Dividing a group into fair teams becomes harder than it first appears once you factor in skill differences, repeat pairings from past sessions, and any specific requirements about who should or shouldn't be grouped together. Team Balancer addresses this by searching through possible team arrangements and selecting one that minimizes skill imbalance and constraint violations, based on user-defined priorities.

The application runs entirely in the browser as a single HTML file, with no installation, account, or server component required. Roster data is not transmitted anywhere; it exists only in the browser session unless explicitly exported.

## Highlights

- **Optimization-based assignment** — teams are generated through a search process rather than random shuffling, using a technique adapted from combinatorial optimization.
- **Adjustable priorities** — separate weighting controls determine how strongly the tool favors skill balance, mixing of skill tiers, and avoidance of repeat teammates.
- **Custom constraints** — specific players can be required to stay on the same team or be kept apart.
- **Background processing** — calculations run off the main thread, so the interface remains responsive regardless of roster size or search length.
- **CSV import and export** — rosters can be loaded from a spreadsheet file, and results exported as CSV or copied as text, Markdown, or tab-separated values.
- **No external dependencies** — the tool is a single self-contained file with no build step.

## How It Works

The assignment process begins with an initial arrangement rather than a random one: players are ranked by skill and distributed across teams in an alternating order, producing a reasonably balanced starting point.

From there, the tool refines the arrangement iteratively. On each step, it considers swapping two players between teams and evaluates whether the change improves overall balance — accounting for skill totals, repeat pairings, and any defined constraints. Improving swaps are generally kept, but the process occasionally accepts a swap that appears slightly worse, which helps avoid settling on a suboptimal arrangement early in the search. Over the course of the run, this willingness to accept worse moves decreases, and the search becomes increasingly selective. This approach, known as simulated annealing, is a standard method for problems with a large number of possible configurations.

All of this computation takes place on a separate background thread, so the page does not become unresponsive during longer searches.

## Getting Started

The application requires no installation or setup.

1. Save the `team-balancer.html` file.
2. Open it in a browser, either by double-clicking the file or using File → Open.

The file can also be hosted as a static page (for example, via GitHub Pages) if a shareable link is preferred over a local file.

## Usage

1. **Add players** — enter them manually, import a CSV file, or load the included sample roster.
2. **Select a grouping mode** — specify either a fixed team size or a fixed number of teams.
3. **Set priority weights** — adjust the sliders controlling skill balance, skill-tier mixing, and avoidance of repeat teammates.
4. **Define constraints** (optional) — specify any players who must be kept together or apart.
5. **Generate teams** — the tool searches for a balanced arrangement and displays a progress indicator during the process.
6. **Export results** — download as CSV or copy the output as text for use in other tools.

## Importing a Roster

Rosters can be imported from a CSV file with three columns:

```csv
player name,previous team,score
Alex Rivera,Red,7.5
Jamie Chen,Blue,4
Taylor Brooks,,9
```

Name and score are required. The previous-team column is optional and is used only by the repeat-pairing avoidance setting.

## Settings Reference

| Setting | Description |
|---|---|
| Grouping mode | Fixed team size, or fixed number of teams |
| Skill-tier range | Defines the cutoff used to classify players as high- or low-skill for mixing purposes |
| Previous-teammate weight | Controls how strongly the tool avoids repeating prior pairings |
| Skill-mixing weight | Controls how strongly the tool mixes high- and low-skill players within teams |
| Score-balance weight | Controls how strongly the tool equalizes total skill across teams |
| Search depth (iterations) | Number of optimization steps performed; higher values produce a more thorough but slower search |
| Constraints | Defined "must be together" or "must be apart" player pairs |

## Other Applications

The underlying problem applies to a range of contexts beyond sports, including:

- Assigning students to academic project or lab groups
- Forming league or pickup sports teams
- Organizing hackathon or offsite teams across departments
- Matchmaking for esports scrimmages by rank
- Any setting requiring repeated regrouping while limiting repeat pairings over time

## Possible Future Work

- Persisting roster data between sessions
- Additional cooling/search strategies
- Automated tests for the core optimization logic
- A TypeScript implementation with the optimizer extracted as a standalone module

## Privacy

There is no backend and no network calls. Roster data exists only in the browser for the duration of the session and is not transmitted or stored unless the user explicitly exports it.

## License

Released under the [MIT License](LICENSE).

Made with the assistance of Claude 4.6 Sonnet