# 📚 @discord.js/discord-api-types - TYPE DEFINITIONS DOCUMENTATION

**Package:** `discord-api-types`  
**Version:** v0.37+  
**Purpose:** TypeScript type definitions for Discord API

---

## 📦 INSTALLATION

```bash
npm install discord-api-types
```

---

## 🎯 OVERVIEW

Provides up-to-date TypeScript typings for the Discord API, including:
- REST API payloads
- Gateway events  
- Enums & constants
- All Discord structures

---

## 📋 MAIN EXPORTS

### **API Versions**
```typescript
import * as v10 from 'discord-api-types/v10';  // Latest
import * as v9 from 'discord-api-types/v9';    // Legacy
```

### **REST Routes**
```typescript
import { Routes } from 'discord-api-types/v10';

Routes.user()                                  // /users/@me
Routes.guild('id')                             // /guilds/{id}
Routes.channel('id')                           // /channels/{id}
Routes.channelMessages('id')                   // /channels/{id}/messages
```

### **Gateway Events**
```typescript
import { GatewayDispatchEvents } from 'discord-api-types/v10';

GatewayDispatchEvents.Ready
GatewayDispatchEvents.MessageCreate
GatewayDispatchEvents.GuildCreate
GatewayDispatchEvents.InteractionCreate
```

---

## 🏗️ MAIN TYPES

### **REST Payloads**
```typescript
import type {
    RESTPostAPIChannelMessageJSONBody,
    RESTGetAPIGuildMembersQuery,
    RESTPatchAPIGuildJSONBody
} from 'discord-api-types/v10';

const messagePayload: RESTPostAPIChannelMessageJSONBody = {
    content: 'Hello!',
    embeds: [{
        title: 'Title',
        description: 'Description'
    }]
};
```

### **Gateway Payloads**
```typescript
import type {
    GatewayReadyDispatch,
    GatewayMessageCreateDispatch,
    GatewayIdentifyData
} from 'discord-api-types/v10';
```

### **Discord Structures**
```typescript
import type {
    APIUser,
    APIGuild,
    APIChannel,
    APIMessage,
    APIRole,
    APIEmoji
} from 'discord-api-types/v10';

const user: APIUser = {
    id: '123456789',
    username: 'username',
    discriminator: '0001',
    avatar: null,
    bot: true
};
```

---

## 🎨 ENUMS

### **Channel Types**
```typescript
import { ChannelType } from 'discord-api-types/v10';

ChannelType.GuildText           // 0
ChannelType.DM                  // 1
ChannelType.GuildVoice          // 2
ChannelType.GroupDM             // 3
ChannelType.GuildCategory       // 4
ChannelType.GuildAnnouncement   // 5
ChannelType.AnnouncementThread  // 10
ChannelType.PublicThread        // 11
ChannelType.PrivateThread       // 12
ChannelType.GuildStageVoice     // 13
ChannelType.GuildDirectory      // 14
ChannelType.GuildForum          // 15
ChannelType.GuildMedia          // 16
```

### **Message Types**
```typescript
import { MessageType } from 'discord-api-types/v10';

MessageType.Default                    // 0
MessageType.UserJoin                   // 7
MessageType.GuildBoost                 // 8
MessageType.Reply                      // 19
MessageType.ChatInputCommand           // 20
MessageType.ContextMenuCommand         // 23
```

### **Button Styles**
```typescript
import { ButtonStyle } from 'discord-api-types/v10';

ButtonStyle.Primary    // 1 (Blurple)
ButtonStyle.Secondary  // 2 (Grey)
ButtonStyle.Success    // 3 (Green)
ButtonStyle.Danger     // 4 (Red)
ButtonStyle.Link       // 5 (Grey with URL)
ButtonStyle.Premium    // 6 (Purple)
```

### **Application Command Types**
```typescript
import { ApplicationCommandType } from 'discord-api-types/v10';

ApplicationCommandType.ChatInput  // 1 (Slash command)
ApplicationCommandType.User       // 2 (User context menu)
ApplicationCommandType.Message    // 3 (Message context menu)
```

---

