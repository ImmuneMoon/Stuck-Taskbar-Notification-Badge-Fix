# 🔴 Stuck Taskbar Notification Badge Fix (Windows 10 & 11)

## The Problem
An app's taskbar icon (Discord, Teams, Slack, and so on) shows a red unread badge, but there is nothing unread in the app. When a real message arrives, the badge counts up from the stuck number: one ghost unread plus one real one shows as **2**.

## How the Badge Actually Works
Windows does not track your unread messages. The app draws the badge itself and hands it to the taskbar, and the taskbar shows whatever it was given last. When the app changes its count, it sends a new badge. When the app quits, the badge should go away.

So a stuck badge comes from one of two places:
* **The taskbar** is holding an old badge the app never cleared. This is the most common cause, and the easiest to fix.
* **The app** thinks something is still unread, so it keeps sending a badge.

You can't reliably tell these apart by watching the number. A stale taskbar badge can still appear to count up when a new message arrives (stuck at 1, then showing 2), so don't take that as proof the app is the cause.

## Try This First: Restart Windows Explorer
This takes about 10 seconds, is harmless, and fixes most stale badges. It rebuilds the taskbar from scratch.

1. Press `Ctrl + Shift + Esc` to open Task Manager.
2. On the **Processes** tab, find **Windows Explorer**.
3. Right-click it and choose **Restart**. The taskbar will disappear for a moment and reload.

If the badge is gone and stays gone, you're done. If it comes back, go to **Step 1**.

> **Real example:** A Discord user on Windows 10 had a badge stuck at 1, and a new message changed it to 2. Discord showed nothing unread: no count on the Discord logo, no server badges, no friend requests. Marking the server read, clearing Discord's cache, and quitting and relaunching Discord all did nothing. Restarting Windows Explorer cleared it.

## Step 1: Find Out Which One You Have
If restarting Explorer didn't fix it, run these checks:

1. **Look for anything unread inside the app.** Check the app's own unread markers (in Discord: the red count on the Discord logo at top-left, and the badges on server icons).
   * If you can find unread items, work through **Phase 1**.
   * If the app shows **nothing unread anywhere**, go to **Phase 1B**.
2. **Fully quit the app** from the system tray (bottom-right, near the clock). Right-click its icon and choose **Quit** (not just closing the window).
   * If the badge **stays on the taskbar with the app closed**, the taskbar is stuck. Go to **Phase 2**.
   * If the badge **disappears** and comes back after you relaunch the app, the app is sending it. Use the result of check 1 to choose **Phase 1** or **Phase 1B**.

> **Hint only:** If the number counts up from the stuck value when a new message arrives, the app *may* be counting something hidden. But a stale taskbar badge can do this too, so use the checks above to decide.

## Phase 1: Find the Hidden Unread Item (App-Side)
Use this when the app shows unread markers you haven't cleared. The unread item exists, but it is somewhere you don't normally look.

### Discord
Check these places, in this order:

