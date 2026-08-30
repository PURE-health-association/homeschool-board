# START HERE

Read this first when you come back to this project.

## What this is

A learning board for the kids, built as **one single file: `index.html`**.
Everything is in that one file — the pages, the styling, the code. No build
step, nothing to install, no other files to manage.

**Live site:** https://homeschool-board.vercel.app/

## How to pick it back up

Open Claude Code in this folder and just say what you want changed.
For example: *"add a new video to the Reading section"* or *"the Money quiz
is too hard, make it easier."*

You do **not** need to copy files, run git commands, or install anything.
Claude edits `index.html` and pushes it. About 30 seconds later the live
site updates by itself. Your only job is to look at the site and say what
you think.

## What's on the board right now

Eight subjects, all working:

| Tile | What's in it |
|---|---|
| **Time & Calendar** | Days, months, seasons, 3 videos, quiz |
| **Clock** | Reading an analog clock, quiz |
| **Money** | Coins, bills, counting, *Enough?*, *Making Change*, quiz |
| **Devotions** | Grace and bedtime prayer, 2 videos |
| **Reading** | Alphabet, 2- and 3-letter words, word groups, Dolch words, quiz |
| **States** | Texas home base, regions, compass, all 50 capitals, family travel map, quiz |
| **Healthy Me** | Daily checklist, hygiene cards, body cards, quiz |
| **Screen Smart** | Screen rules, *Real or Not?*, quiz |

Tapping almost anything reads it out loud. Every subject has a **Read Aloud**
button at the top right that explains the page the child is on.

## Two things that save on the device

These remember themselves in the browser, per device:

- **States → My Travels** — which states the family has lived in / traveled to.
- **Healthy Me → My Day** — the daily checklist. **It clears itself every
  morning** on purpose, so each day starts fresh.

They are saved on whichever phone or tablet the child used. They don't sync
between devices.

## Known to-dos for next time

1. **The GitHub token is sitting in plain text** in this repo's git settings.
   Anyone who gets it can push to the repo. Worth revoking at
   github.com/settings/tokens and letting git ask for login normally.
   Claude can rewire this in one step — just ask.
2. **Reading → Videos tab is still empty.** It's waiting on YouTube video IDs.
   When the NotebookLM videos are up, give Claude the ID and title.
3. **Cities for the travel quiz** — states are a fixed list of 50, but cities
   aren't. Tell Claude which cities matter to the family and they can be added.
4. **Ideas not built yet:** emergency info (911, address, phone number),
   measurement, skip counting / times tables, calendar math.

## If something looks broken

Say what you tapped and what happened (or didn't happen). Claude can open the
live site, click it, and read the error directly — no need to describe it
perfectly or dig into settings.