## 🔧 UTILITY TYPES

### **Snowflake**
```typescript
import type { Snowflake } from 'discord-api-types/v10';

const userId: Snowflake = '123456789012345678';
```

### **Nullable**
```typescript
import type { Nullable } from 'discord-api-types/v10';

const avatar: Nullable<string> = null;
```

### **REST Methods**
```typescript
import type {
    RESTGetResult,
    RESTPostResult,
    RESTPatchResult,
    RESTDeleteResult
} from 'discord-api-types/v10';
```

---

## 💡 USAGE WITH DISCORD.JS

```typescript
import { Client } from 'discord.js';
import type { APIMessage, APIUser } from 'discord-api-types/v10';
import { Routes } from 'discord-api-types/v10';

// Type-safe REST calls
const message: APIMessage = await client.rest.post(
    Routes.channelMessages('channel_id'),
    {
        body: {
            content: 'Hello!'
        }
    }
) as APIMessage;

// Type-safe structures
const user: APIUser = await client.rest.get(
    Routes.user()
) as APIUser;
```

---

## 📚 COMMON TYPES REFERENCE

| Category | Types |
|----------|-------|
| **Users** | APIUser, APIGuildMember, RESTGetAPIUserResult |
| **Guilds** | APIGuild, APIRole, APIEmoji, APIBan |
| **Channels** | APIChannel, APIMessage, APIAttachment |
| **Commands** | APIApplicationCommand, APIApplicationCommandOption |
| **Interactions** | APIInteraction, APIMessageComponent, APIModal |
| **Voice** | APIVoiceState, APIVoiceRegion |
| **Webhooks** | APIWebhook |
| **Audit Log** | APIAuditLog, APIAuditLogEntry |

---

## 🔗 RESOURCES

- **NPM:** https://www.npmjs.com/package/discord-api-types
- **GitHub:** https://github.com/discordjs/discord-api-types
- **Discord API Docs:** https://discord.com/developers/docs

---

**Created:** 2025-01-09  
**Version:** discord-api-types v0.37+
EOFAPI

cat /home/claude/discord-api-types.md
Output

# 📚 DISCORD-API-TYPES - TYPE DEFINITIONS DOCUMENTATION

**Package:** `discord-api-types`  
**Version:** v0.37+  
**Purpose:** TypeScript type definitions for Discord API

---

## 📦 INSTALLATION

```bash
npm install discord-api-types
```

---

## 🎯 OVERVIEW

Provides up-to-date TypeScript typings for the Discord API, including:
- REST API payloads
- Gateway events  
- Enums & constants
- All Discord structures

---

## 📋 MAIN EXPORTS

### **API Versions**
```typescript
import * as v10 from 'discord-api-types/v10';  // Latest
import * as v9 from 'discord-api-types/v9';    // Legacy
```

### **REST Routes**
```typescript
import { Routes } from 'discord-api-types/v10';

Routes.user()                                  // /users/@me
Routes.guild('id')                             // /guilds/{id}
Routes.channel('id')                           // /channels/{id}
Routes.channelMessages('id')                   // /channels/{id}/messages
```

### **Gateway Events**
```typescript
import { GatewayDispatchEvents } from 'discord-api-types/v10';

GatewayDispatchEvents.Ready
GatewayDispatchEvents.MessageCreate
GatewayDispatchEvents.GuildCreate
GatewayDispatchEvents.InteractionCreate
```

---

## 🏗️ MAIN TYPES

### **REST Payloads**
```typescript
import type {
    RESTPostAPIChannelMessageJSONBody,
    RESTGetAPIGuildMembersQuery,
    RESTPatchAPIGuildJSONBody
} from 'discord-api-types/v10';

const messagePayload: RESTPostAPIChannelMessageJSONBody = {
    content: 'Hello!',
    embeds: [{
        title: 'Title',
        description: 'Description'
    }]
};
```

### **Gateway Payloads**
```typescript
import type {
    GatewayReadyDispatch,
    GatewayMessageCreateDispatch,
    GatewayIdentifyData
} from 'discord-api-types/v10';
```

