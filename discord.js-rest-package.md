# 📚 @discord.js/rest - REST CLIENT DOCUMENTATION

**Package:** `@discordjs/rest`  
**Version:** v2+  
**Purpose:** REST API client for Discord API requests

---

## 🎯 OVERVIEW

The REST package provides a powerful and flexible way to interact with Discord's REST API. It handles rate limiting, request queuing, and provides a clean interface for making API calls.

---

## 📦 INSTALLATION

```bash
npm install @discordjs/rest
# or
yarn add @discordjs/rest
```

---

## 🚀 BASIC SETUP

```typescript
import { REST } from '@discordjs/rest';
import { Routes } from 'discord-api-types/v10';

const rest = new REST({ version: '10' }).setToken('YOUR_BOT_TOKEN');

// Make a request
const user = await rest.get(Routes.user());
console.log(user);
```

---

## 📋 MAIN COMPONENTS

### **1. REST CLASS**

#### **Constructor**

```typescript
interface RESTOptions {
    /**
     * API version to use
     * @default '10'
     */
    version?: string;
    
    /**
     * User agent to use
     */
    userAgentAppendix?: string;
    
    /**
     * Base URL for API requests
     * @default 'https://discord.com/api'
     */
    api?: string;
    
    /**
     * Base URL for CDN requests
     * @default 'https://cdn.discordapp.com'
     */
    cdn?: string;
    
    /**
     * Headers to include in all requests
     */
    headers?: Record<string, string>;
    
    /**
     * Timeout for requests (ms)
     * @default 15000
     */
    timeout?: number;
    
    /**
     * Number of retries
     * @default 3
     */
    retries?: number;
    
    /**
     * Offset for rate limits (ms)
     * @default 0
     */
    offset?: number;
    
    /**
     * Rejection filter
     */
    rejectOnRateLimit?: string[] | ((data: RateLimitData) => boolean | Promise<boolean>);
    
    /**
     * Global request per second limit
     * @default 50
     */
    globalRequestsPerSecond?: number;
    
    /**
     * HTTP agent
     */
    agent?: Dispatcher;
}
```

**Example:**
```typescript
const rest = new REST({
    version: '10',
    timeout: 30000,
    retries: 5,
    offset: 50
}).setToken(process.env.TOKEN!);
```

---

### **2. HTTP METHODS**

#### **GET**

```typescript
// Get current user
const user = await rest.get(Routes.user());

// Get guild
const guild = await rest.get(Routes.guild('guild_id'));

// Get channel
const channel = await rest.get(Routes.channel('channel_id'));

// Get guild members
const members = await rest.get(Routes.guildMembers('guild_id'), {
    query: new URLSearchParams({ limit: '100' })
});
```

---

#### **POST**

```typescript
// Send message
await rest.post(Routes.channelMessages('channel_id'), {
    body: {
        content: 'Hello World!',
        embeds: [{
            title: 'Embed Title',
            description: 'Embed description'
        }]
    }
});

// Create channel
await rest.post(Routes.guildChannels('guild_id'), {
    body: {
        name: 'new-channel',
        type: 0 // Text channel
    }
});

// Create role
await rest.post(Routes.guildRoles('guild_id'), {
    body: {
        name: 'New Role',
        color: 0x0099FF,
        permissions: '8' // Administrator
    }
});
```

---

#### **PATCH**

```typescript
// Edit message
await rest.patch(Routes.channelMessage('channel_id', 'message_id'), {
    body: {
        content: 'Edited content'
    }
});

// Edit guild
await rest.patch(Routes.guild('guild_id'), {
    body: {
        name: 'New Server Name',
        description: 'New description'
    }
});

// Edit channel
await rest.patch(Routes.channel('channel_id'), {
    body: {
        name: 'renamed-channel',
        topic: 'New topic'
    }
});
```

---

#### **PUT**

```typescript
// Add guild member role
await rest.put(Routes.guildMemberRole('guild_id', 'user_id', 'role_id'));

// Add reaction
await rest.put(Routes.channelMessageOwnReaction('channel_id', 'message_id', '👍'));

// Edit permissions
await rest.put(Routes.channelPermission('channel_id', 'overwrite_id'), {
    body: {
        type: 0, // Role
        allow: '2048', // Send Messages
        deny: '0'
    }
});
```

---

#### **DELETE**

```typescript
// Delete message
await rest.delete(Routes.channelMessage('channel_id', 'message_id'));

// Delete channel
await rest.delete(Routes.channel('channel_id'));

// Delete guild
await rest.delete(Routes.guild('guild_id'));

// Remove guild member role
await rest.delete(Routes.guildMemberRole('guild_id', 'user_id', 'role_id'));

// Delete own reaction
await rest.delete(Routes.channelMessageOwnReaction('channel_id', 'message_id', '👍'));
```

