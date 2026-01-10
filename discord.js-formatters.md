# 📚 @discord.js/formatters - TEXT FORMATTING DOCUMENTATION

**Package:** `@discordjs/formatters`  
**Version:** v0.4+  
**Purpose:** Utilities for formatting Discord text and mentions

---

## 📦 INSTALLATION

```bash
npm install @discordjs/formatters
```

---

## 🎯 TEXT FORMATTING

### **Bold**
```typescript
import { bold } from '@discordjs/formatters';

bold('text')  // **text**
```

### **Italic**
```typescript
import { italic } from '@discordjs/formatters';

italic('text')  // *text*
```

### **Underline**
```typescript
import { underline } from '@discordjs/formatters';

underline('text')  // __text__
```

### **Strikethrough**
```typescript
import { strikethrough } from '@discordjs/formatters';

strikethrough('text')  // ~~text~~
```

### **Spoiler**
```typescript
import { spoiler } from '@discordjs/formatters';

spoiler('text')  // ||text||
```

### **Code**
```typescript
import { inlineCode, codeBlock } from '@discordjs/formatters';

inlineCode('code')  // `code`

codeBlock('javascript', 'const x = 1;')
// ```javascript
// const x = 1;
// ```
```

### **Quote**
```typescript
import { quote, blockQuote } from '@discordjs/formatters';

quote('line')  // > line

blockQuote('multiple\nlines')
// >>> multiple
// lines
```

---

## 💬 MENTIONS

### **User Mention**
```typescript
import { userMention } from '@discordjs/formatters';

userMention('123456789')  // <@123456789>
```

### **Channel Mention**
```typescript
import { channelMention } from '@discordjs/formatters';

channelMention('123456789')  // <#123456789>
```

### **Role Mention**
```typescript
import { roleMention } from '@discordjs/formatters';

roleMention('123456789')  // <@&123456789>
```

### **Everyone/Here**
```typescript
import { EmbedBuilder } from 'discord.js';

// These are plain strings
'@everyone'  // Mentions everyone
'@here'      // Mentions online members
```

---

## ⏰ TIMESTAMPS

### **Timestamp Formatting**
```typescript
import { time, TimestampStyles } from '@discordjs/formatters';

const timestamp = Math.floor(Date.now() / 1000);

time(timestamp)                              // <t:1704844800>
time(timestamp, TimestampStyles.ShortTime)   // <t:1704844800:t> → 3:20 PM
time(timestamp, TimestampStyles.LongTime)    // <t:1704844800:T> → 3:20:00 PM
time(timestamp, TimestampStyles.ShortDate)   // <t:1704844800:d> → 01/09/2026
time(timestamp, TimestampStyles.LongDate)    // <t:1704844800:D> → January 9, 2026
time(timestamp, TimestampStyles.ShortDateTime) // <t:1704844800:f> → January 9, 2026 3:20 PM
time(timestamp, TimestampStyles.LongDateTime)  // <t:1704844800:F> → Thursday, January 9, 2026 3:20 PM
time(timestamp, TimestampStyles.RelativeTime)  // <t:1704844800:R> → 2 hours ago
```

---

## 🔗 LINKS

### **Hyperlink**
```typescript
import { hyperlink, hideLinkEmbed } from '@discordjs/formatters';

hyperlink('Google', 'https://google.com')  // [Google](https://google.com)

hideLinkEmbed('https://google.com')  // <https://google.com>
```

---

## 📝 HEADERS

```typescript
import { heading, HeadingLevel } from '@discordjs/formatters';

heading('Title', HeadingLevel.One)    // # Title
heading('Subtitle', HeadingLevel.Two)  // ## Subtitle
heading('Section', HeadingLevel.Three) // ### Section
```

---

## 📋 LISTS

### **Unordered List**
```typescript
import { unorderedList } from '@discordjs/formatters';

unorderedList(['Item 1', 'Item 2', 'Item 3'])
// - Item 1
// - Item 2
// - Item 3
```

### **Ordered List**
```typescript
import { orderedList } from '@discordjs/formatters';

orderedList(['First', 'Second', 'Third'])
// 1. First
// 2. Second
// 3. Third
```

---

## 🎨 COMBINING FORMATTERS

```typescript
import { bold, italic, underline, userMention } from '@discordjs/formatters';

// Bold + Italic
bold(italic('text'))  // ***text***

// Bold + Underline
bold(underline('text'))  // __**text**__

// Mention + Bold
`${userMention('123')} ${bold('Welcome!')}`  // <@123> **Welcome!**
```

---

## 💡 PRACTICAL EXAMPLES

### **Welcome Message**
```typescript
const welcomeMsg = `
${bold('Welcome!')} ${userMention(userId)}

Please read ${channelMention(rulesChannel)} before chatting.
Type ${inlineCode('/help')} for commands.

Joined at: ${time(Date.now() / 1000, TimestampStyles.ShortDateTime)}
`;
```

### **Embed with Formatting**
```typescript
import { EmbedBuilder } from 'discord.js';
import { bold, codeBlock, hyperlink } from '@discordjs/formatters';

