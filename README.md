# oefentoetsen-skills

Two skills for [Claude Code](https://claude.com/claude-code) that turn your child's school goals into practice, for Dutch primary school (groep 5–8):

| Skill | What it does |
|---|---|
| `les-toets-bouwen` | Writes an interactive lesson and a matching test from the school's goal sheet, a classroom poster or a workbook page. Checks its own answer keys before handing over. |
| `nakijken` | Marks an attempt the way the school does, explains every lost point to the child, finds the pattern behind the mistakes and keeps short learning notes for the next round. |

They work with the [oefentoetsen](https://github.com/eduelias/oefentoetsen) player, a small self-hosted web app the child uses on a tablet. Everything stays on your own computer, apart from what your AI agent sends to its model provider.

## Install

First get the player and check that it runs:

```bash
git clone https://github.com/eduelias/oefentoetsen.git
```

Then add the skills, inside Claude Code:

```
/plugin marketplace add eduelias/oefentoetsen-skills
/plugin install oefentoetsen-skills@oefentoetsen
```

Or copy the folders in `skills/` to `~/.claude/skills/`.

## Use

Start Claude Code in the player folder and ask in your own words:

- "Maak een les en een oefentoets voor rekenen blok 3. Dit is het doelenoverzicht van school." (add the PDF or a photo)
- "Kijk de laatste toets na."
- "Maak een vervolgtoets over wat nog niet goed ging."

## What the skills insist on

- The school's material decides what is right, also where it differs from general knowledge.
- A child is tested only on what has been studied.
- The lesson practises in the same format the test asks, with other items.
- Answer keys are recomputed a second way and run through `tools/check.js` before a child sees them.
- Marking is strict, like at school, and every lost point gets a reason.

## School material and privacy

Teaching methods are copyrighted. The skills follow a method's goals and order and write every question new; they do not copy exercises, word lists, maps or pictures. Tests you make from your school's own lists are for home use: do not publish them.

Results and learning notes describe a child. They stay in the player's `results/` folder, which is never served or committed. The material you give your AI agent is sent to its model provider; leave the child's surname and the school's name out.

## License

MIT, see [LICENSE](LICENSE).