---

### **3. ROUTES**

Routes from `discord-api-types/v10`:

```typescript
import { Routes } from 'discord-api-types/v10';

// User
Routes.user()                                        // /users/@me
Routes.userGuilds()                                  // /users/@me/guilds
Routes.userGuild('guild_id')                         // /users/@me/guilds/{guild_id}

// Guild
Routes.guild('guild_id')                             // /guilds/{guild_id}
Routes.guildChannels('guild_id')                     // /guilds/{guild_id}/channels
Routes.guildMembers('guild_id')                      // /guilds/{guild_id}/members
Routes.guildMember('guild_id', 'user_id')           // /guilds/{guild_id}/members/{user_id}
Routes.guildRoles('guild_id')                        // /guilds/{guild_id}/roles
Routes.guildRole('guild_id', 'role_id')             // /guilds/{guild_id}/roles/{role_id}
Routes.guildBans('guild_id')                         // /guilds/{guild_id}/bans
Routes.guildBan('guild_id', 'user_id')              // /guilds/{guild_id}/bans/{user_id}

// Channel
Routes.channel('channel_id')                         // /channels/{channel_id}
Routes.channelMessages('channel_id')                 // /channels/{channel_id}/messages
Routes.channelMessage('channel_id', 'message_id')   // /channels/{channel_id}/messages/{message_id}

// Interactions
Routes.interactionCallback('interaction_id', 'token') // /interactions/{id}/{token}/callback
Routes.webhook('webhook_id', 'token')                // /webhooks/{id}/{token}

// Application Commands
Routes.applicationCommands('application_id')         // /applications/{id}/commands
Routes.applicationGuildCommands('app_id', 'guild_id') // /applications/{id}/guilds/{guild_id}/commands
```

---

### **4. RATE LIMITING**

The REST client automatically handles rate limits:

```typescript
// Rate limit events
rest.on('rateLimited', (info) => {
    console.log(`Rate limited for ${info.timeToReset}ms`);
    console.log(`Route: ${info.route}`);
    console.log(`Global: ${info.global}`);
});

// Custom rate limit handling
const rest = new REST({
    rejectOnRateLimit: (data) => {
        // Reject if global rate limit
        return data.global;
    }
});

// Or reject specific routes
const rest = new REST({
    rejectOnRateLimit: ['/channels/:id/messages']
});
```

---

### **5. REQUEST OPTIONS**

```typescript
interface RequestData {
    /**
     * Request body
     */
    body?: unknown;
    
    /**
     * Files to upload
     */
    files?: RawFile[];
    
    /**
     * Query parameters
     */
    query?: URLSearchParams;
    
    /**
     * Headers
     */
    headers?: Record<string, string>;
    
    /**
     * Authentication
     */
    auth?: boolean;
    
    /**
     * Reason for audit log
     */
    reason?: string;
}
```

**Example:**
```typescript
// With query parameters
await rest.get(Routes.guildMembers('guild_id'), {
    query: new URLSearchParams({
        limit: '100',
        after: 'user_id'
    })
});

// With audit log reason
await rest.delete(Routes.guildMember('guild_id', 'user_id'), {
    reason: 'Spamming'
});

// With custom headers
await rest.post(Routes.channelMessages('channel_id'), {
    body: { content: 'Hello!' },
    headers: {
        'X-Custom-Header': 'value'
    }
});
```

---

### **6. FILE UPLOADS**

```typescript
import { readFile } from 'fs/promises';

// Upload single file
const file = await readFile('./image.png');

await rest.post(Routes.channelMessages('channel_id'), {
    body: {
        content: 'Check this image!'
    },
    files: [{
        name: 'image.png',
        data: file
    }]
});

// Upload multiple files
await rest.post(Routes.channelMessages('channel_id'), {
    body: {
        content: 'Multiple files'
    },
    files: [
        { name: 'file1.png', data: await readFile('./file1.png') },
        { name: 'file2.png', data: await readFile('./file2.png') }
    ]
});

// Attach files to embed
await rest.post(Routes.channelMessages('channel_id'), {
    body: {
        embeds: [{
            title: 'Image',
            image: { url: 'attachment://image.png' }
        }]
    },
    files: [{
        name: 'image.png',
        data: await readFile('./image.png')
    }]
});
```

---

## 📊 COMMON USE CASES

### **1. Send Messages**

