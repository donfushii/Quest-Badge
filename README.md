<p align="center">
	<a href="https://discord.gg/"><img src=".assets/icon.ico" alt="Imperium" height="90" /></a>
</p>

<h4 align="center">GENSHIN IMPACT DISCORD BADGE</h4>
<p align="center">
	V4.6  |  Credits to aamiaa.
</p>

<p align="center">
	<a href="#-how-to-use">Instructions</a> •
	<a href="#-important">IMPORTANT</a> •
	<a href="#-faq">FAQ</a> •
	<a href="https://discord.gg/">Discord server</a>
</p>
<br/>

## • How to use

1. Accept the quest under User Settings -> Gift Inventory.
2. Join a VC Channel.
3. Stream any window (Can be notepad, etc).
4. Press <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>I</kbd> to open DevTools.
5. Go to the `Console` TAB.
6. Paste the following code and hit enter:

```js
let wpRequire;
window.webpackChunkdiscord_app.push([[ Math.random() ], {}, (req) => { wpRequire = req; }]);

let api = Object.values(wpRequire.c).find(x => x?.exports?.getAPIBaseURL).exports.HTTP;
let ApplicationStreamingStore = Object.values(wpRequire.c).find(x => x?.exports?.default?.getStreamerActiveStreamMetadata).exports.default;
let QuestsStore = Object.values(wpRequire.c).find(x => x?.exports?.default?.getQuest).exports.default;
let encodeStreamKey = Object.values(wpRequire.c).find(x => x?.exports?.encodeStreamKey).exports.encodeStreamKey;
let sleep = ms => new Promise(resolve => setTimeout(resolve, ms));

let quest = [...QuestsStore.quests.values()].find(x => x.userStatus?.enrolledAt && !x.userStatus?.completedAt)
if(!quest) {
	console.log("You don't have any uncompleted quests!")
} else {
	let streamId = encodeStreamKey(ApplicationStreamingStore.getCurrentUserActiveStream())
	let secondsNeeded = quest.config.streamDurationRequirementMinutes * 60
	let heartbeat = async function() {
		console.log("Completing quest", quest.config.messages.gameTitle, "-", quest.config.messages.questName)
		while(true) {
			let res = await api.post({url: `/quests/${quest.id}/heartbeat`, body: {stream_key: streamId}})
			let progress = res.body.stream_progress_seconds
			
			console.log(`Quest progress: ${progress}/${secondsNeeded}`)
			
			if(progress >= secondsNeeded) break;
			await sleep(30 * 1000)
		}
		
		console.log("Quest completed. Now you can claim your badge!")
	}
	heartbeat()
}
```

7. Keep the stream running for 15 minutes.
8. You can now claim the reward in User Settings -> Gift Inventory!

## • IMPORTANT
- Discord has patched the script from working in browsers. Use the desktop app, or alternatively find some extension which lets you change your User-Agent and append the string `Electron/` anywhere in it
- You can track the progress by looking at the `Quest progress:` prints in the Console tab, or by reopening the Gift Inventory tab in settings. The progress should update every 30s.
- [ You need an alt account watching your stream for this to work. ]

## • FAQ

**Q: `Ctrl + Shift + I` Doesn't work**

A: Press <kbd>Win</kbd> + <kbd>R</kbd>, Type `%appdata%\discord`... Look for the `settings.json` file and open it in notepad.
At the top of the file, under the open curly brace you need to add `"DANGEROUS_ENABLE_DEVTOOLS_ONLY_ENABLE_IF_YOU_KNOW_WHAT_YOURE_DOING": true,`

So you file should be

```js
{
  "DANGEROUS_ENABLE_DEVTOOLS_ONLY_ENABLE_IF_YOU_KNOW_WHAT_YOURE_DOING": true,
  "IS_MAXIMIZED": false,
  "IS_MINIMIZED": false,
}
```

After that, save the file, open your task manager... <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>Escape</kbd> and close Discord.
Open Discord again and you should now be able to press <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>I</kbd>.
