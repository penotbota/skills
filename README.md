# Peanut Butter Skills

Community skills for [Peanut Butter](https://penotbota.com), the companion that runs on your phone.

A skill teaches Peanut Butter one more thing to do when you ask — a checklist, a plan, a question. Each skill is a single `.pbskill` file you can read in full before you install it.

**Browse and install:** [skills.penotbota.com](https://skills.penotbota.com), or in the app under **You → Skills → Browse community skills**.

## What a skill can and can't do

Peanut Butter hears your household, so a skill written by someone else has to be safe to install without trusting its author. That comes from what a skill structurally cannot do, not from review alone.

A community skill **can**:

- read the sources it declares — things you've said, your task list, today's calendar, today's weather, your Things to try list, the people Peanut Butter has heard about and what's outstanding with them — and nothing else
- or, instead of reading anything of yours, ask Peanut Butter to run one of its errands: flight prices, places nearby, or a Wikipedia lookup. Peanut Butter's own code builds the search from the words you said and sends it; the skill never sees an address or a key, and the install screen says exactly what goes out
- show a checklist, a plan or a question
- propose one of three actions — add to your list, set a reminder, add to Things to try — which only happen after you say yes
- link to the sites it names, which open only when you tap

A community skill **cannot**:

- reach the internet itself, or any other app — the only things that leave the phone are an errand's search, sent by Peanut Butter, and a link you tap
- read anything of yours and run an errand in the same skill, so nothing private can end up in a search
- run code of its own
- see anything said in confidence, or a present someone is planning
- act without your yes

Before you install a skill, Peanut Butter shows who wrote it, what it reads and what it can do, and runs its tests on your own phone in a sandbox that holds none of your data.

## How the index works

- Skills live in [`skills/`](skills), one `.pbskill` file each.
- Every pull request is checked by [`tools/pbskill.jar`](tools) — the same validator the app uses.
- Merging to `main` rebuilds [skills.penotbota.com](https://skills.penotbota.com): the browse page, `index.json`, and each file with its SHA-256 checksum. The app refuses a download whose checksum doesn't match the index, so a file can't change between review and install.
- Opening the browse screen in the app fetches `index.json` and nothing else. No account, no identifiers, no tracking.

## Writing a skill

The easiest way is [skills.penotbota.com/new](https://skills.penotbota.com/new/): fill in a form, and the page checks the skill as you go with the same rules as the pull request check. From there you can download the file to try on your iPhone, or open the pull request. To write the file by hand, see [CONTRIBUTING.md](CONTRIBUTING.md). The full format is described in *PB Skill Format v2*.

## Licence

The skills in [`skills/`](skills) and the documentation are shared under the [MIT License](LICENSE).

[`tools/pbskill.jar`](tools) is not: it is Neo-2's, and may be run only to check and publish skills. See [tools/LICENSE](tools/LICENSE).
