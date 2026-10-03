[README.md](https://github.com/user-attachments/files/32999925/README.md)
# WikiTree Marathon Drawing

A live-drawing tool for the WikiTree Source-a-Thon livecast. The host pulls winners from WikiTree's Random Participant tool, and the page shows a bib slideshow that slows down and lands on each winner, with a drum roll while it spins and applause when the winner lands. It shows the winner's name, WikiTree ID, team and bib.

**Live page:** https://robinson27225.github.io/WikiTreeMarathonDrawing/

Everything runs in the browser. There is no server to maintain. The only outside pieces are WikiTree (the random tool and the bib images) and a Google Apps Script that lives in the participants spreadsheet and shares the ineligible list and participant table between hosts.

## How a host uses it

The tool has three screens.

1. **Setup**
   - Choose **Test** or **Live** at the top. Test mode shows an amber banner on every screen and saves nothing, so use it to practise. Live mode saves the ineligible list.
   - Enter how many winners to draw.
   - Add any ineligible WikiTree IDs (previous winners who have claimed their prize). Paste IDs one per line or separated by commas, or paste wikitree.com/wiki/... links, then press **Add to list**. Remove an ID with the x on its chip.
   - Add the **studio hosts** for this draw (see below).
   - Under **Draw settings** you can choose where winners come from, turn the drum roll and applause on or off, run a **Sound check**, and set the spin length (3 to 20 seconds).
   - Press **Continue**.
2. **Ready to draw**: press the WikiTree logo to start.
3. **The draw**: the bibs spin and slow down, then land on the winner. Press **Draw next winner** for each additional winner. After the last one, **Show all winners** gives a summary.

A small **Sound on / Sound off** button in the bottom-right corner mutes everything quickly.

## How winners are picked

By default the page asks the WikiTree Random Participant tool for a participant, once per winner, and keeps asking until it gets someone who is on the roster, is eligible, and has not already been drawn in the current draw.

If the tool cannot be reached, the page says so and offers **Entire eligible roster** instead. That picks at random from every eligible person on the roster, including people who have not been active recently, so use it only as a fallback.

## Who is ineligible

A participant cannot win if any of these is true:

- They have anything in the **Note** column of the participant table. That column is only for members who won in the previous marathon or who are WikiTree team members.
- A host has added their ID to the **ineligible list** for the weekend.
- They are one of the **studio hosts** entered for the current draw.

### Studio hosts (this draw only)

The hosts in the studio cannot win the current draw. Enter their WikiTree IDs in **Studio hosts** on the setup screen. This list is for the current draw only: it is not saved, not shared with other hosts, and it clears when you press **New draw**. Enter them again for the next draw.

## The shared ineligible list

The ineligible list is shared by every host and tied to the marathon weekend, **Friday 8:00 AM ET to Monday 9:00 AM ET**. It clears itself when the next Friday 8:00 AM ET window starts.

The list is stored in an **Ineligible** tab of the participants Google Sheet, through the Google Apps Script (`ineligible-sync.gs`). Hosts can also view and edit it directly in that tab. The tab's columns are WikiTree ID, Date (the Friday that starts the weekend) and Timestamp.

If the shared list cannot be reached, the page shows a warning and asks for confirmation before a draw, so nobody draws from a stale list by accident.

If a **host code** is set in the script, each host types it once, in the box under the ineligible list.

## Two codes

The script uses two different passcodes. They must not be the same.

| Code | Who has it | What it allows |
| --- | --- | --- |
| **Host code** (`PASSCODE`) | Everyone drawing | Read and edit the ineligible list, and load the participant table. |
| **Admin code** (`ADMIN_PASSCODE`) | Only the person managing the participant table | Everything above, plus publishing or removing the participant table and clearing the whole ineligible list. |

Both codes are checked by the Google script, not by the page, so neither appears in the page's source. Admin actions are refused until an admin code is set.

## The participant table

The table lives in the participants Google Sheet (the tab with WikiTreeID, Name, Team, Bib#, Image and Note columns). Hosts do not read that tab directly. An admin **publishes** it, which copies it to a **Roster** tab that every host's page reads. Each page checks for a newer table every 15 seconds, and also when it opens. If nothing has been published, pages use the table built into `index.html`.

So a change to the participant table reaches the hosts only when someone with the admin code publishes it.

## Admin view

Add `#admin` to the end of the page address (the page has no link to it). It asks for the admin code before showing anything, and it locks again when you leave the view or press **Lock admin**. The code is held only in memory.

- **Participant table:** press **Publish from the spreadsheet** to publish the participants tab of the sheet (it finds the tab automatically, or type its name). Or paste or upload a CSV, check the summary, and press **Publish to all hosts**. **Back to the built-in table for everyone** removes the published table.
- **Random participant tool:** enter the marathon's tool URL (it must start with `https://plus.wikitree.com/`, and should include the challenge name and hours) and test it. This setting is saved in the browser you use.
- **Ineligible list:** see the weekend window, test or change the shared list connection, or clear the list for everyone.

## Setting up for a new marathon

1. Update the participants tab of the Google Sheet, including the Note column (previous marathon winners and WikiTree team members).
2. Open the page with `#admin`, enter the admin code, and press **Publish from the spreadsheet**. Check the summary line under "Participant table".
3. Enter the new marathon's Random Participant tool URL and press **Test it**. Do this in the browser each host uses, or check that the built-in default is right.
4. On the setup screen, check that it says "Shared with all hosts" and "Connected to the WikiTree random participant tool", then do a practice draw in Test mode.

The table built into `index.html` is a snapshot (Oct 2, 2026, 602 participants). It is used until a table is published.

## Setting up the Google script (one time)

1. Open the participants Google Sheet and choose **Extensions, then Apps Script**.
2. Replace the sample code with the contents of `ineligible-sync.gs`.
3. Set `PASSCODE` (host code) and `ADMIN_PASSCODE` (admin code) near the top, using two different words.
4. Choose **Deploy, then New deployment, then Web app**. Set **Execute as** to **Me** and **Who has access** to **Anyone**, then **Deploy** and authorize it.
5. Copy the **Web app URL**. It is built into `index.html` (the `SYNC_DEFAULT` value), or an admin can paste it in the Admin view.
6. After any later change to the script, use **Deploy, Manage deployments, pencil icon, Version: New version, Deploy**. The URL stays the same.

This repository and page are public, so set both codes. Without them anyone who finds the web app URL could change the list or publish a different participant table.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole app in one file, including the built-in table, WikiTree logo and sounds. This is the only file GitHub Pages needs. |
| `ineligible-sync.gs` | Reference copy of the Google Apps Script. It runs inside the Google Sheet and is not served by the page. |
| `README.md` | This file. |

## Notes

- The page and repository are public. `index.html` contains the participant names, WikiTree IDs, teams and bib numbers from the built-in table.
- Bib images load from wikitree.com and are cached in the background when the page opens.
- Sound only plays after a click, which is a browser rule. The page unlocks it when the host presses Continue and the logo.
- The logo and the audio clips are embedded in `index.html`. Make sure you have permission to publish them.
- The ineligible list, studio hosts and settings are held by the browser while the page is open. Studio hosts are never saved. Clearing a browser's saved site data resets that browser's settings but does not affect the shared lists.

## Troubleshooting

| What you see | What to do |
| --- | --- |
| "Enter the host code to use the shared list" | Type the host code under the ineligible list and press Enter or **Save code**. The page tells you if it is wrong. |
| "Can't reach the shared list" | Check the connection and press **Retry**. The reason is shown in brackets. "Unreadable response" usually means the Apps Script was not deployed with access set to **Anyone**. |
| Admin says "No admin passcode has been set" | Set `ADMIN_PASSCODE` in the script and deploy a new version. |
| Admin says the admin passcode must differ from the host code | Change one of the two codes in the script and deploy a new version. |
| "That passcode isn't right" in Admin | The admin code is case-sensitive. Check it with whoever set up the script. |
| The participant table did not change on a host's screen | Press **Refresh roster** at the bottom of the setup screen, or wait about 15 seconds. Make sure someone published it from the Admin view. |
| Tool shows as not reachable | Press **Check again** under Draw settings, or confirm the URL in the Admin view. Use the whole-roster option only as a fallback. |
| A winner's bib does not appear | The participant table may be missing that person. Update the sheet and publish it again. |
| No sound | Press **Sound check**, make sure the corner button says "Sound on", and check the computer's volume. |
