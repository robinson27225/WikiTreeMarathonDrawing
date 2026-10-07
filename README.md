[README.md](https://github.com/user-attachments/files/33176000/README.md)
# WikiTree Marathon Drawing

A live-drawing tool for WikiTree livecasts. For the Source-a-Thon marathon, the host pulls winners from WikiTree's Random Participant tool, and the page shows a bib slideshow that slows down and lands on each winner. For any other WikiTree event, such as the Connect-a-Thon, it draws from a list of WikiTree IDs or attendee names and shows spinning balls in WikiTree colors instead of bibs. Either way there's a drum roll while it spins and applause when the winner lands.

**Live page:** https://robinson27225.github.io/WikiTreeMarathonDrawing/

Everything runs in the browser. There is no server to maintain. The only outside pieces are WikiTree (the random tool and the bib images) and a Google Apps Script that lives in the participants spreadsheet and shares the ineligible list and participant table between hosts.

## How a host uses it

The tool has three screens.

1. **Setup**
   - In Live mode, enter the **host code** in the box at the top first (see "The host code" below). Nothing can be drawn or changed until it is accepted.
   - Choose **Test** or **Live** at the top. Test mode shows an amber banner on every screen and saves nothing, so use it to practise. Live mode saves the ineligible list.
   - A summary line at the top shows how many participants are eligible and how many are marked ineligible in the sheet.
   - Enter how many winners to draw.
   - Add the **studio hosts** for this draw (see below).
   - Choose **When someone wins** (see below): winners are either added to the ineligible list automatically, or hosts add them by hand after they claim.
   - Add any other ineligible WikiTree IDs by hand. Paste IDs one per line or separated by commas, or paste wikitree.com/wiki/... links, then press **Add to list**. Remove an ID with the x on its chip.
   - Under **Draw settings** you can choose where winners come from, turn the drum roll and applause on or off, run a **Sound check**, and set the spin length (3 to 20 seconds).
   - Press **Continue**.
2. **Ready to draw**: press the WikiTree logo to start.
3. **The draw**: the bibs spin and slow down, then land on the winner. Press **Draw next winner** for each additional winner. After the last one, **Show all winners** gives a summary, **Prize winners table** opens the prize winners screen (below), and **New draw** returns to setup.

A small **Sound on / Sound off** button in the bottom-right corner mutes everything quickly.

## Other events: balls instead of bibs

At the top of the setup screen, **What are we drawing for?** switches between:

- **Source-a-Thon marathon (bibs)**: everything described above, with the participant table and bib images.
- **Other event (balls)**: for the Connect-a-Thon or any other WikiTree event. Instead of bibs, a ball spins and changes color at random through the WikiTree brand palette (`#FCB815`, `#F37C26`, `#E5EECF`, `#E1F0B4`, `#A5D167`, `#25422D`, `#FFEE99`, `#FFE270`, `#FAD158` and `#8FC641`; black is left out because it would vanish on the page). Each ball carries one entry from the event's list, slows down with the draw, and lands on the winner. The drum roll, applause, studio hosts, ineligible list, host code, and the prize winners screen all work the same way.

An admin publishes the **event list** for the event (see Admin view). It is one of two kinds:

1. **Participants' WikiTree IDs.** The balls show WikiTree IDs, and the winner screen shows the person's name when it is known, with their ID linked to their profile. The WikiTree Random Participant tool is available as an option, but the default is the whole list.
2. **Attendee names**, for events with guests who are not on WikiTree. The balls show names, and winners come from the list only. The Random Participant tool is not used for these events and is hidden.

Other things to know:

- Each event name has its **own ineligible list**, shared by all hosts. A new event name starts fresh, and the marathon's weekend list is separate and unaffected. The list stays until an admin clears it.
- With **Add winners automatically** on, winners are added to the event's list as they land, as in the marathon.
- On the prize winners screen, the Event column defaults to the event name. Attendees without a WikiTree ID get an empty ID cell, and WikiTree IDs without a known name get empty First and Last cells. Fill those in before you paste.
- The event list is protected by the host code like the ineligible list. A new tab asks for the code before it shows the list.

## After the draw: the prize winners screen

When the draw is finished, press **Prize winners table**. It opens a screen with:

- A link to the WikiTree **Prize Winners** page: https://www.wikitree.com/wiki/Space:Prize_Winners
- **WikiTree markup** for this draw's winners, ready to copy with one button. Each winner is a table row in the format of the Current Event Winners table (WikiTree ID, First, Last, Event):

  ```
  |-
  |Zurcher-160||Randi||Zurcher||SaTXI
  ```

- **Rows to add** (the default) gives just this draw's rows. Paste them into the existing table on the Prize Winners page, just above the last line (the `|}` that closes the table).
- **Whole table** gives the complete Current Event Winners section: **everyone on this event's ineligible list, in the order they were added, plus this draw's winners**. The ineligible list is where winners are recorded, so this includes winners from earlier draws and from other hosts' screens. The page re-reads the shared list when the prize screen opens, so it is up to date. It also includes anyone a host added to the list by hand, so remove any entries that weren't winners before you copy.
- **Already have the table on WikiTree?** WikiTree doesn't let the page read its own page, so the tool can't see rows that were added before it started recording winners. Open **Already have the table on WikiTree? Paste it to add to it** under the buttons and paste the whole Current Event Winners section from the edit box. The tool keeps your rows exactly as they are and adds only the winners that aren't in it yet, just before the closing `|}`.
- An **Event name** box (it starts as SaTXI and is remembered in the browser) fills the Event column. Change it for each marathon.
- The markup box is editable. First and Last are split from the participant's name automatically, keeping prefixes such as "van der" with the surname. Check them, and fix any by hand, before copying.

**Back to the first screen** returns to setup, ready for the next draw, and clears the studio hosts. **Back to the winners** returns to the winner display.

## How winners are picked

By default the page asks the WikiTree Random Participant tool for a participant, once per winner, and keeps asking until it gets someone who is on the roster, is eligible, and has not already been drawn in the current draw.

If the tool cannot be reached, the page says so and offers **Entire eligible roster** instead. That picks at random from every eligible person on the roster, including people who have not been active recently, so use it only as a fallback.

## Who is ineligible

A participant cannot win if any of these is true:

- They have anything in the **Note** column of the participant table. That column is only for members who won in the previous marathon or who are WikiTree team members.
- Their ID is on the **ineligible list** for the weekend, either added by a host or added automatically when they won.
- They are one of the **studio hosts** entered for the current draw.

### When someone wins: two ways

A toggle on the setup screen chooses what happens to each winner.

- **Add winners to the ineligible list automatically** (the default). The moment a winner lands, their ID goes on the ineligible list, so they cannot win again this weekend. Nobody has to wait for a claim. In Live mode the ID is saved to the shared list that every host sees; in Test mode it goes on the test list only. If saving to the shared list fails, a short warning appears under the winner so a host can add the ID by hand. If a draw was a mistake, remove the ID with the x on its chip.
- **Hosts add winners after they claim.** The original way. Nothing is added automatically, and hosts add each winner to the ineligible list by hand once they have claimed their prize.

The choice is saved in each host's browser, so make sure everyone drawing uses the same one.

### Studio hosts (this draw only)

The hosts in the studio cannot win the current draw. Enter their WikiTree IDs in **Studio hosts** on the setup screen. This list is for the current draw only: it is not saved, not shared with other hosts, and it clears when you press **New draw**. Enter them again for the next draw.

## The shared ineligible list

The ineligible list is shared by every host and tied to the marathon weekend, **Friday 8:00 AM ET to Monday 9:00 AM ET**. It clears itself when the next Friday 8:00 AM ET window starts.

The list is stored in an **Ineligible** tab of the participants Google Sheet, through the Google Apps Script (`ineligible-sync.gs`). Hosts can also view and edit it directly in that tab. The tab's columns are WikiTree ID, Date (the Friday that starts the weekend) and Timestamp.

If the shared list cannot be reached, the page shows a warning and asks for confirmation before a draw, so nobody draws from a stale list by accident.

### The host code

If a **host code** is set in the script, every host must enter it before using Live mode. A box at the top of the setup screen asks for it.

- Until the code is accepted, Live mode is **locked**: the ineligible list is hidden, IDs cannot be added or removed, and **Continue** and the draw are blocked. There is no "continue anyway".
- The code is checked by the script, so even a modified page cannot change the list without it.
- The code is remembered **until the browser tab is closed**. A reload in the same tab keeps it, but opening the page in a new tab, or after closing the tab, asks for it again. It is never saved in the browser's long-term storage. If the script stops accepting a remembered code (for example, after you change it), the page forgets it and asks again. Any code or list saved on a device by an older version is removed.
- If the code stops being accepted, or the shared list cannot be reached, the draw is stopped before anyone is drawn.
- Test mode does not need the code. It never reads or changes the shared list.
- The page can only require a code that the script requires. If `PASSCODE` in the script is empty, nothing is locked.

## Two codes

The script uses two different passcodes. They must not be the same.

| Code | Who has it | What it allows |
| --- | --- | --- |
| **Host code** (`PASSCODE`) | Everyone drawing | Read and edit the ineligible list (including the winners added automatically), and load the participant table and event list. Remembered only until the tab is closed. |
| **Admin code** (`ADMIN_PASSCODE`) | Only the person managing the participant table | Everything above, plus publishing or removing the participant table or the event list, and clearing the whole ineligible list. |

Both codes are checked by the Google script, not by the page, so neither appears in the page's source. Admin actions are refused until an admin code is set.

## The participant table

The table lives in the participants Google Sheet (the tab with WikiTreeID, Name, Team, Bib#, Image and Note columns). Hosts do not read that tab directly. An admin **publishes** it, which copies it to a **Roster** tab that every host's page reads. Each page checks for a newer table every 15 seconds, and also when it opens. If nothing has been published, pages use the table built into `index.html`.

So a change to the participant table reaches the hosts only when someone with the admin code publishes it.

## Admin view

Add `#admin` to the end of the page address (the page has no link to it). It asks for the admin code before showing anything, and it locks again when you leave the view or press **Lock admin**. The code is held only in memory.

- **Participant table:** press **Publish from the spreadsheet** to publish the participants tab of the sheet (it finds the tab automatically, or type its name). Or paste or upload a CSV, check the summary, and press **Publish to all hosts**. **Back to the built-in table for everyone** removes the published table.
- **Event list (other events):** enter the event name, choose whether the list holds WikiTree IDs or attendee names, then paste or upload the list (one entry per line) and press **Read this list**. Check the summary and press **Publish to all hosts**. For WikiTree IDs, you can add a name after a comma (`Smith-123, Jane Smith`). For IDs that come without a name, the script looks the names up on WikiTree when you publish, as a best effort. If WikiTree is slow or limits requests, the list is still published with IDs only. **Remove the event list for everyone** takes it down.
- **Random participant tool:** enter the marathon's tool URL (it must start with `https://plus.wikitree.com/`, and should include the challenge name and hours) and test it. This setting is saved in the browser you use.
- **Ineligible list:** see the weekend window, test or change the shared list connection, or clear the list for everyone.

## Setting up a new event (balls)

1. Open the page with `#admin`, enter the admin code, and find **Event list (other events)**.
2. Enter the event name, choose **WikiTree IDs** or **Attendee names**, paste the list, press **Read this list**, then **Publish to all hosts**.
3. On the setup screen, choose **Other event (balls)**. Check that the line under it names your event and shows the right count, then do a practice draw in Test mode.
4. When the next event comes, publish a new list with a new event name. The old list's ineligible entries stay on the sheet and do not carry over.

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
   The script can look up names on WikiTree when you publish an ID list, so Google asks you to approve a permission to "connect to an external service". That is only used for that lookup.
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
| "Enter the host code to use Live mode" | Type the host code in the box at the top of the setup screen and press Enter or **Unlock**. The page tells you if it is wrong. It is remembered until the tab is closed, and has to be entered again in a new tab. |
| "The draw was stopped because the shared list isn't unlocked" | The code was rejected or the shared list could not be reached when the draw started. Nothing was drawn. Enter the code again, or press **Retry**, and start again. |
| "Can't reach the shared list" | Check the connection and press **Retry**. The reason is shown in brackets. "Unreadable response" usually means the Apps Script was not deployed with access set to **Anyone**. |
| Admin says "No admin passcode has been set" | Set `ADMIN_PASSCODE` in the script and deploy a new version. |
| Admin says the admin passcode must differ from the host code | Change one of the two codes in the script and deploy a new version. |
| "That passcode isn't right" in Admin | The admin code is case-sensitive. Check it with whoever set up the script. |
| The participant table did not change on a host's screen | Press **Refresh roster** at the bottom of the setup screen, or wait about 15 seconds. Make sure someone published it from the Admin view. |
| "No event list has been published yet" | Choose **Other event** only after an admin has published a list in the Admin view. Press **Refresh roster** at the bottom if it was just published. |
| Tool shows as not reachable | Press **Check again** under Draw settings, or confirm the URL in the Admin view. Use the whole-roster option only as a fallback. |
| A winner's bib does not appear | The participant table may be missing that person. Update the sheet and publish it again. |
| No sound | Press **Sound check**, make sure the corner button says "Sound on", and check the computer's volume. |
