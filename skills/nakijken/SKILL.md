---
name: nakijken
description: Mark a child's attempt in the oefentoetsen player the way a Dutch primary school would, explain every lost point, find the pattern behind the mistakes and update the child's learning notes. Use when a parent or teacher says "kijk de toets na", "nakijken", "hoe ging de toets", "mark the test", "what did she get wrong", "wat moet er nog geoefend worden", or when a new result has come in and a follow-up round is wanted.
license: MIT
---

# nakijken

You mark one attempt, tell the child what went well and why each point was lost, and tell the parent what to practise next. Text for the child is simple, kind Dutch.

## Where you work

Inside a checkout of the [oefentoetsen](https://github.com/eduelias/oefentoetsen) player, on the machine that holds `results/` (or with those files copied here). Marking and the result format are in that repo's `README.md` ("Marking").

## Steps

1. **Find the attempt.** `node tools/report.js` takes the newest file in `results/`; pass a file name for another one. It prints the score per goal, every wrong item with what the child typed, and the open answers that wait for marking. An unfinished attempt is in `results/progress/<testId>.json`.

2. **Check the key before you blame the child.** For each wrong item ask: is the child's answer in fact right? Answer keys have bugs, and a correct alternative spelling may be missing. If the child is right, fix `tests/<id>/test.json`, give the points in `manual`, and tell the parent the key was wrong. An answer that shows the right knowledge in a form the question did not ask for ("6/8" where the blank is followed by "achtsten") is not a key bug: leave the score, and report it under step 4.

3. **Mark the open and drawn answers** against the question's `rubric`, one rubric line at a time. A line is earned or it is not; give half a point only when half of that line is there. Drawings are stored as data URLs: decode to a PNG and look at it. Judge by the school's material when you have it, otherwise by the question's `model` and `rubric`; not by general knowledge.

4. **Mark strictly, the way the school does.** A place name without its capital is wrong. A spelling slip in a spelling test is wrong. Do not round up out of kindness; a practice grade that is higher than the school's grade helps nobody. Leniency is the parent's call: mention where you would consider it and why.

5. **Write the marking.** Either edit the `manual` object in the result file, or post it:

   ```
   POST /api/grade/<file>
   {"manual": {"<questionId>": {"points": 1, "comment": "…"}, "_comment": "…", "_done": true}}
   ```

   Every open or drawn question gets `points` and a `comment`. An auto-checked question gets a `comment` only where the child lost points and the reason is worth saying; leave `points` out there unless you are overriding the score. A comment is one or two sentences to the child: what was right, why the point was lost and what would have earned it. `_comment` is the overall message: start with what went well, then the one thing to work on. The grade appears once `_done` is true, so send it when all marking is in (the same request is fine). Posting needs the server running.

6. **Find the pattern.** Sort every mistake into one of these, with the child's own answers as evidence:
   - **Wrong rule**: the same mistake on several items. State the rule the child seems to use and check that it predicts the wrong answers given. This is the valuable finding.
   - **Slip**: right method, wrong copy or a misread question word (steden, delen, landen). Look for a repeated previous answer.
   - **Format**: the picture and true/false questions on a topic are right and one question type is wrong. The question confused the child, not the topic.
   - **Spelling or capitals** on words the child knew.
   - **Half an explanation**: the cause without the consequence, or the rule without the answer.

   Read the lesson log too (`tools/report.js` prints the exercises that were not right the first time): first tries show what the child believed before the lesson corrected it. Compare `started` and `finished` with the amount of work.

7. **Update the learning notes** in `results/leerling.md` (create it if needed; it sits beside the results and is never served or committed). When more than one child uses the player, keep one file per child: `results/leerling-<voornaam>.md`. Write the notes and the report in the language the parent writes in. Keep it short and dated: the score, what is now fixed, what is still open in order of importance, and what kind of question to use or avoid next time.

8. **Report to the parent**: the grade and score, the score per goal, the two or three findings that matter with the child's answers quoted, anything unusual stated plainly and without a verdict (an open answer that reads like copied text, a very short or very long session), and what you recommend for the next round. If the score is below full marks, offer a follow-up round on the open points only, built with the `les-toets-bouwen` skill.

## Never

- Change the child's answers, or lower points without saying so.
- Call a child lazy or careless, to the child or the parent. Describe what happened.
- Publish or commit a result, a child's name or the learning notes.
