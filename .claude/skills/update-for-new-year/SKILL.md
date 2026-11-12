---
name: update-for-new-year
description: Update the 社会と科学技術の界面 (biodiversity / iNaturalist) course slides and Moodle quiz for a new academic year. Use when the user says to update the course/slides/website for this year, next year, or a new year, or asks to change the class dates.
---

# Update the course for a new year

This Quarto project has two revealjs slide decks (`day1/index.qmd`, `day2/index.qmd`) and a Moodle question file (`quiz/moodle-questions.xml`). Almost everything year-specific is a date, a year in a URL, a stat, or a screenshot.

## 0. Ask first

Ask the user for anything you can't derive. Don't guess dates.

- Day 1 date (classroom session; quiz + iNat install) and Day 2 date (field survey).
- Rain-backup date (補講日).
- Whether the iNaturalist app UI changed (screenshots need retaking).

Then work out the old values by reading the files, not from this skill, since they change every year.

## 1. Dates and years: `day1/index.qmd`

Search with: `grep -n -E "202[0-9]|[0-9]+月[0-9]+日" day1/index.qmd day2/index.qmd`

Places that usually change:
- `## アウトライン（M月D日）` — two headings (Day 1, Day 2).
- `## クイズについて` — quiz date.
- `## 野外実習について` — field-day date, rain-backup date (appears twice in one bullet), and "iNaturalist install happens at the end of the (Day 1) class" date.
- Syllabus links near the top: `https://syllabus.gs.chiba-u.jp/<YEAR>/...` (two links). Check each link still resolves.

## 2. Day 2 date filter and stats: `day2/index.qmd`

- `「Date Observed」→「Exact Date」→ \`YYYY-MM-DD\`` — set to the new field-survey date.
- `## 千葉大学西キャンパスの現状` — the 現在 numbers (観察 / 種 / 観察者) are a snapshot. Refresh from the live link on that slide heading (`place_id=209168`) and update. The `2024年` block is historical (pre-course baseline); leave it as is.
- Screenshots named `Screenshot YYYY-MM-DD ...` in `images/` are referenced throughout. Only replace if the iNat app UI changed; if so, retake and update every reference (`grep -n "Screenshot" day2/index.qmd`).

## 3. Links that rot — do not add Moodle URLs

Moodle URLs change every year. Never put a Moodle link in the slides; refer to the section by name instead (for example 「Moodleの『第11, 12回』のセクションを確認してください」). If the section numbers (第11~12回, 第13~14回) change, update the `subtitle:` in each deck's front matter and any slide text that names them.

The `nitta9359-qr.png` QR code (day1 slides, several places) points to the user's generic Socrative room. It is reused every year, so no update is needed.

## 4. Moodle quiz: `quiz/moodle-questions.xml`

This is the Day 2 analysis assignment (Quiz activity, 12 questions). Its design:
- Q01–04 and Q10 are Numerical with a placeholder answer `0` at 0%. Q06 is multiple choice with no option marked correct. The rest are Essay. Everything is graded after the data is in, because the right answers depend on what students upload to iNat.
- Question names use two digits (Q01…Q12) so Moodle sorts them.
- Q02 says 今日の日付（調査日） instead of a hardcoded date. That is correct only because students do the analysis the same day as the survey. Keep it that way.
- Question text has no leading "1." numbers, since Moodle numbers quiz items itself.

For a new year:
- Check that nothing in the XML hardcodes a year or date (`grep -n -E "202[0-9]" quiz/moodle-questions.xml`).
- Check the `Place` names (`Chiba University Nishi-Chiba Campus, ... West Half / East Half`) still exist on iNaturalist.
- Re-importing creates duplicates. Delete last year's questions from the question bank category 生物多様性データ解析 first, or reuse last year's quiz by copying the Moodle course.
- The user has chosen to parse the exported Responses CSV with code after submission instead of regrading in Moodle. Don't push for re-import just to change answer settings.

## 5. Check and render

- Re-grep for the old year and old dates to catch leftovers: `grep -rn "<OLD_YEAR>" day1 day2 quiz --include='*.qmd' --include='*.xml'`.
- Validate the XML: `python3 -I -c "import xml.dom.minidom as m; m.parse('quiz/moodle-questions.xml')"`.
- Render with `quarto render` (output goes to `_site`, which is gitignored). Open the result and check the date slides.
- Don't commit unless the user asks. Their usual commit style is a short message like "Update content to <YEAR>".

## 6. Publish the website

The site is published with `quarto publish gh-pages`, which renders the project and pushes the output to the `gh-pages` branch. Pushing `main` alone does **not** update the live site; `_site` is gitignored, so the published pages only change through `quarto publish`.

- Publishing is public and outward-facing: confirm with the user before running it.
- Commit and push `main` as well if the user wants the source saved, but treat that as separate from publishing.
- After publishing, check the live site for the new dates.
