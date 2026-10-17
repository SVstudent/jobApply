# jobApply: handoff queue for Meta Muse

This is the queue of Summer 2027 internship applications that Claude Code couldn't finish because each one needs an account or login (Workday, iCIMS, Phenom, Avature, Oracle, SuccessFactors, Eightfold, and company career portals such as IBM, Cisco, NVIDIA, Amazon and TikTok). Claude Code already handles every no-login form (Greenhouse, Ashby, Lever, etc.), so nothing in this queue overlaps with its work.

## Files

| Path | What it is |
|---|---|
| `queue/roles.csv` | **The work queue.** One row per role, newest first. Columns: `id, company, title, location, posted, apply_url, linkedin_url, portal, suggested_resume, status` |
| `profile/PROFILE_AND_RULES.md` | Candidate profile and the answer to every common form question: contact info, school, dates, work authorization, experience, projects, honors, essay guidance. **Answer forms only from this file.** |
| `resumes/*.pdf` | Résumé variants. Upload the one named in `suggested_resume` (ml_infra, ai_agents, backend_data, fullstack_product, quant_dev, general). |
| `essays_samples/` | Recent essays, for his voice and facts. Write new essays for each role; don't copy these. |
| `applied/already_applied.csv` | Everything already applied to or ruled out. **Check it before every application.** |
| `status/muse_log.csv` | Append one row per role you handle. |

## Procedure for each row with `status=TODO`

1. **Find the apply page.** Use `apply_url` if it's set. Otherwise open `linkedin_url`, read the posting, and follow its **Apply** button to the company's own site. Apply on the company site, not with LinkedIn Easy Apply.
2. **Skip duplicates.** Skip the row if the same company plus role, or the same job ID, appears in `applied/already_applied.csv` or `status/muse_log.csv`.
3. **Check eligibility.** Skip the row and log the reason if any of these is true:
   - The term doesn't include Summer 2027 (or Summer 2028), for example a Jan–Apr, Spring or Fall co-op.
   - It requires a Master's or PhD.
   - It requires US citizenship or a security clearance. He holds a **green card**, so "US person", ITAR, and "authorized to work without sponsorship" are all fine.
   - It requires a transcript, an assessment you would have to take for him, or a no-AI certification.
   - It requires a start date before June 1, 2027 with no flexibility.
4. **Apply.**
   - Sign in with his existing account, or create one with his email `vempati.honey@gmail.com`.
   - Upload `suggested_resume`.
   - Fill every field truthfully from `profile/PROFILE_AND_RULES.md`. That includes LinkedIn, GitHub, portfolio, Devpost, GPA 4.00, graduation June 2028, and a start date of June 1, 2027.
   - Decline the voluntary EEO questions.
   - Write role-specific short answers. Open each with the most relevant real proof: 5× TensorRT inference speedup, RadWatch data loss cut from 8.2% to 0.05%, Stanford EEG latency down 34%, and his hackathon wins.
   - Never invent experience, titles, scores or references.
5. **Respect company limits.**
   - Optiver: one active application.
   - Jane Street: one across all roles.
   - Coinbase: 3 per 6 months.
   - Google: 3 per 30 days.
   - Waymo: already at 3.
   - Never apply to Scale AI, Ramp, Flint, KP Fellows, Netic or Revel.
6. **Log the result.** Append to `status/muse_log.csv` using `id`, company, title, the URL used, a status, today's date and a short note. Use one of these statuses:

   | Status | Meaning |
   |---|---|
   | `SUBMITTED` | Confirmation page or email received |
   | `SKIPPED-INELIGIBLE` | Fails one of the eligibility rules |
   | `SKIPPED-DUPLICATE` | Already applied |
   | `SKIPPED-LIMIT` | Company limit reached |
   | `BLOCKED-CAPTCHA` | Stopped by a CAPTCHA or bot check |
   | `NEEDS-USER` | Needs information only he has |

   Then set the row's `status` in `queue/roles.csv` to the same value.

Work newest-first and commit progress often. Claude Code reads `status/muse_log.csv` back into its master log so neither side applies to the same role twice.
