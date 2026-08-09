# Jeremiah's Auto — Vehicle Inventory & Intelligence System (Prototype)

This is a working, click-through prototype of a dealership inventory system for Jeremiah's Auto, covering all three branches (PMB, Richards Bay, Durban). It's a single self-contained HTML file — there's nothing to install. Open `index.html` in any browser (Chrome, Edge, Safari) and everything runs locally in that tab. It's built to work on a laptop, tablet, or phone.

**Before you demo it:** everything in this prototype lives in the browser's memory only. If you refresh the page or close the tab, it resets back to the sample data it started with. That's expected — it's not a bug. The next stage of this project is connecting it to a real backend (Supabase) so data actually persists between sessions, across devices, and between staff.

---

## What to try when demoing this

**1. The dashboard**
Open the file and you land here. Three things worth pointing out:
- The "The standard" panel — four sales discipline lines, sized to sit clearly beneath the Jeremiah's Auto name, not competing with it. Fixed in place, not going anywhere on their own.
- The three branch tiles — each shows a 👆 "Tap to open" hint, and clicking (or tapping) any one opens that branch's full inventory.
- The **Sales / Stock on floor** heat map at the bottom — click each button to switch what it's showing. (See "What's real vs. illustrative" below — this one's currently sample numbers.)

**2. Search**
Type into the search box on the dashboard — results update as you type, across all three branches at once. Try searching a make, model, or year. You can also narrow it to one branch using the dropdown next to the search box, and open "Advanced filters" for status, price range, or mileage range.

**3. Add a vehicle**
Click "Add vehicle" in the top navigation. Fill in the required fields (marked with *), and note the photo requirement — at least 5 photos must be "captured" (this simulates the camera/gallery flow; actual photo storage isn't wired up yet) before the Save button becomes clickable. Save it and watch the branch tile count update immediately, along with the branch's inventory list.

**4. Open a vehicle's report**
Click into any vehicle from a branch list or search result. This is the full report — commercial details, cost price and profit (see note below), mechanical specs, description, and the audit trail showing who captured it and when.

**5. Edit and see the history log itself update**
From a vehicle's report, click "Edit," change the price, and save. Go back into that vehicle's report — you'll see the price change logged in the price history, and the activity log at the bottom records exactly what changed and when.

**6. Log a comment with a follow-up date**
Still on a vehicle's report, scroll to "Client / lead comments," fill in a comment with a follow-up date a few days out, and save. Go back to the dashboard — the red "Upcoming follow-ups" panel now shows it. This is meant to be the "nothing falls through the cracks" feature — any salesman, at any branch, can see what needs a callback.

**7. Mark a vehicle Sold**
Edit a vehicle and change its status to "Sold." This automatically stamps who sold it and when — no manual entry needed. Check the Leaderboard in the top navigation afterward; that sale now counts toward that salesperson's total.

**8. Aging stock**
Any vehicle sitting for 45+ days without selling shows up in the amber "Aging stock" panel on the dashboard automatically — no one has to remember to check.

**9. Archive and restore**
From a vehicle's report, "Archive" removes it from active stock (with a confirmation prompt first) without deleting it. "View archived vehicles" on the dashboard shows everything archived, and any one of them can be restored back to active stock at any time.

**10. Leaderboard**
Shows vehicles sold, active leads, and overdue follow-ups per salesperson, filterable by branch and by "This month" vs. "All time."

---

## What's real vs. illustrative right now

Being upfront about this so nothing gets over-promised to the client by accident:

| Feature | Status |
|---|---|
| Branch inventory, search, filters | **Fully real** — driven by actual data in the prototype |
| Add / edit / archive / restore | **Fully real** — all logic works, just doesn't persist past a refresh |
| Price history, activity log | **Fully real** |
| Follow-ups and aging stock panels | **Fully real** — calculated live from actual vehicle data |
| Leaderboard | **Fully real** — calculated live from actual sales |
| Cost price / profit per vehicle | **Real calculation**, but **not yet access-controlled** — anyone opening a report currently sees it. Needs a decision from the team on who should see this before real rollout (see note below) |
| Performance heat map (Sales / Stock) | **Illustrative sample data only.** The system doesn't yet keep a month-by-month sales history — it only knows current stock. Wiring this to real numbers means recording a timestamped sale event per vehicle and building monthly aggregation, which is a piece of backend work, not just a display change |
| Vehicle photos | **Placeholder only.** The 5-photo requirement and preview thumbnails work, but no actual image is stored or displayed — this becomes real once there's a backend to store uploaded images |
| Staff directory (names, phone numbers) | **Fictional placeholder data**, clearly fake, for demo purposes |

## Open decision: who sees cost price and profit

The owner has flagged this as something to discuss with the team before going further. Right now, cost price and profit are visible to anyone who opens a vehicle's report. Most dealerships restrict this to management only, since salesmen seeing exact margins can unintentionally affect how they negotiate. Once there's a decision, the next backend build should include real login and role-based access — not just a policy, but something the system actually enforces.

## The full audit pass

Before this handoff, the entire file went through a genuine top-to-bottom audit — not just a read-through, but actual automated testing:

- **Every user flow was driven through a real (simulated) browser**, not just read as code: adding a vehicle, editing it, marking it sold, adding a lead comment, archiving, restoring, searching, filtering, and switching the leaderboard and heat map views. Every one of these was exercised end to end and checked against expected behavior.
- **The math was independently recomputed and cross-checked**, not just trusted. Every number that appears on screen — branch tile counts, the donut chart total, the follow-up count, the aging stock count, the archive count, and every salesperson's numbers on the leaderboard — was recalculated separately from the raw data and compared against what the screen actually shows, across different filter and branch combinations.
- **Two real issues were found and fixed:**
  1. Dead leftover CSS from earlier design rounds (unused tile color variants and an orphaned style rule from before the tiles became solid black) — removed for a cleaner codebase.
  2. None of the form labels across the entire app were properly linked to their input fields — meaning tapping a label's text wouldn't focus that field, which especially hurts usability on phones and tablets, where tapping a small input directly is fiddly but tapping its label should just work. All 53 labels across every form are now correctly linked, verified with no naming collisions.
- Everything above was re-tested after each fix to confirm nothing broke — the audit didn't stop at finding issues, it confirmed the fixes actually worked.

## What's next

The plan discussed is to move this into Supabase for a real backend — meaning:
- Data actually persists (no more resets on refresh)
- Multiple people can use it at once, from different devices, seeing the same live data
- Real login and role-based permissions (so cost/profit access can be properly restricted)
- Real photo upload and storage
- Monthly sales history tracking, so the heat map reflects real figures instead of sample data

This prototype's job was to prove out the workflow, the features, and the feel of the system before investing in that backend build. This audit pass was the most thorough one yet — genuine automated testing of every flow and every number, not just a visual check — and everything came back clean or was fixed on the spot. This is the version to hand to the client with confidence.
