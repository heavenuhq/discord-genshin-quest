# discord-genshin-quest
> [!NOTE]
> This no longer works in browser! (only dekstop) / ne fonctionne plus sur le navigateur (fonctionne uniquement sur l'app discord pc)

> [!NOTE]
> This no longer works if you're alone in vc! Somebody else has to join you! (vous devez pas etre seul en voc une personne dois etre avec vous! (fonctionne aussi avec un dc)
>
How to use this script:
1. Accept the quest under User Settings -> Gift Inventory
2. Join a vc
3. **Join the same vc on an alt**
4. Stream any window (can be notepad or something)
5. Press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>I</kbd> to open DevTools
6. Go to the `Console` tab
7. Paste the following code and hit enter:
```js
let wpRequire;
window.webpackChunkdiscord_app.push([[ Math.random() ], {}, (req) => { wpRequire = req; }]);

let ApplicationStreamingStore = Object.values(wpRequire.c).find(x => x?.exports?.default?.getStreamerActiveStreamMetadata).exports.default;
let QuestsStore = Object.values(wpRequire.c).find(x => x?.exports?.default?.getQuest).exports.default;
let FluxDispatcher = Object.values(wpRequire.c).find(x => x?.exports?.default?.flushWaitQueue).exports.default;

let quest = [...QuestsStore.quests.values()].find(x => x.userStatus?.enrolledAt && !x.userStatus?.completedAt && new Date(x.config.expiresAt).getTime() > Date.now())
let isApp = navigator.userAgent.includes("Electron/")
if(!isApp) {
	console.log("This no longer works in browser. Use the desktop app!")
} else if(!quest) {
	console.log("You don't have any uncompleted quests!")
} else {
	let pid = Math.floor(Math.random() * 30000) + 1000
	ApplicationStreamingStore.getStreamerActiveStreamMetadata = () => ({
		id: quest.config.applicationId,
		pid,
		sourceName: null
	})
	
	let secondsNeeded = quest.config.streamDurationRequirementMinutes * 60
	let fn = data => {
		let progress = data.userStatus.streamProgressSeconds
		console.log(`Quest progress: ${progress}/${secondsNeeded}`)
		
		if(progress >= secondsNeeded) {
			console.log("Quest completed!")
			FluxDispatcher.unsubscribe("QUESTS_SEND_HEARTBEAT_SUCCESS", fn)
		}
	}
	FluxDispatcher.subscribe("QUESTS_SEND_HEARTBEAT_SUCCESS", fn)
	
	console.log(`Spoofed your stream to ${quest.config.applicationName}. Stay in vc for ${Math.ceil(quest.config.streamDurationRequirementMinutes - (quest.userStatus?.streamProgressSeconds ?? 0) / 60)} more minutes.`)
	console.log("Remember that you need at least 1 other person to be in the vc!")
}
```
7. Keep the stream running for 15 minutes
8. You can now claim the reward in User Settings -> Gift Inventory!

You can track the progress by looking at the `Quest progress:` prints in the Console tab, or by reopening the Gift Inventory tab in settings. The progress should update every 30s.

## FAQ

**Q: Ctrl + Shift + I doesn't work**

A: Either download the [ptb client](https://discord.com/api/downloads/distributions/app/installers/latest?channel=ptb&platform=win&arch=x64), or use [this](https://www.reddit.com/r/discordapp/comments/sc61n3/comment/hu4fw5x/) to enable DevTools on stable


**Q: I get an error saying "Unauthorized"**

A: Discord has patched the script from working in browsers. Use the desktop app, or alternatively find some extension which lets you change your User-Agent and append the string `Electron/` anywhere in it.

They have also started checking how many people are in the vc, so make sure you join it on at least 1 other account.


**Q: I get a different error**

A: Make sure you've started streaming *before* running the script. Also make sure you're copy/pasting it correctly.

Comment utiliser ce script :
1. Acceptez la quête sous Paramètres utilisateur -> Inventaire des cadeaux
2. Joindre un vc
3. **Joindre le même vc sur un alt**
4. Diffuser n’importe quelle fenêtre (peut être un bloc-notes ou quelque chose)
5. Appuyez sur <kbd>Ctrl</kbd>+<kbd>Maj</kbd>+<kbd>I</kbd> pour ouvrir DevTools
6. Allez à l’onglet « Console ».
7. Collez le code suivant et appuyez sur Entrée :
```js
let wpRequire;
window.webpackChunkdiscord_app.push([[ Math.random() ], {}, (req) => { wpRequire = req; }]);

let ApplicationStreamingStore = Object.values(wpRequire.c).find(x => x?.exports?.default?.getStreamerActiveStreamMetadata).exports.default;
let QuestsStore = Object.values(wpRequire.c).find(x => x?.exports?.default?.getQuest).exports.default;
let FluxDispatcher = Object.values(wpRequire.c).find(x => x?.exports?.default?.flushWaitQueue).exports.default;

let quest = [...QuestsStore.quests.values()].find(x => x.userStatus?.enrolledAt && !x.userStatus?.completedAt && new Date(x.config.expiresAt).getTime() > Date.now())
let isApp = navigator.userAgent.includes("Electron/")
if(!isApp) {
	console.log("This no longer works in browser. Use the desktop app!")
} else if(!quest) {
	console.log("You don't have any uncompleted quests!")
} else {
	let pid = Math.floor(Math.random() * 30000) + 1000
	ApplicationStreamingStore.getStreamerActiveStreamMetadata = () => ({
		id: quest.config.applicationId,
		pid,
		sourceName: null
	})
	
	let secondsNeeded = quest.config.streamDurationRequirementMinutes * 60
	let fn = data => {
		let progress = data.userStatus.streamProgressSeconds
		console.log(`Quest progress: ${progress}/${secondsNeeded}`)
		
		if(progress >= secondsNeeded) {
			console.log("Quest completed!")
			FluxDispatcher.unsubscribe("QUESTS_SEND_HEARTBEAT_SUCCESS", fn)
		}
	}
	FluxDispatcher.subscribe("QUESTS_SEND_HEARTBEAT_SUCCESS", fn)
	
	console.log(`Spoofed your stream to ${quest.config.applicationName}. Stay in vc for ${Math.ceil(quest.config.streamDurationRequirementMinutes - (quest.userStatus?.streamProgressSeconds ?? 0) / 60)} more minutes.`)
	console.log("Remember that you need at least 1 other person to be in the vc!")
}
```
7. Gardez le courant pendant 15 minutes
8. Vous pouvez maintenant réclamer la récompense dans Paramètres utilisateur -> Inventaire des cadeaux!

Vous pouvez suivre la progression en consultant les impressions « Progression de la quête » dans l’onglet Console ou en rouvrant l’onglet Inventaire des cadeaux dans les paramètres. La progression devrait être mise à jour toutes les 30 secondes.

## FAQ

**Q : Ctrl + Maj + I ne fonctionne pas**

R : Téléchargez le [client ptb](https://discord.com/api/downloads/distributions/app/installers/latest?channel=ptb&platform=win&arch=x64) ou utilisez [ceci](https://www.reddit.com/r/discordapp/comments/sc61n3/comment/hu4fw5x/) pour activer DevTools sur stable.


**Q : Je reçois une erreur indiquant « Non autorisé »**

R : Discord a corrigé le script pour qu’il ne fonctionne plus dans les navigateurs. Utilisez l’application de bureau, ou trouvez une extension qui vous permet de changer votre User-Agent et ajoutez la chaîne `Electron/' n’importe où.

Ils ont également commencé à vérifier combien de personnes sont dans le vc, alors assurez-vous de le rejoindre sur au moins 1 autre compte.


**Q : Je reçois une erreur différente**

R : Assurez-vous que vous avez commencé à diffuser *avant* d’exécuter le script. Assurez-vous également que vous le copiez/collez correctement.
