# `Transcripts 4 Discord.JS` 

[![Discord](https://img.shields.io/discord/555474311637499955?label=discord)](https://discord.gg/aG6fENXT)
![GitHub package.json version](https://img.shields.io/github/package-json/v/DimSystems/T4DJ)
![GitHub Repo stars](https://img.shields.io/github/stars/DimSystems/T4DJ?style=social)

Transcripts 4 Discord.JS is a highly modified version of the Discord-Html-Transcripts by derock with translations, more customaization and more. MOST components are originally made by derock and please give kudos to him

This module can format the following:
- Languages (English & Brazilian [Languange information is below])
- Discord flavored markdown
  - Uses [discord-markdown-parser](https://github.com/ItzDerock/discord-markdown-parser)
  - Allows for complex markdown syntax to be parsed properly
- Embeds
- System messages
  - Join messages
  - Message Pins
  - Boost messages
- Slash commands
  - Will show the name of the command in the same style as Discord
- Buttons
- Reactions
- Attachments
  - Images, videos, audio, and generic files
- Replies
- Mentions
- Threads
- Stickers
- Improved Saving Systems
- Good customaization

**This module is designed to work with the latest version of [discord.js](https://discord.js.org/#/) _only_. If you need v13 support, roll back to v2.X.X**

Styles from [@derockdev/discord-components](https://github.com/ItzDerock/discord-components).  
Behind the scenes, this package uses React SSR to generate a static site.

## 👋 Support

Please do not DM me requesting support with this package, I will not respond.  
Instead, please open a thread in thie help channel joining [this](https://discord.gg/p3npbXQHSw) discord server.

**This module uses a completely new CSS system and adds new features, there are ways to change back the CSS to the original version**

Styles from [@derockdev/discord-components](https://github.com/ItzDerock/discord-components).  
Behind the scenes, this package uses React SSR to generate a static site.

## Old CSS Example Output [Note: Does not use this module.]

![output](https://derock.media/r/6G6FIl.gif)

## Comparison of OLD Css & _New_ Css

### Old

![output](https://mdps.xyz/assets/Screenshot_2023-09-02_at_02.42.43.png)

### New

![output](https://mdps.xyz/assets/Screenshot_2023-09-02_at_02.42.09.png)

## 📝 Usage

### Example usage using the built in message fetcher.

```js
const discordTranscripts = require('T4DJ');
// or (if using typescript) import * as discordTranscripts from 'T4DJ';

const channel = message.channel; // or however you get your TextChannel

// Must be awaited
const attachment = await discordTranscripts.createTranscript(channel);

channel.send({
  files: [attachment],
});
```

### Or if you prefer, you can pass in your own messages.

```js
const discordTranscripts = require('T4DJ');
// or (if using typescript) import * as discordTranscripts from 'T4DJ';

const messages = someWayToGetMessages(); // Must be Collection<string, Message> or Message[]
const channel = someWayToGetChannel(); // Used for ticket name, guild icon, and guild name

// Must be awaited
const attachment = await discordTranscripts.generateFromMessages(messages, channel);

channel.send({
  files: [attachment],
});
```

## ⚙️ Configuration

Both methods of generating a transcript allow for an option object as the last parameter.  
**All configuration options are optional!**

### Built in Message Fetcher

```js
const attachment = await discordTranscripts.createTranscript(channel, {
    limit: -1, // Max amount of messages to fetch. `-1` recursively fetches.
    returnType: 'attachment', // Valid options: 'buffer' | 'string' | 'attachment' Default: 'attachment' OR use the enum ExportReturnType
    filename: 'transcript.html', // Only valid with returnType is 'attachment'. Name of attachment.
   // Download all images and include the image data in the HTML (allows viewing the image even after it has been deleted) (! WILL INCREASE FILE SIZE USE IF NECESSCARY !)
    footerText: "Exported {number} message{s}", // Change text at footer, don't forget to put {number} to show how much messages got exported, and {s} for plural
    callbacks: {
      // register custom callbacks for the following:
      resolveChannel: (channelId: string) => Awaitable<Channel | null>,
      resolveUser: (userId: string) => Awaitable<User | null>,
      resolveRole: (roleId: string) => Awaitable<Role | null>
    },
    FileConfig: {
      SaveAttachments: false, // Saves image attachments in channel
      SaveExternalEmojis: false, // Saves all external emojis in channel
      SaveStickers: false // Saves all stickers send in channnel
    },
    callbacks: {
      resolveChannel: async (id) => channel.client.channels.fetch(id).catch(() => null),
      resolveUser: async (id) => channel.client.users.fetch(id).catch(() => null),
      resolveRole: channel.isDMBased() ? () => null : async (id) => channel.guild?.roles.fetch(id).catch(() => null),
    },
    customCSS: {  // FOR Users who want to customaize their css alot, needs good css experience
      GlobalCSS: {
        BackgroundColor: "Black", 
        Color: "White",
      },
      MessagesCSS1: {
       Color: "#afafaf",
       BackgroundColor:  "#1a1818",
       Display: "block",
       FontSize: "16px",
       FontFamily: `Whitney, 'Source Sans Pro', ui-sans-serif, system-ui, -apple-system, 'system-ui', 'Segoe UI', Roboto, 'Helvetica Neue', Arial,
       sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji'`,
       LineHeight: "170%",
       Border: "5px solid rgba(255, 255, 255, 0.05);"
      }, 
      MessagesCSS2: {
        Color:  "#afafaf",
        Display: "flex",
        FontSize: "0.9em",
        FontFamily: `Whitney, 'Source Sans Pro', ui-sans-serif, system-ui, -apple-system, 'system-ui', 'Segoe UI', Roboto, 'Helvetica Neue', Arial,
        sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji'`,
        Padding: "0px 1em",
        Position: "relative",
        WordWrap: "break-word",
        Flex: "0 0 auto",
        MinHeight: "1.375rem",
        PaddingRight: "48px !important;",
        MarginTop: "1.0625rem"
      }, 
      MessageReplyCSS: {
        Color: "#b9bbbe",
        Display: "flex",
        FontSize: "0.875rem",
        FontFamily: `Whitney, 'Source Sans Pro', ui-sans-serif, system-ui, -apple-system, 'system-ui', 'Segoe UI', Roboto, 'Helvetica Neue', Arial,
        sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji'`,
        PaddingTop: "2px",
        MarginLeft: "56px",
        MarginBottom: "4px",
        AlignItems: "center",
        Position: "relative",
        WhiteSpace: "pre",
        UserSelect: "none"
      },
    },
    Language: "English" // Any compatiable languages. You can check below for compatiable or upcoming translations
    poweredBy: true, // Whether to include the "Powered by T4DJ" footer
    useNewCSS: true, // Whether to use the New CSS or old, although if you are going for a realistic look to discord, use old.
    headerText: "Yay! I love my messages! | {date}", // Show a string on top of the transcript. Optional
    headerColor: "green" // Your choice of color for that specific string. Remember this color is for CSS. Which means whatever color system compatible with CSS can be used. Optional | Default is green
    hydrate: false // Make most of the components compaitable offline unless its a profile picture, emoji etc.
});
```

### Providing your own messages

```js
const attachment = await discordTranscripts.generateFromMessages(messages, channel, {
  // Same as createTranscript, except no limit
});
```

### Compatiable Languages
| Language    | Information |   Translator   |
| -------- | ------- | --------   |
| English  | There by default. Developers speak and write english    |                       |
| Brazilian | Available and optional for the brazilians     | Oreczx [Github](https://github.com/OreczXOfficial)  [Website](https://oreczxdev.xyz/)      |
| Dutch         |     Available and optional for those who speak dutch            |  Percent [Github](https://)                                        |
| French    | Coming soon eventually    |      Looking...                |
| Spanish    | Coming soon eventually    |      Looking...                |

## 🤝 Enjoy the package?

Give it a star! ⭐ 

## More features coming soon:
- Advanced Custom CSS
- More languages
- File Saving System
