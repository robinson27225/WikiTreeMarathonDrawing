[README.md](https://github.com/user-attachments/files/32999750/README.md)
# WikiTree Marathon Drawing

A live-drawing tool for the WikiTree Source-a-Thon livecast. The host pulls winners from WikiTree's Random Participant tool, and the page shows a bib slideshow that slows down and lands on each winner, with a drum roll while it spins and applause when the winner lands. It shows the winner's name, WikiTree ID, team and bib.

**Live page:** https://robinson27225.github.io/WikiTreeMarathonDrawing/

Everything runs in the browser. There is no server to maintain. The only outside pieces are WikiTree (the random tool and the bib images) and an optional Google Apps Script that shares the ineligible list between hosts.

## How a host uses it

The tool has three screens.

1. **Setup**
   - Choose **Test** or **Live** at the top. Test mode shows an amber banner on every screen and saves nothing, so use it to practise. Live mode saves the ineligible list.
   - Enter how many winners to draw.
   - Add any ineligible WikiTree IDs (previous winners who have claimed their prize). Paste IDs one per line or separated by commas, or paste wikitree.com/wiki/... links, then press **Add to list**. Remove an ID with the x on its chip.
   - Under **Draw settings** you can choose where winners come from, turn the drum roll and applause on or off, run a **Sound check**, and set the spin length (3 to 20 seconds).
   - Press **Continue**.
2. **Ready to draw**: press the WikiTree logo to start.
3. **The draw**: the bibs spin and slow down, then land on the winner. Press **Draw next winner** for each additional winner. After the last one, **Show all winners** gives a summary.

A small **Sound on / Sound off** button in the bottom-right corner mutes everything quickly.

## How winners are picked

By default the page asks the WikiTree Random Participant tool for a participant, once per winner, and keeps asking until it gets someone who is on the roster, is eligible, and has not already been drawn in the current draw.

If the tool cannot be reached, the page says so and offers **Entire eligible roster** instead. That picks at random from every eligible person on the roster, including people who have not been active recently, so use it only as a fallback.

## Who is ineligible

A participant cannot win if either of these is true:

- They have anything in the **Note** column of the roster (for example a previous marathon's code, "Team" or "Host").
- A host has added their ID to the ineligible list.

## The shared ineligible list

The ineligible list is shared by every host and tied to the marathon weekend, **Friday 8:00 AM ET to Monday 9:00 AM ET**. It clears itself when the next Friday 8:00 AM ET window starts.

The list is stored in an **Ineligible** tab of the participants Google Sheet, through a small Google Apps Script (`ineligible-sync.gs`). Hosts can also view and edit it directly in that tab. The tab's columns are WikiTree ID, Date (the Friday that starts the weekend) and Timestamp.

If the shared list cannot be reached, the page shows a warning and asks for confirmation before a draw, so nobody draws from a stale list by accident.

If a host code is set in the script, each host types it once, in the box under the ineligible list.

If no shared list is set up, the ineligible list is saved on that one device only.

## Admin view

Add `#admin` to the end of the page address (the page has no link to it). The settings there are saved in the browser you use, not shared between hosts.

- **Random participant tool:** enter the marathon's tool URL (it must start with `https://plus.wikitree.com/`, and should include the challenge name and hours) and test it.
- **Participants:** load the roster from a Google Sheet link, or paste or upload a CSV. Required columns are WikiTreeID, Name, Team and Bib#. Image (a bib image URL) and Note are optional. If you save a sheet link, the page re-reads the roster every time it opens, so edits made during the marathon show up.
- **Ineligible list:** see the weekend window, test or change the shared list connection, or clear the list.

The Admin view is not password-protected. It is just a separate view.

## Setting up for a new marathon

1. Update the participants sheet, including the Note column.
2. Open the page with `#admin` on each host's browser. Under Participants, paste the sheet link (sharing must be "Anyone with the link", or the sheet published to the web as CSV) and press **Load from sheet**. Enter the sheet's tab name if it is not "Participants".
3. Enter the new marathon's Random Participant tool URL and press **Test it**.
4. Check that the setup screen says "Shared with all hosts" and "Connected to the WikiTree random participant tool", then do a practice draw in Test mode.

The roster built into `index.html` is a snapshot (Oct 2, 2026, 602 participants). It is used until a host loads a newer one.

## Setting up the shared list (one time)

1. Open the participants Google Sheet and choose **Extensions, then Apps Script**.
2. Replace the sample code with the contents of `ineligible-sync.gs`. Optionally set `PASSCODE` to a word only your hosts know.
3. Choose **Deploy, then New deployment, then Web app**. Set **Execute as** to **Me** and **Who has access** to **Anyone**, then **Deploy** and authorize it.
4. Copy the **Web app URL**. It is built into `index.html` (the `SYNC_DEFAULT` value), or hosts can paste it in the Admin view.
5. After any later change to the script, use **Deploy, Manage deployments, pencil icon, Version: New version, Deploy**. The URL stays the same.

Set a passcode if you can. This repository and page are public, so without one anyone who finds the web app URL could change the list.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole app in one file, including the roster, WikiTree logo and sounds. This is the only file GitHub Pages needs. |
| `ineligible-sync.gs` | Optional reference copy of the Google Apps Script. It runs inside the Google Sheet and is not served by the page. |
| `README.md` | This file. |

## Notes

- The page and repository are public. `index.html` contains the participant names, WikiTree IDs, teams and bib numbers from the roster.
- Bib images load from wikitree.com and are cached in the background when the page opens.
- Sound only plays after a click, which is a browser rule. The page unlocks it when the host presses Continue and the logo.
- The logo and the audio clips are embedded in `index.html`. Make sure you have permission to publish them.
- Ineligible lists and Admin settings are kept in the browser's saved site data. Clearing it, or using a private window, resets them on that device. The shared list is not affected.

## Troubleshooting

| What you see | What to do |
| --- | --- |
| "Enter the host code to use the shared list" | Type the host code under the ineligible list and press Enter or **Save code**. The page tells you if it is wrong. |
| "Can't reach the shared list" | Check the connection and press **Retry**. The reason is shown in brackets. "Unreadable response" usually means the Apps Script was not deployed with access set to **Anyone**. |
| Tool shows as not reachable | Press **Check again** under Draw settings, or confirm the URL in the Admin view. Use the whole-roster option only as a fallback. |
| A winner's bib does not appear | The roster or the sheet may be missing that person. Update the roster in the Admin view. |
| No sound | Press **Sound check**, make sure the corner button says "Sound on", and check the computer's volume. |