1. **Pending friend requests.** Click the Discord logo (top-left) > **Friends** > **Pending**. Accept or ignore anything there. Pending requests can hold a badge even when every channel is read.
2. **Message Requests.** On the same Direct Messages screen, open **Message Requests** (DMs from people who aren't on your friends list). Accept or ignore anything there.
3. **Closed or hidden DMs.** Scroll through the direct messages list, and check the top of the server bar for a DM icon with a red count.
4. **Muted servers and folders.** @mentions in muted servers still count toward the badge. Right-click **each server icon** (or a server folder) and choose **Mark As Read**. Pay attention to servers you've muted and forgotten about.
5. **Threads and forum posts.** A mention inside a thread or forum post can stay unread after the parent channel is read. Open any thread with a red marker and read it.
6. **The Inbox.** Click the inbox icon (top-right of Discord) and check the **Mentions** and **Unreads** tabs. Clear anything listed.
7. **Reload the client.** Press `Ctrl + R` with Discord focused. This reloads the app and re-syncs unread state with Discord's servers. It helps when a message was deleted before you saw it and the counter never went back down.
8. **Clear Discord's cache.**
   1. Quit Discord from the system tray (right-click > **Quit Discord**).
   2. Press `Windows Key + R`, type `%appdata%\discord`, and press **Enter**.
   3. Delete the folders named `Cache`, `Code Cache`, and `GPUCache`.
   4. Relaunch Discord. It rebuilds these folders on its own. Your login and settings are not affected.
9. **Toggle the badge setting.** Go to **User Settings** (gear icon) > **Notifications**, turn **Enable Unread Message Badge** off, then back on. This forces Discord to redraw the badge. If nothing else works, you can leave it off. You will lose the badge but keep your normal notifications.
10. **Log out and back in.** Go to **User Settings** > **Log Out**, then sign back in. This drops Discord's local session and pulls everything fresh from Discord's servers. It's a quicker test than reinstalling: if logging out doesn't clear the badge, a reinstall won't either.
11. **Clean reinstall (last resort).** A normal uninstall leaves Discord's data folders behind, so a plain reinstall picks up the same local state. To reset it properly:
    1. Quit Discord from the system tray, then uninstall it from **Settings** > **Apps**.
    2. Press `Windows Key + R`, type `%appdata%\discord`, press **Enter**, and delete the contents of that folder.
    3. Do the same for `%localappdata%\Discord`.
    4. Download Discord from discord.com, install it, and sign in.
    * A reinstall only resets what's on your PC. If the ghost count is coming from your account (see **Phase 1B**), it will come back after you sign in.

> **Keyboard shortcut note:** In Discord, `Esc` marks the **current channel** as read and `Shift + Esc` marks the **current server** as read. Neither one clears every server, friend request, or DM, so they won't reach an unread item somewhere else.

### Other Apps (Teams, Slack, Signal, WhatsApp, etc.)
The same idea applies. The app is counting something you can't see.

1. Look for a **Mark all as read** option, and check activity feeds, mention lists, invites, and requests.
2. **Reload the app.** Many chat apps are built on the same framework as Discord, and `Ctrl + R` reloads them too (Slack, for example). If that doesn't work, fully quit the app from the system tray and relaunch it.
3. **Clear its cache.** Quit the app, then press `Windows Key + R` and open `%appdata%`. Find the folder named after the app and delete its `Cache`-type folders (not the whole app folder, which may sign you out or reset your settings). If the app isn't there, check `%localappdata%`.
4. **Sign out and back in** to force a full re-sync with the service.
5. **Clean reinstall (last resort).** Uninstall the app, then delete its leftover folders in `%appdata%` and `%localappdata%` before installing again. A plain uninstall usually leaves them behind. Like signing out, this only resets what's on your PC.

## Phase 1B: When the App Shows Nothing Unread (Out-of-Sync Counter)
> **Before you start:** Make sure you've restarted Windows Explorer (see **Try This First**). A stuck taskbar badge looks exactly like this problem, and it's a much quicker fix.

Use this phase when the app looks completely clear, you've already restarted Explorer, and the badge keeps coming back. The app gets its unread count from the service's servers, so the ghost item is tied to your **account**, not your PC. That's why clearing the cache and restarting Explorer don't help.

Common reasons this happens: a message that mentioned you was deleted before you saw it, or you lost access to a channel that still had an unread mention in it. The counter goes up but never comes back down.

> **Reinstalling won't fix this.** A reinstall or cache clear only resets what's on your PC. When you sign back in, the app downloads the same unread count from its servers. Logging out and back in is a quick way to confirm this: if the badge survives that, it's account-side.

1. **Toggle the app's badge setting off and on.** In Discord: **User Settings** > **Notifications** > **Enable Unread Message Badge**. This makes the app recalculate the badge and send it again.
2. **Reload the app** with `Ctrl + R`.
3. **Check the account from another device.** Open the service in a web browser (for Discord, `discord.com/app`) and look at the browser tab title for an unread count. Check the phone app too.
   * If the count shows up there as well, that confirms it's account-side.
   * The browser or phone version sometimes shows the unread item the desktop app hides. Open it and read it there.
4. **Check the out-of-the-way places** (Discord):
   * **Message Requests** > **Spam** tab. Requests Discord files as spam are tucked away and easy to miss.
   * **Inbox** (top-right) > the **Unreads** tab as well as **Mentions**.
   * **Hidden channels.** In each server, open the server menu and check **Browse Channels** or **Channels & Roles** for channels you've hidden. Show them for a moment and look for unread markers. (It isn't confirmed that hidden channels hold on to mention counts, but the interface wouldn't show you one there.)
5. **Workaround if nothing turns it up.** Leave the badge setting from step 1 **off**. You lose the taskbar badge but keep your normal notifications and sounds.
6. **Report it.** If the ghost count survives all of this, it may be stuck on the service's end. Contact the app's support (for Discord, support.discord.com) and tell them what you've already tried.

## Phase 2: Clear a Stale Badge (Taskbar-Side)
Use these fixes if the badge stays with the app closed, or stays frozen no matter what the app does.

1. **Restart Windows Explorer** (if you haven't already). This rebuilds the taskbar from scratch.
   1. Press `Ctrl + Shift + Esc` to open Task Manager.
   2. On the **Processes** tab, find **Windows Explorer**.
   3. Right-click it and choose **Restart**. The taskbar will disappear for a moment and reload.
   * **Useful test:** If the badge comes back right after Explorer restarts while the app is running, the app is sending it again. That points back to **Phase 1**.
2. **Turn taskbar badges off and on.**
   * **Windows 10:** **Settings** > **Personalization** > **Taskbar** > **Show badges on taskbar buttons**.
   * **Windows 11:** **Settings** > **Personalization** > **Taskbar** > **Taskbar behaviors** > **Show badges on taskbar apps**.
   * Turn it off, wait a few seconds, and turn it back on.
3. **Unpin and re-pin the app.** Right-click the app's taskbar icon > **Unpin from taskbar**, launch the app, then right-click its icon > **Pin to taskbar**.
4. **Restart the PC.** Use **Restart**, not Shut Down. On default settings, Windows 10 and 11 **Shut Down** uses Fast Startup and doesn't fully reset the session.

## Fixes That Don't Help
* **Deleting a "badges" folder in the app's data folder.** This tip circulates online, but Windows does not keep a cache folder of taskbar badges, and Discord doesn't store its badge in one either. The badge is drawn live each time. If the folder doesn't exist, that's expected.
* **Rebuilding the Windows icon cache (`IconCache.db`).** That cache stores normal app icons, not notification badges, so clearing it won't touch a stuck badge.

## Quick Checklist
- [ ] Restarted Windows Explorer first (**Try This First**)
- [ ] App shows unread items → work through them (**Phase 1**)
- [ ] App shows **nothing unread** and the badge survives an Explorer restart → out-of-sync counter (**Phase 1B**)
- [ ] Badge **stays** with the app fully quit → taskbar-side (**Phase 2**)
- [ ] Discord: checked Pending friend requests, Message Requests, muted servers, threads, and the Inbox
- [ ] Reloaded the app with `Ctrl + R`, then cleared its cache
- [ ] Checked the account in a browser and on a phone; checked Message Requests > Spam and hidden channels
- [ ] Toggled taskbar badges, re-pinned the app, then restarted the PC
