# Contributing a skill

Thanks for writing one. A skill is a single `.pbskill` file — a JSON document with the skill's manifest, its prompt, its tests and a short readme.

**The easy way:** [skills.penotbota.com/new](https://skills.penotbota.com/new/) builds the file from a form. It checks as you type with the same rules as step 4, can start from any published skill, and opens the pull request for you. The rest of this guide is for writing the file by hand — start from [`skills/what-to-wear.pbskill`](skills/what-to-wear.pbskill).

## 1. Write the file

```json
{
  "pbskill": 1,
  "skill": {
    "format": 2,
    "id": "your-skill",
    "name": "Your skill",
    "version": 1,
    "author": "your name or handle",
    "description": "the user asks …",
    "triggers": {
      "phrases": ["short lowercase cores"],
      "examples": ["A real request that should fire it"],
      "not": ["A line with a phrase in it that must not fire it"]
    },
    "reads": ["weather"],
    "card": "checklist",
    "guards": ["checklist:strip_brackets", "speech:no_unbacked_claims"],
    "actions": []
  },
  "prompt": "…",
  "tests": [ … ],
  "readme": "What it's for, in a sentence or two."
}
```

| Field | Rules |
|---|---|
| `id` | Lowercase words joined by hyphens. Not the name of a built-in skill. The file must be named `<id>.pbskill`. |
| `description` | Starts "the user asks". It's read by Peanut Butter's router, not by people. |
| `triggers.phrases` | Short, lowercase, no apostrophes. A message must contain one before the model is even asked. Be generous — a phrase that fires wrongly costs a quick "no"; one that misses means the skill never runs. |
| `triggers.examples` | Each must contain one of your phrases. They run as "must fire" tests. |
| `triggers.not` | Lines that contain a phrase but must not fire. They run as "must not fire" tests. |
| `reads` | Any of `memory.search`, `commitments`, `commitments.soon`, `calendar.today`, `weather`, `things_to_try`, `profile`. Add `:N` to size a source (`memory.search:6`). |
| `card` | `checklist`, `plan_grid` or `ask`. An `ask` card needs `ask.slots` and a `resolve` prompt. |
| `guards` | Checks Peanut Butter runs on the model's output. See the list below. |
| `actions` | Any of `add_to_list`, `set_reminder`, `add_thing_to_try`. Only for an `ask` card: they are offered as buttons on the confirmation that follows the answer, and each happens only after the user says yes. `set_reminder` shows its button only when a time can be read from the confirmation's title or details. |

### Prompt variables

Your prompt may only use variables its `reads` fill. The validator refuses anything else, because a blank section is exactly what makes a small model invent facts.

| Always | From `reads` |
|---|---|
| `{{REQUEST}}` `{{DATE}}` `{{TIME}}` `{{PROFILE}}` `{{USER_NAME}}` | `memory.search` → `{{QUERY_RESULTS}}` · `commitments` / `commitments.soon` → `{{COMMITMENTS}}` · `calendar.today` → `{{CALENDAR}}` · `weather` → `{{WEATHER}}` · `things_to_try` → `{{THINGS}}` |

A resolve prompt can also use `{{SLOT}}`, `{{ANSWER}}` and `{{KNOWN}}`.

### Guards

| Guard | What Peanut Butter does |
|---|---|
| `items_from:commitments` | Checklist items are the open tasks, in deadline order |
| `plan:times_per_week` | "N times a week" keeps N planned days per week |
| `plan:weeks` | "N weeks" caps the rows |
| `plan:empty_days` | A cell saying Rest/Off/None becomes an empty day |
| `ask:no_echo` | A question that repeats the request is replaced |
| `ask:known_must_be_given` | "Not specified" isn't shown as a known detail |
| `checklist:strip_brackets` | "[3 days ago]" loses its brackets |
| `names:heard` | Checklist items must be made of words the skill was given |
| `speech:no_unbacked_claims` | Drops spoken sentences that offer actions, claim to see, or recall what wasn't given |

Use `speech:no_unbacked_claims` on every skill unless you have a reason not to.

## 2. Write prompts for a small model

Skills run on a 4-billion-parameter model on the phone. These rules come from prompts that failed:

- **Describe the output; never quote an example sentence.** A quoted example is a sentence the model will one day say to someone.
- **Let the model write words and let Peanut Butter decide facts.** Counts, dates, lists and names belong to sources and guards.
- **Keep every string short.** The card is read aloud and drawn on a phone.
- **Say what to do when information is missing.** Every source says in words when it has nothing; tell the model to say so too.

## 3. Write tests

Community skills must include at least one test. A test is a request, the sandbox it runs in, and properties the result must have — never exact wording.

```json
{
  "say": "What should I wear today?",
  "given": {
    "weather": { "tempF": 48, "description": "steady rain" },
    "calendar": [ { "title": "Walk to the office", "at": "08:30" } ]
  },
  "expect": { "fires": true, "card": "checklist", "items_min": 2 }
}
```

`given` can hold `commitments` (`due`: `"-2d"`, `"0d"`, `"+3d"`), `memories` (`ago`: `"3d"`), `things_to_try`, `calendar` (leave it out for "no calendar", `[]` for an empty day) and `weather`. `expect` can check `fires`, `card`, `items`, `items_min`, `speech_mentions` and `speech_not`.

The app runs these on the phone before install. Run the checks below before you open a pull request; the model-based tests run when someone installs your skill.

## 4. Check it

You need Java 21.

```sh
java -jar tools/pbskill.jar check skills/your-skill.pbskill
```

Fix everything it lists. The same check runs on your pull request.

## 5. Open a pull request

Add your file under `skills/` and open a pull request. By submitting a skill you agree to share it under the [MIT License](LICENSE), so anyone can use and adapt it with attribution. A reviewer looks at whether the skill is useful, whether it reads only what it needs, and whether its prompt is written for a small model. Updates to an existing skill must bump `version`.

## What gets declined

- Skills that read more than they need
- Prompts that ask the model to invent, guess, flatter or pretend to act
- Anything aimed at monitoring, judging or reporting on another person in the household
- Anything that impersonates a person, brand or service