```typescript
// Simple message
await rest.post(Routes.channelMessages('channel_id'), {
    body: {
        content: 'Hello World!'
    }
});

// Message with embed
await rest.post(Routes.channelMessages('channel_id'), {
    body: {
        content: 'Check this embed:',
        embeds: [{
            title: 'Embed Title',
            description: 'Embed description',
            color: 0x0099FF,
            fields: [
                { name: 'Field 1', value: 'Value 1', inline: true },
                { name: 'Field 2', value: 'Value 2', inline: true }
            ]
        }]
    }
});

// Message with components
await rest.post(Routes.channelMessages('channel_id'), {
    body: {
        content: 'Click a button:',
        components: [{
            type: 1, // Action Row
            components: [{
                type: 2, // Button
                style: 1, // Primary
                label: 'Click Me',
                custom_id: 'button_1'
            }]
        }]
    }
});
```

---

### **2. Manage Members**

```typescript
// Kick member
await rest.delete(Routes.guildMember('guild_id', 'user_id'), {
    reason: 'Violation of rules'
});

// Ban member
await rest.put(Routes.guildBan('guild_id', 'user_id'), {
    body: {
        delete_message_seconds: 86400 // Delete messages from last day
    },
    reason: 'Spamming'
});

// Unban member
await rest.delete(Routes.guildBan('guild_id', 'user_id'));

// Edit member
await rest.patch(Routes.guildMember('guild_id', 'user_id'), {
    body: {
        nick: 'New Nickname',
        roles: ['role_id_1', 'role_id_2']
    }
});

// Timeout member
await rest.patch(Routes.guildMember('guild_id', 'user_id'), {
    body: {
        communication_disabled_until: new Date(Date.now() + 60000).toISOString() // 1 minute
    },
    reason: 'Spamming'
});
```

---

### **3. Manage Channels**

```typescript
// Create text channel
await rest.post(Routes.guildChannels('guild_id'), {
    body: {
        name: 'general',
        type: 0, // Text channel
        topic: 'General discussion',
        nsfw: false
    }
});

// Create voice channel
await rest.post(Routes.guildChannels('guild_id'), {
    body: {
        name: 'Voice Chat',
        type: 2, // Voice channel
        bitrate: 96000,
        user_limit: 10
    }
});

// Edit channel
await rest.patch(Routes.channel('channel_id'), {
    body: {
        name: 'renamed-channel',
        topic: 'New topic',
        nsfw: true
    }
});

// Delete channel
await rest.delete(Routes.channel('channel_id'), {
    reason: 'No longer needed'
});

// Bulk delete messages
await rest.post(Routes.channelBulkDelete('channel_id'), {
    body: {
        messages: ['message_id_1', 'message_id_2']
    }
});
```

---

### **4. Manage Roles**

```typescript
// Create role
await rest.post(Routes.guildRoles('guild_id'), {
    body: {
        name: 'Moderator',
        color: 0x0099FF,
        hoist: true,
        permissions: '8', // Administrator
        mentionable: true
    }
});

// Edit role
await rest.patch(Routes.guildRole('guild_id', 'role_id'), {
    body: {
        name: 'Admin',
        color: 0xFF0000,
        permissions: '8'
    }
});

// Delete role
await rest.delete(Routes.guildRole('guild_id', 'role_id'), {
    reason: 'No longer needed'
});

// Add role to member
await rest.put(Routes.guildMemberRole('guild_id', 'user_id', 'role_id'));

// Remove role from member
await rest.delete(Routes.guildMemberRole('guild_id', 'user_id', 'role_id'));
```

---

### **5. Slash Commands**

```typescript
// Register global command
await rest.post(Routes.applicationCommands('application_id'), {
    body: {
        name: 'ping',
        description: 'Replies with Pong!',
        type: 1 // Chat input
    }
});

// Register guild command
await rest.post(Routes.applicationGuildCommands('application_id', 'guild_id'), {
    body: {
        name: 'admin',
        description: 'Admin command',
        default_member_permissions: '8' // Administrator
    }
});

// Get all commands
const commands = await rest.get(Routes.applicationCommands('application_id'));

// Delete command
await rest.delete(Routes.applicationCommand('application_id', 'command_id'));

// Bulk overwrite commands
await rest.put(Routes.applicationCommands('application_id'), {
    body: [
        { name: 'ping', description: 'Ping command' },
        { name: 'help', description: 'Help command' }
    ]
});
```

---

### **6. Interaction Responses**

```typescript
// Reply to interaction
await rest.post(Routes.interactionCallback('interaction_id', 'token'), {
    body: {
        type: 4, // CHANNEL_MESSAGE_WITH_SOURCE
        data: {
            content: 'Response!',
            flags: 64 // Ephemeral
        }
    }
});

// Deferred response
await rest.post(Routes.interactionCallback('interaction_id', 'token'), {
    body: {
        type: 5 // DEFERRED_CHANNEL_MESSAGE_WITH_SOURCE
    }
});

// Edit original response
await rest.patch(Routes.webhookMessage('application_id', 'token', '@original'), {
    body: {
        content: 'Edited response'
    }
});

// Delete original response
await rest.delete(Routes.webhookMessage('application_id', 'token', '@original'));
```

