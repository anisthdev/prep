# prep

A tracker for interview preparation. It logs DSA attempts and schedules
re-solves for the ones that weren't clean, times attempts against the 35
minutes of an interview, logs study time, mock interviews and contests, keeps
an application pipeline and behavioural stories, and measures progress
against a phased plan with an exit test for each phase.

It is a plain CLI over plain files. quickbar shows it as a bar pill, a
dashboard and a menu (Super+D), but nothing here depends on that.

## Use

```sh
prep                              # where you stand (same as prep status)
prep log two-sum s                # solved clean
prep log "Group Anagrams" h       # solved with hints: re-solve in 3 days
prep log https://leetcode.com/problems/majority-element/ f -p arrays -d e -m 25
prep timer start top-k-frequent-elements
prep log top-k-frequent-elements s   # takes the minutes from the timer
prep undo                         # remove the last attempt

prep study 90 "Java concurrency"
prep mock pass -k design -m 60
prep contest 2 -r 15230

prep app add Acme -r "a friend"
prep app stage acme applied   # wishlist referral applied oa screen loop offer signed rejected withdrawn
prep app next acme "OA on Friday"

prep stories                      # STAR stories, … marks ones still to write
prep edit stories                 # also attempts, events, apps, plan
prep problems -p two-pointers     # the list, ✓ clean, ~ hints, ✗ failed
```

Results are `solved`, `hint` or `fail` (`s`, `h`, `f`). A problem is a
slug, its title or its LeetCode URL. Problems on the bundled list (NeetCode
150) know their pattern and difficulty; others take `-p` and `-d` the first
time and remember them.

## The plan

`plan.json` holds the phases (dates, focus, exit test and how it is
measured), a weekly focus for the first weeks, the re-solve gap (3 days),
the timer length (35 min) and the application stages. The bundled one is
an example: `prep edit plan` copies it to `~/.config/prep/plan.json`, which
is read instead, for your own dates. Keep your own plan there, out of this
repository. Each phase's `metric` is one of:

| `kind` | Measures |
|---|---|
| `studied-days` | days studied in the last `window`, against `target` |
| `fresh-mediums` | first attempts at mediums in the last `window` days solved clean within `minutes`, against `target` (a share), judged once `minimum` are timed |
| `mocks-and-loops` | mocks passed (`mocks`) and applications that reached `loop` (`loops`) |
| `offer` | an application at `signed` |

A day counts as studied if anything was logged on it.

## Files

In `$XDG_DATA_HOME/prep` (`~/.local/share/prep`):

- `attempts.jsonl`: one attempt a line,
  `{"at", "date", "problem", "title", "pattern", "diff", "result", "mins", "retry", "note"?}`.
  `retry` is the re-solve date, null when solved clean. A problem's latest
  attempt decides whether it is due.
- `events.jsonl`: `{"at", "date", "type": "study"|"mock"|"contest", "mins", …}`.
- `applications.json`: `[{"company", "stage", "history", "referral", "next", "notes", "updated"}]`.
- `stories.md`: Markdown, a `## ` heading a story.
- `.changed`: rewritten in place on every change, for frontends to watch.

The running timer is `$XDG_STATE_HOME/prep/timer.json`. Bundled files
(`problems.json`, `plan.json`, `stories.md`) are read from
`~/.config/prep/` first, then `/usr/share/prep/`.

## For frontends

`prep status --json` has everything a dashboard needs: `phase` (with
`metric`: `label`, `progress` 0–1, `met`), `week`, `thisWeek`, `strip` (the
last 14 days), `due` and `upcoming` re-solves, `patterns`, `totals`,
`recent`, `timer` (`started`, `problem`, `title`, `minutes`) and
`applications`. `prep problems --json` and `prep stories --json` list those.
Write through the CLI, so the files keep one writer.

`PREP_TODAY=YYYY-MM-DD` pretends it is another day, for trying things out.
