# New Discord Badge - Completed a Quest

## Badge?
![badgee](https://cdn.discordapp.com/attachments/1189022662266208386/1233418932061470861/image.png?ex=662d0665&is=662bb4e5&hm=4d61b4cc47596d43b54e02ab3a543e77dd81654a50bcdca0369a6d66b2a7d2fe&)


1.  Go to User Settings -> Gift Inventory and accept the quest.
    
2.  Join a voice channel.
    
3.  Start streaming any window, such as Notepad.
    
4.  Open the Developer Tools:
    
    *   If using the Discord desktop app, press Ctrl+Shift+I. If the Developer Tools don't open, try using Discord in a web browser instead.
    *   If using Discord in a web browser:
        *   For Chrome or Firefox, press F12 or Ctrl+Shift+I
        *   For Safari, go to Preferences -> Advanced and check "Show Develop menu in menu bar". Then, click Develop -> Show Web Inspector.
5.  Navigate to the `Console` tab.
    
6.  Type `allow pasting` to enable pasting in the console.
    
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

8.  Keep the stream running for 15 minutes.
    
9.  Once the quest is completed, you can claim the reward by going to User Settings -> Gift Inventory.
    

Monitoring Progress:
--------------------

To track the progress of the quest:

*   Check the `Quest progress:` messages in the Console tab.
*   Reopen the Gift Inventory tab in User Settings to see the updated progress.

The progress should update every 30 seconds.

Note: You don't need any viewers watching your stream for this script to work. You can be alone in the voice channel.

<p align="center">
  <a href="https://discord.com/users/1188569749068718234" target="_blank"><img src="https://lanyard.cnrad.dev/api/1188569749068718234?hideActivity=true" alt="Discord Presence" style="max-width: 100%;"></a>
</p>