---

### **7. Webhooks**

```typescript
// Create webhook
const webhook = await rest.post(Routes.channelWebhooks('channel_id'), {
    body: {
        name: 'My Webhook',
        avatar: null
    }
});

// Execute webhook
await rest.post(Routes.webhook('webhook_id', 'token'), {
    body: {
        content: 'Hello from webhook!',
        username: 'Custom Name',
        avatar_url: 'https://example.com/avatar.png'
    }
});

// Edit webhook
await rest.patch(Routes.webhook('webhook_id'), {
    auth: false, // No auth needed with token
    body: {
        name: 'New Name'
    }
});

// Delete webhook
await rest.delete(Routes.webhook('webhook_id', 'token'), {
    auth: false
});
```

---

## 🔧 ADVANCED FEATURES

### **1. CDN URLs**

```typescript
// Get CDN instance
const cdn = rest.cdn;

// User avatar
const avatarURL = cdn.avatar('user_id', 'avatar_hash', { size: 512 });

// Guild icon
const iconURL = cdn.icon('guild_id', 'icon_hash', { format: 'png' });

// Emoji
const emojiURL = cdn.emoji('emoji_id', { format: 'gif' });

// Banner
const bannerURL = cdn.banner('user_id', 'banner_hash');

// Default avatar
const defaultURL = cdn.defaultAvatar(0); // Discriminator
```

---

### **2. Error Handling**

```typescript
import { DiscordAPIError, HTTPError, RateLimitError } from '@discordjs/rest';

try {
    await rest.post(Routes.channelMessages('channel_id'), {
        body: { content: 'Hello!' }
    });
} catch (error) {
    if (error instanceof DiscordAPIError) {
        console.error(`API Error: ${error.code} - ${error.message}`);
        console.error(`Status: ${error.status}`);
    } else if (error instanceof HTTPError) {
        console.error(`HTTP Error: ${error.status}`);
    } else if (error instanceof RateLimitError) {
        console.error(`Rate limited for ${error.timeToReset}ms`);
    } else {
        console.error('Unknown error:', error);
    }
}
```

---

### **3. Request Queue**

```typescript
// Queue info
rest.on('response', (request) => {
    console.log(`Request to ${request.path} completed`);
});

// Request info
rest.on('request', (request) => {
    console.log(`Requesting ${request.method} ${request.path}`);
});

// Get queue size
console.log(`Queued requests: ${rest.queueSize}`);
```

---

### **4. OAuth2**

```typescript
// Get OAuth2 URL
const authURL = `https://discord.com/api/oauth2/authorize?client_id=${client_id}&redirect_uri=${redirect_uri}&response_type=code&scope=identify%20guilds`;

// Exchange code for token
const tokenResponse = await rest.post(Routes.oauth2TokenExchange(), {
    auth: false,
    body: new URLSearchParams({
        client_id: 'client_id',
        client_secret: 'client_secret',
        grant_type: 'authorization_code',
        code: 'code',
        redirect_uri: 'redirect_uri'
    }),
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
    }
});

// Use access token
const userRest = new REST().setToken(tokenResponse.access_token);
const user = await userRest.get(Routes.user());
```

---

## 💡 BEST PRACTICES

### **1. Reuse REST Instance**

```typescript
// Good: Single instance
const rest = new REST({ version: '10' }).setToken(token);

// Bad: Multiple instances
const rest1 = new REST().setToken(token);
const rest2 = new REST().setToken(token);
```

---

### **2. Handle Rate Limits**

```typescript
// Listen to rate limit events
rest.on('rateLimited', (info) => {
    console.warn(`Rate limited: ${info.route}`);
});

// Use offset to stay under limits
const rest = new REST({ offset: 50 });
```

---

### **3. Use Audit Log Reasons**

```typescript
// Always provide reasons for moderation actions
await rest.delete(Routes.guildMember('guild_id', 'user_id'), {
    reason: 'Spamming in #general'
});
```

---

### **4. Batch Operations**

```typescript
// Bulk delete messages
await rest.post(Routes.channelBulkDelete('channel_id'), {
    body: {
        messages: messageIds // Array of message IDs
    }
});

// Bulk overwrite commands
await rest.put(Routes.applicationCommands('app_id'), {
    body: commands
});
```

---

## 📚 RESOURCES

- **Documentation:** https://discord.js.org/#/docs/rest
- **Discord API:** https://discord.com/developers/docs
- **GitHub:** https://github.com/discordjs/discord.js/tree/main/packages/rest

---

**Created:** 2025-01-09  
**Version:** @discordjs/rest v2+  
**Last Updated:** 2025-01-09
