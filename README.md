# Badges System
Welcome to the badges problem Guide.
I can help you to remove by every badge the spaces with better-sqlite3.
If you have questions about the sqlite databases. DM me in Discord with the name `Pluide#1234`.

## Installation:
```
npm i better-sqlite3 --save
npm i discord.js --save
```
## 1: Showcase -> index.js
First I create a database in the main file.
For me is `index.js`. You can name it what you want.
```js
const sqlite = require('better-sqlite3')
const discord = require('discord.js')
const client = new discord.Client()

const p = new sqlite("./profile.sqlite")

client.on('ready', async() => {
   
   const table1 = p.prepare("SELECT count(*) FROM sqlite_master WHERE type='table' AND name = 'Profile';").get();
    if (!table1['count(*)']) {
      p.prepare("CREATE TABLE Profile (id TEXT PRIMARY KEY, user TEXT, founder TEXT, developer TEXT, staff TEXT);").run();
      p.prepare("CREATE UNIQUE INDEX idx_Profile_id ON Profile (id);").run();
      p.pragma("journal_mode = wal");
    }
    
    client.getProfile = p.prepare("SELECT * FROM Profile WHERE user = ?");
    client.setProfile = p.prepare("INSERT OR REPLACE INTO Profile (id, user, founder, developer, staff) VALUES (@id, @founder, @developer, @staff);");

})
```
If you can create databases, just skip that part.
## 2: Showcase -> message.js
```js
const discord = require('discord.js')
var p = client.getProfile.get(message.author.id)

if(!p) {
  p = { id: `${message.author.id}`, user: `${message.author.id}`, founder: `false`, developer: `false`, staff: `false` }
}
client.setProfile.run(p)
```
If you can create databases, just skip that part.
## 3: Showcase -> profile.js
If you want to make badges in other command just do this, I will use in `profile.js`.
```js
const { MessageEmbed } = require("discord.js")
var p = client.getProfile.get(message.author.id)

var ubadges = "";

if (p.founder == "true") ubadges = ubadges + ` <:EMOJI_NAME:EMOJI_ID>`;
if (p.developer == "true") ubadges = ubadges + ` <:EMOJI_NAME:EMOJI_ID>`;
if (p.staff == "true") ubadges = ubadges + ` <:EMOJI_NAME:EMOJI_ID>`;
if (ubadges == "" || ubadges == undefined || ubadges == "undefined") {
  ubadges = "`Not found`"; // You can rename this, if you want a other error if the user doesn't have badges.
}

// Embed Part
const embed = new MessageEmbed()
.setTitle(`**Your Badges:** ${ubadges}`)

message.channel.send(embed)
```
