# Peanut Butter Skills

Community skills for [Peanut Butter](https://penotbota.com), the companion that runs on your phone.

A skill teaches Peanut Butter one more thing to do when you ask — a checklist, a plan, a question. Each skill is a single `.pbskill` file you can read in full before you install it.

**Browse and install:** [skills.penotbota.com](https://skills.penotbota.com), or in the app under **You → Skills → Browse community skills**.

## What a skill can and can't do

Peanut Butter hears your household, so a skill written by someone else has to be safe to install without trusting its author. That comes from what a skill structurally cannot do, not from review alone.

A community skill **can**:

- read the sources it declares — things you've said, your task list, today's calendar, today's weather, your Things to try list — and nothing else
- show a checklist, a plan or a question
- propose one of three actions — add to your list, set a reminder, add to Things to try — which only happen after you say yes

A community skill **cannot**:

- reach the internet or any other app
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

See [CONTRIBUTING.md](CONTRIBUTING.md). The full format is described in *PB Skill Format v2*.