const embed = new EmbedBuilder()
    .setTitle(bold('Server Stats'))
    .setDescription(`
        ${bold('Members:')} 1,234
        ${bold('Online:')} 567
        
        ${hyperlink('Visit Website', 'https://example.com')}
    `)
    .addFields({
        name: 'Code Example',
        value: codeBlock('javascript', 'console.log("Hello!");')
    });
```

### **Help Command**
```typescript
const help = `
${heading('Bot Commands', HeadingLevel.One)}

${bold('Moderation')}
${unorderedList([
    `${inlineCode('/ban')} - Ban a user`,
    `${inlineCode('/kick')} - Kick a user`,
    `${inlineCode('/mute')} - Mute a user`
])}

${bold('Fun')}
${orderedList([
    `${inlineCode('/meme')} - Get a random meme`,
    `${inlineCode('/joke')} - Tell a joke`
])}

${spoiler('Secret command: /secret')}
`;
```

---

## 📚 ALL FORMATTERS

| Function | Output | Example |
|----------|--------|---------|
| `bold(text)` | `**text**` | **text** |
| `italic(text)` | `*text*` | *text* |
| `underline(text)` | `__text__` | __text__ |
| `strikethrough(text)` | `~~text~~` | ~~text~~ |
| `spoiler(text)` | `\|\|text\|\|` | ||text|| |
| `inlineCode(text)` | `` `text` `` | `text` |
| `codeBlock(lang, code)` | ` ```lang\ncode\n``` ` | ```code``` |
| `quote(text)` | `> text` | > text |
| `blockQuote(text)` | `>>> text` | >>> text |
| `userMention(id)` | `<@id>` | <@123> |
| `channelMention(id)` | `<#id>` | <#123> |
| `roleMention(id)` | `<@&id>` | <@&123> |
| `time(timestamp)` | `<t:timestamp>` | <t:1704844800> |
| `hyperlink(text, url)` | `[text](url)` | [text](url) |
| `hideLinkEmbed(url)` | `<url>` | <url> |
| `heading(text, level)` | `# text` | # text |

---

## 🔗 RESOURCES

- **NPM:** https://www.npmjs.com/package/@discordjs/formatters
- **GitHub:** https://github.com/discordjs/discord.js/tree/main/packages/formatters

---

**Created:** 2025-01-09  
**Version:** @discordjs/formatters v0.4+
EOF
cat discord.js-formatters.md
Output

# 📚 @DISCORDJS/FORMATTERS - TEXT FORMATTING DOCUMENTATION

**Package:** `@discordjs/formatters`  
**Version:** v0.4+  
**Purpose:** Utilities for formatting Discord text and mentions

---

## 📦 INSTALLATION

```bash
npm install @discordjs/formatters
```

---

## 🎯 TEXT FORMATTING

### **Bold**
```typescript
import { bold } from '@discordjs/formatters';

bold('text')  // **text**
```

### **Italic**
```typescript
import { italic } from '@discordjs/formatters';

italic('text')  // *text*
```

### **Underline**
```typescript
import { underline } from '@discordjs/formatters';

underline('text')  // __text__
```

### **Strikethrough**
```typescript
import { strikethrough } from '@discordjs/formatters';

strikethrough('text')  // ~~text~~
```

### **Spoiler**
```typescript
import { spoiler } from '@discordjs/formatters';

spoiler('text')  // ||text||
```

### **Code**
```typescript
import { inlineCode, codeBlock } from '@discordjs/formatters';

inlineCode('code')  // `code`

codeBlock('javascript', 'const x = 1;')
// ```javascript
// const x = 1;
// ```
```

### **Quote**
```typescript
import { quote, blockQuote } from '@discordjs/formatters';

quote('line')  // > line

blockQuote('multiple\nlines')
// >>> multiple
// lines
```

---

## 💬 MENTIONS

### **User Mention**
```typescript
import { userMention } from '@discordjs/formatters';

userMention('123456789')  // <@123456789>
```

### **Channel Mention**
```typescript
import { channelMention } from '@discordjs/formatters';

channelMention('123456789')  // <#123456789>
```

### **Role Mention**
```typescript
import { roleMention } from '@discordjs/formatters';

roleMention('123456789')  // <@&123456789>
```

### **Everyone/Here**
```typescript
import { EmbedBuilder } from 'discord.js';

// These are plain strings
'@everyone'  // Mentions everyone
'@here'      // Mentions online members
```

---

## ⏰ TIMESTAMPS

### **Timestamp Formatting**
```typescript
import { time, TimestampStyles } from '@discordjs/formatters';

const timestamp = Math.floor(Date.now() / 1000);