### **Discord Structures**
```typescript
import type {
    APIUser,
    APIGuild,
    APIChannel,
    APIMessage,
    APIRole,
    APIEmoji
} from 'discord-api-types/v10';

const user: APIUser = {
    id: '123456789',
    username: 'username',
    discriminator: '0001',
    avatar: null,
    bot: true
};
```

---

## 🎨 ENUMS

### **Channel Types**
```typescript
import { ChannelType } from 'discord-api-types/v10';

ChannelType.GuildText           // 0
ChannelType.DM                  // 1
ChannelType.GuildVoice          // 2
ChannelType.GroupDM             // 3
ChannelType.GuildCategory       // 4
ChannelType.GuildAnnouncement   // 5
ChannelType.AnnouncementThread  // 10
ChannelType.PublicThread        // 11
ChannelType.PrivateThread       // 12
ChannelType.GuildStageVoice     // 13
ChannelType.GuildDirectory      // 14
ChannelType.GuildForum          // 15
ChannelType.GuildMedia          // 16
```

### **Message Types**
```typescript
import { MessageType } from 'discord-api-types/v10';

MessageType.Default                    // 0
MessageType.UserJoin                   // 7
MessageType.GuildBoost                 // 8
MessageType.Reply                      // 19
MessageType.ChatInputCommand           // 20
MessageType.ContextMenuCommand         // 23
```

### **Button Styles**
```typescript
import { ButtonStyle } from 'discord-api-types/v10';

ButtonStyle.Primary    // 1 (Blurple)
ButtonStyle.Secondary  // 2 (Grey)
ButtonStyle.Success    // 3 (Green)
ButtonStyle.Danger     // 4 (Red)
ButtonStyle.Link       // 5 (Grey with URL)
ButtonStyle.Premium    // 6 (Purple)
```

### **Application Command Types**
```typescript
import { ApplicationCommandType } from 'discord-api-types/v10';

ApplicationCommandType.ChatInput  // 1 (Slash command)
ApplicationCommandType.User       // 2 (User context menu)
ApplicationCommandType.Message    // 3 (Message context menu)
```

---

## 🔧 UTILITY TYPES

### **Snowflake**
```typescript
import type { Snowflake } from 'discord-api-types/v10';

const userId: Snowflake = '123456789012345678';
```

### **Nullable**
```typescript
import type { Nullable } from 'discord-api-types/v10';

const avatar: Nullable<string> = null;
```

### **REST Methods**
```typescript
import type {
    RESTGetResult,
    RESTPostResult,
    RESTPatchResult,
    RESTDeleteResult
} from 'discord-api-types/v10';
```

---

## 💡 USAGE WITH DISCORD.JS

```typescript
import { Client } from 'discord.js';
import type { APIMessage, APIUser } from 'discord-api-types/v10';
import { Routes } from 'discord-api-types/v10';

// Type-safe REST calls
const message: APIMessage = await client.rest.post(
    Routes.channelMessages('channel_id'),
    {
        body: {
            content: 'Hello!'
        }
    }
) as APIMessage;

// Type-safe structures
const user: APIUser = await client.rest.get(
    Routes.user()
) as APIUser;
```

---

## 📚 COMMON TYPES REFERENCE

| Category | Types |
|----------|-------|
| **Users** | APIUser, APIGuildMember, RESTGetAPIUserResult |
| **Guilds** | APIGuild, APIRole, APIEmoji, APIBan |
| **Channels** | APIChannel, APIMessage, APIAttachment |
| **Commands** | APIApplicationCommand, APIApplicationCommandOption |
| **Interactions** | APIInteraction, APIMessageComponent, APIModal |
| **Voice** | APIVoiceState, APIVoiceRegion |
| **Webhooks** | APIWebhook |
| **Audit Log** | APIAuditLog, APIAuditLogEntry |

---

## 🔗 RESOURCES

- **NPM:** https://www.npmjs.com/package/discord-api-types
- **GitHub:** https://github.com/discordjs/discord-api-types
- **Discord API Docs:** https://discord.com/developers/docs

---

**Created:** 2025-01-09  
**Version:** discord-api-types v0.37+
