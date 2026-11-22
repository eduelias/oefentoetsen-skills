---
name: les-toets-bouwen
description: Build an interactive lesson and a matching practice test for a Dutch primary-school child (groep 5-8) in the oefentoetsen player, from the school's own goals or material. Use when a parent or teacher says "maak een oefentoets", "maak een les over", "oefenen voor de toets van blok 3", "practice test for rekenen/taal/spelling/aardrijkskunde", shares a goal sheet, poster photo or workbook page and wants practice from it, or asks for a follow-up round on a child's weak spots.
license: MIT
---

# les-toets-bouwen

You write one lesson and one test that belong together, check them, and hand them over ready to use. The child sees Dutch; write all child-facing text in simple Dutch for the group level.

## Where you work

Inside a checkout of the [oefentoetsen](https://github.com/eduelias/oefentoetsen) player: the folder has `engine/grade.js`, `tests/`, `lessons/` and `tools/check.js`. If it does not, stop and say the player is needed.

The formats are in that repo's `README.md` ("Writing a test", "Writing a lesson"). Read those two sections before writing, and copy `tests/voorbeeld-vraagtypen/test.json` and `lessons/voorbeeld-kommagetallen.html` as templates. Do not invent fields.

## What you need before you write

- **The school's material**: a goal sheet (doelenoverzicht), a classroom poster, a workbook page, or the parent's description. It tells you the goals and the models the teacher uses.
- **What the child has studied.** Test only that. If it is unclear which blok or chapter is done, ask once.
- **What the child gets wrong**, if this is a follow-up: read `results/leerling.md` when it exists, the last result for that subject (`node tools/report.js`), and the lesson logs in `results/progress/les-*.json`.

## Rules for the content

1. **The school's material decides what is right.** Definitions and models differ per method (which countries count as Scandinavia, which rule decides d or t). Use the material's version, also when general knowledge says otherwise. Use the same model the teacher uses (verhoudingstabel, getallenlijn, the school's stappenplan), not a different trick.
2. **Follow the goals, write everything new.** You may follow a method's goals, their order and its type of exercise. Every sentence, number, story and picture in the lesson and test is your own. Do not copy exercises, word lists with their definitions, maps or illustrations from the material into anything that could be shared. If the parent wants the school's exact word list practised, keep that test in their private copy and say it must not be published.
3. **Tag every section with a `goal`**, such as `B3D2` (blok 3, doel 2) or a short name. Results are scored per goal.
4. **Lesson first, test second.** Set `"lesson": "lessons/<id>.html"` on the test so it stays locked until the lesson is done.
5. **The lesson practises in the format the test asks.** If the test has the child tap words, the lesson has them tap words. A child who learned with buttons and is tested with tapping fails on the format. A lesson has three kinds of exercise (typed, pick, tap), so keep most of the test in `fill`, `mc`, `tf` and `words`; practise a `match` or `tf` as pick. Open questions are the exception: the lesson cannot practise them, so the question itself says how many things to write.
6. **Test items differ from lesson items**: same kind of mistake possible, other numbers or sentences.
7. **Dutch conventions**: decimal comma (2,5), dot for thousands (3.000.000), `×` and `:` for multiply and divide, grades from 1 to 10.

## How to teach in the lesson

- **One concrete model for the why.** Money, a measuring jug, a number line. One model per idea, used on every slide of that idea. Draw the teacher's model yourself as a simple SVG and reuse the same drawing style in the test.
- **Let the wrong method fail.** If the child is known or likely to use a wrong rule, give an exercise where that rule produces an answer the child can see is wrong, then show the right one. Telling the rule again does not work.
- **Fade the help**: a worked example, then one with gaps, then one alone.
- **Feedback for the likely wrong answers.** For each exercise, work out what the common wrong methods produce and put those in `data-fb` with one sentence that names the mistake. Compute these; a feedback text that does not match its key is worse than none.
- **Hints go from vague to exact.** `data-hint` points at the method, never at the answer. The player shows the answer itself after three tries.
- **Explanations need both halves.** Children stop at the cause and skip the consequence. In open questions say how many things to write ("Schrijf twee dingen op: …") and give the rubric one line per point.
- **Keep it short.** A lesson is three to five slides including the closing one, about ten minutes. A test is three to six sections, fifteen to twenty-five minutes.

## How to write the test

- Mix question types; the showcase test lists them all. Use `open` once or twice for "leg uit" and give `points`, `model` and `rubric`.
- Maths answers: `num: true`. Fractions: `frac: true`. Times and other fixed strings: list the accepted spellings and set `near: false`.
- Names (places, people) stay case-sensitive, because schools mark them that way. Ordinary words get `case: false`.
- Accept every spelling the school would accept; list alternatives in `answer`.
- No give-aways: spread the right answer over the positions in `mc`, do not make every `tf` statement true, add a decoy to a `wordbank` or `match`, and make sure one question does not answer another.
- Word the question so it cannot be read two ways. If a blank wants the answer of the sum and not of the check, say so.

## Check before you hand over

1. Run `node tools/check.js <testId>`. Fix every error. Read every warning and decide; a `t` before `jij` or `je` is a real mistake unless the stem ends in t (eet jij, kom jij).
2. **Recompute every key a second way.** Maths: a short script with exact arithmetic (fractions as fractions, decimals as decimals), not mental arithmetic. Language: apply the school's rule step by step to each item. Better still, give a fresh subagent the questions without the keys and compare.
3. Check each `data-fb` key: enter it in the lesson and see that its feedback appears. `tools/check.js` does not check lesson answers.
4. Click through the lesson and the test in a browser (`python3 serve.py`, then the overview page) when you can. Say so if you could not.
5. Remove the trial attempts you made from `results/`. On a machine that holds a child's real results, delete only the files your own run created.

## Hand over

Tell the parent: the test id and lesson id, the number of points (auto and manual), which goals and which weak spots it targets, and what you did not check. If the player runs on another machine, copy the new files there; never copy `results/` or `grade.key` over the ones on that machine.

A child's name belongs only in the `student` field of a private copy. Never commit results or a child's name to a public repository.