time(timestamp)                              // <t:1704844800>
time(timestamp, TimestampStyles.ShortTime)   // <t:1704844800:t> → 3:20 PM
time(timestamp, TimestampStyles.LongTime)    // <t:1704844800:T> → 3:20:00 PM
time(timestamp, TimestampStyles.ShortDate)   // <t:1704844800:d> → 01/09/2026
time(timestamp, TimestampStyles.LongDate)    // <t:1704844800:D> → January 9, 2026
time(timestamp, TimestampStyles.ShortDateTime) // <t:1704844800:f> → January 9, 2026 3:20 PM
time(timestamp, TimestampStyles.LongDateTime)  // <t:1704844800:F> → Thursday, January 9, 2026 3:20 PM
time(timestamp, TimestampStyles.RelativeTime)  // <t:1704844800:R> → 2 hours ago
```

---

## 🔗 LINKS

### **Hyperlink**
```typescript
import { hyperlink, hideLinkEmbed } from '@discordjs/formatters';

hyperlink('Google', 'https://google.com')  // [Google](https://google.com)

hideLinkEmbed('https://google.com')  // <https://google.com>
```

---

## 📝 HEADERS

```typescript
import { heading, HeadingLevel } from '@discordjs/formatters';

heading('Title', HeadingLevel.One)    // # Title
heading('Subtitle', HeadingLevel.Two)  // ## Subtitle
heading('Section', HeadingLevel.Three) // ### Section
```

---

## 📋 LISTS

### **Unordered List**
```typescript
import { unorderedList } from '@discordjs/formatters';

unorderedList(['Item 1', 'Item 2', 'Item 3'])
// - Item 1
// - Item 2
// - Item 3
```

### **Ordered List**
```typescript
import { orderedList } from '@discordjs/formatters';

orderedList(['First', 'Second', 'Third'])
// 1. First
// 2. Second
// 3. Third
```

---

## 🎨 COMBINING FORMATTERS

```typescript
import { bold, italic, underline, userMention } from '@discordjs/formatters';

// Bold + Italic
bold(italic('text'))  // ***text***

// Bold + Underline
bold(underline('text'))  // __**text**__

// Mention + Bold
`${userMention('123')} ${bold('Welcome!')}`  // <@123> **Welcome!**
```

---

## 💡 PRACTICAL EXAMPLES

### **Welcome Message**
```typescript
const welcomeMsg = `
${bold('Welcome!')} ${userMention(userId)}

Please read ${channelMention(rulesChannel)} before chatting.
Type ${inlineCode('/help')} for commands.

Joined at: ${time(Date.now() / 1000, TimestampStyles.ShortDateTime)}
`;
```

### **Embed with Formatting**
```typescript
import { EmbedBuilder } from 'discord.js';
import { bold, codeBlock, hyperlink } from '@discordjs/formatters';

const embed = new EmbedBuilder()
    .setTitle(bold('Server Stats'))
    .setDescription(`
        ${bold('Members:')} 1,234
        ${bold('Online:')} 567
        
        ${hyperlink('Visit Website', 'https://example.com')}
    `)
    .addFields({
        name: 'Code Example',
        value: codeBlock('javascript', 'console.log("Hello!");')
    });
```

### **Help Command**
```typescript
const help = `
${heading('Bot Commands', HeadingLevel.One)}

${bold('Moderation')}
${unorderedList([
    `${inlineCode('/ban')} - Ban a user`,
    `${inlineCode('/kick')} - Kick a user`,
    `${inlineCode('/mute')} - Mute a user`
])}

${bold('Fun')}
${orderedList([
    `${inlineCode('/meme')} - Get a random meme`,
    `${inlineCode('/joke')} - Tell a joke`
])}

${spoiler('Secret command: /secret')}
`;
```

---

## 📚 ALL FORMATTERS

| Function | Output | Example |
|----------|--------|---------|
| `bold(text)` | `**text**` | **text** |
| `italic(text)` | `*text*` | *text* |
| `underline(text)` | `__text__` | __text__ |
| `strikethrough(text)` | `~~text~~` | ~~text~~ |
| `spoiler(text)` | `\|\|text\|\|` | ||text|| |
| `inlineCode(text)` | `` `text` `` | `text` |
| `codeBlock(lang, code)` | ` ```lang\ncode\n``` ` | ```code``` |
| `quote(text)` | `> text` | > text |
| `blockQuote(text)` | `>>> text` | >>> text |
| `userMention(id)` | `<@id>` | <@123> |
| `channelMention(id)` | `<#id>` | <#123> |
| `roleMention(id)` | `<@&id>` | <@&123> |
| `time(timestamp)` | `<t:timestamp>` | <t:1704844800> |
| `hyperlink(text, url)` | `[text](url)` | [text](url) |
| `hideLinkEmbed(url)` | `<url>` | <url> |
| `heading(text, level)` | `# text` | # text |

---

## 🔗 RESOURCES

- **NPM:** https://www.npmjs.com/package/@discordjs/formatters
- **GitHub:** https://github.com/discordjs/discord.js/tree/main/packages/formatters

---

**Created:** 2025-01-09  
**Version:** @discordjs/formatters v0.4+
