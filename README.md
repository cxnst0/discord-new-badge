# New Discord Badge - Completed a Quest

## Badge?
![dc_quest](https://github.com/cxnst0/discord-new-badge/assets/125211961/483bd0b5-9ae6-4f15-b79c-35a91b83583f)


1. **Install the Discord desktop or PTB app**

2. Go to **User Settings** -> **Gift Inventory** and accept the quest.
    
3. Join a **VC** (Voice Channel) and stream anything, such as Notepad. Also, have one of your friends or alts watch the stream.
    
4. Press `Ctrl + Shift + I` to open the dev tools.

5.  Navigate to the **Console** tab.
    
6.  Type `allow pasting`, Hit **Enter** to enable pasting in the console.
    
7.  Copy and paste the provided code into the console and press Enter:
    

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
		
		console.log("Quest completed!")
	}
	heartbeat()
}
```

8.  Keep the stream running for the time mentioned in the quest or until the quest progress fills out (usually 15 minutes).
    
9.  Once the quest is completed, you can claim the reward by going to **User Settings** -> **Gift Inventory**.
    

Monitoring Progress:
--------------------

To track the progress of the quest:

*   Check the `Quest progress:` messages in the Console tab.
*   Reopen the Gift Inventory tab in User Settings to see the updated progress.

The progress should update every 30 seconds.

<p align="center">
  <a href="https://discord.com/users/1188569749068718234" target="_blank"><img src="https://lanyard.cnrad.dev/api/1188569749068718234?hideActivity=true" alt="Discord Presence" style="max-width: 100%;"></a>
</p>
