# 📚 discord.js - MAIN LIBRARY DOCUMENTATION

**Package:** `discord.js`  
**Version:** v14+  
**Purpose:** Complete Discord bot framework for Node.js

---

## 🎯 OVERVIEW

Discord.js is a powerful Node.js module that allows you to interact with the Discord API very easily. It takes a much more object-oriented approach than most other JS Discord libraries, making your bot's code significantly tidier and easier to comprehend.

---

## 📦 INSTALLATION

```bash
npm install discord.js
# or
yarn add discord.js
```

---

## 🚀 BASIC SETUP

```typescript
import { Client, GatewayIntentBits } from 'discord.js';

const client = new Client({
    intents: [
        GatewayIntentBits.Guilds,
        GatewayIntentBits.GuildMessages,
        GatewayIntentBits.MessageContent
    ]
});

client.once('ready', () => {
    console.log(`Logged in as ${client.user?.tag}`);
});

client.on('messageCreate', message => {
    if (message.content === '!ping') {
        message.reply('Pong!');
    }
});

client.login('YOUR_BOT_TOKEN');
```

---

## 📋 MAIN COMPONENTS

### **1. CLIENT**

#### **Client Class**

Main interface for interacting with Discord API.

**Constructor Options:**
```typescript
interface ClientOptions {
    intents: GatewayIntentBits[];
    partials?: Partials[];
    presence?: PresenceData;
    ws?: WebSocketOptions;
    rest?: RESTOptions;
    shards?: number | number[] | 'auto';
    shardCount?: number;
    closeTimeout?: number;
    waitGuildTimeout?: number;
    makeCache?: CacheFactory;
    sweepers?: SweeperOptions;
    failIfNotExists?: boolean;
    allowedMentions?: MessageMentionOptions;
    rejectOnRateLimit?: Function;
}
```

**Example:**
```typescript
const client = new Client({
    intents: [
        GatewayIntentBits.Guilds,
        GatewayIntentBits.GuildMessages,
        GatewayIntentBits.MessageContent,
        GatewayIntentBits.GuildMembers,
        GatewayIntentBits.GuildPresences
    ],
    partials: [
        Partials.Message,
        Partials.Channel,
        Partials.Reaction
    ]
});
```

**Properties:**
- `client.user` - Bot user object
- `client.guilds` - Collection of guilds
- `client.channels` - Collection of channels
- `client.users` - Collection of users
- `client.emojis` - Collection of emojis
- `client.application` - Application information
- `client.ws` - WebSocket manager
- `client.rest` - REST manager

**Methods:**
- `client.login(token)` - Login to Discord
- `client.destroy()` - Destroy client connection
- `client.isReady()` - Check if client is ready
- `client.generateInvite(options)` - Generate bot invite link

---

### **2. GATEWAY INTENTS**

Intents define which events your bot receives.

```typescript
enum GatewayIntentBits {
    Guilds = 1 << 0,                    // Guild info
    GuildMembers = 1 << 1,              // Member join/leave (Privileged)
    GuildModeration = 1 << 2,           // Bans, kicks
    GuildEmojisAndStickers = 1 << 3,    // Custom emojis/stickers
    GuildIntegrations = 1 << 4,         // Integrations
    GuildWebhooks = 1 << 5,             // Webhooks
    GuildInvites = 1 << 6,              // Invites
    GuildVoiceStates = 1 << 7,          // Voice states
    GuildPresences = 1 << 8,            // Presences (Privileged)
    GuildMessages = 1 << 9,             // Messages in guilds
    GuildMessageReactions = 1 << 10,    // Reactions
    GuildMessageTyping = 1 << 11,       // Typing indicators
    DirectMessages = 1 << 12,           // DMs
    DirectMessageReactions = 1 << 13,   // DM reactions
    DirectMessageTyping = 1 << 14,      // DM typing
    MessageContent = 1 << 15,           // Message content (Privileged)
    GuildScheduledEvents = 1 << 16,     // Scheduled events
    AutoModerationConfiguration = 1 << 20, // AutoMod config
    AutoModerationExecution = 1 << 21   // AutoMod actions
}
```

**Privileged Intents:**
- `GuildMembers` - Requires approval for 100+ servers
- `GuildPresences` - Requires approval for 100+ servers
- `MessageContent` - Requires approval for 100+ servers

**Common Combinations:**
```typescript
// Basic bot
intents: [
    GatewayIntentBits.Guilds,
    GatewayIntentBits.GuildMessages
]

// Moderation bot
intents: [
    GatewayIntentBits.Guilds,
    GatewayIntentBits.GuildMembers,
    GatewayIntentBits.GuildModeration,
    GatewayIntentBits.MessageContent
]

// Music bot
intents: [
    GatewayIntentBits.Guilds,
    GatewayIntentBits.GuildVoiceStates
]
```

---

### **3. EVENTS**

#### **Client Events**

**Ready Event:**
```typescript
client.once('ready', (client) => {
    console.log(`Ready! Logged in as ${client.user.tag}`);
});
```

**Message Events:**
```typescript
// Message created
client.on('messageCreate', (message) => {
    if (message.author.bot) return;
    console.log(`${message.author.tag}: ${message.content}`);
});

// Message deleted
client.on('messageDelete', (message) => {
    console.log(`Message deleted: ${message.content}`);
});

// Message updated
client.on('messageUpdate', (oldMessage, newMessage) => {
    console.log(`Message edited from "${oldMessage.content}" to "${newMessage.content}"`);
});

// Bulk delete
client.on('messageDeleteBulk', (messages) => {
    console.log(`${messages.size} messages deleted`);
});
```

**Interaction Events:**
```typescript
// Slash commands, buttons, select menus, modals
client.on('interactionCreate', async (interaction) => {
    if (interaction.isChatInputCommand()) {
        // Slash command
        console.log(`Command: ${interaction.commandName}`);
    }
    
    if (interaction.isButton()) {
        // Button click
        console.log(`Button: ${interaction.customId}`);
    }
    
    if (interaction.isStringSelectMenu()) {
        // Select menu
        console.log(`Select: ${interaction.values}`);
    }
    
    if (interaction.isModalSubmit()) {
        // Modal submit
        console.log(`Modal: ${interaction.customId}`);
    }
});
```

**Guild Events:**
```typescript
// Guild create (bot joins server)
client.on('guildCreate', (guild) => {
    console.log(`Joined guild: ${guild.name}`);
});

// Guild delete (bot removed from server)
client.on('guildDelete', (guild) => {
    console.log(`Left guild: ${guild.name}`);
});

// Guild update
client.on('guildUpdate', (oldGuild, newGuild) => {
    console.log(`Guild updated: ${newGuild.name}`);
});
```

**Member Events:**
```typescript
// Member join
client.on('guildMemberAdd', (member) => {
    console.log(`${member.user.tag} joined ${member.guild.name}`);
});

// Member leave
client.on('guildMemberRemove', (member) => {
    console.log(`${member.user.tag} left ${member.guild.name}`);
});

// Member update (roles, nickname, etc)
client.on('guildMemberUpdate', (oldMember, newMember) => {
    console.log(`Member updated: ${newMember.user.tag}`);
});
```

**Role Events:**
```typescript
// Role created
client.on('roleCreate', (role) => {
    console.log(`Role created: ${role.name}`);
});

// Role deleted
client.on('roleDelete', (role) => {
    console.log(`Role deleted: ${role.name}`);
});

// Role updated
client.on('roleUpdate', (oldRole, newRole) => {
    console.log(`Role updated: ${newRole.name}`);
});
```

**Channel Events:**
```typescript
// Channel created
client.on('channelCreate', (channel) => {
    console.log(`Channel created: ${channel.name}`);
});

// Channel deleted
client.on('channelDelete', (channel) => {
    console.log(`Channel deleted: ${channel.name}`);
});

// Channel updated
client.on('channelUpdate', (oldChannel, newChannel) => {
    console.log(`Channel updated: ${newChannel.name}`);
});
```

**Presence Events:**
```typescript
// Presence update (status, activity)
client.on('presenceUpdate', (oldPresence, newPresence) => {
    console.log(`${newPresence.user?.tag} is now ${newPresence.status}`);
});
```

**Voice Events:**
```typescript
// Voice state update (join/leave/mute/deafen)
client.on('voiceStateUpdate', (oldState, newState) => {
    if (!oldState.channel && newState.channel) {
        console.log(`${newState.member?.user.tag} joined ${newState.channel.name}`);
    }
});
```

**Reaction Events:**
```typescript
// Reaction added
client.on('messageReactionAdd', (reaction, user) => {
    console.log(`${user.tag} reacted with ${reaction.emoji.name}`);
});

// Reaction removed
client.on('messageReactionRemove', (reaction, user) => {
    console.log(`${user.tag} removed reaction ${reaction.emoji.name}`);
});
```

**Error Events:**
```typescript
// Error
client.on('error', (error) => {
    console.error('Client error:', error);
});

// Warning
client.on('warn', (info) => {
    console.warn('Client warning:', info);
});

// Debug
client.on('debug', (info) => {
    console.log('Debug:', info);
});
```

---

### **4. STRUCTURES**

#### **Guild (Server)**

```typescript
interface Guild {
    id: Snowflake;
    name: string;
    icon: string | null;
    description: string | null;
    ownerId: Snowflake;
    memberCount: number;
    members: GuildMemberManager;
    channels: GuildChannelManager;
    roles: RoleManager;
    emojis: GuildEmojiManager;
    
    // Methods
    fetch(): Promise<Guild>;
    edit(options: GuildEditOptions): Promise<Guild>;
    delete(): Promise<Guild>;
    setName(name: string): Promise<Guild>;
    setIcon(icon: BufferResolvable): Promise<Guild>;
    leave(): Promise<Guild>;
}
```

**Example Usage:**
```typescript
// Get guild by ID
const guild = client.guilds.cache.get('123456789');

// Edit guild
await guild.edit({
    name: 'New Server Name',
    description: 'New description'
});

// Get member count
console.log(`Members: ${guild.memberCount}`);

// Get roles
guild.roles.cache.forEach(role => {
    console.log(role.name);
});
```

---

#### **User**

```typescript
interface User {
    id: Snowflake;
    username: string;
    discriminator: string;
    tag: string; // username#discriminator
    avatar: string | null;
    bot: boolean;
    system: boolean;
    flags: UserFlagsBitField;
    
    // Methods
    fetch(force?: boolean): Promise<User>;
    send(options: MessageCreateOptions): Promise<Message>;
    displayAvatarURL(options?: ImageURLOptions): string;
    toString(): string; // <@userId>
}
```

**Example Usage:**
```typescript
// Get user
const user = await client.users.fetch('123456789');

// Send DM
await user.send('Hello!');

// Get avatar URL
const avatarURL = user.displayAvatarURL({ size: 512 });

// Mention user
message.channel.send(`Hello ${user}!`);
```

---

#### **GuildMember**

```typescript
interface GuildMember {
    id: Snowflake;
    user: User;
    nickname: string | null;
    displayName: string;
    roles: GuildMemberRoleManager;
    joinedAt: Date | null;
    premiumSince: Date | null;
    voice: VoiceState;
    permissions: Readonly<PermissionsBitField>;
    
    // Methods
    kick(reason?: string): Promise<GuildMember>;
    ban(options?: BanOptions): Promise<GuildMember>;
    timeout(timeout: number, reason?: string): Promise<GuildMember>;
    setNickname(nickname: string): Promise<GuildMember>;
    send(options: MessageCreateOptions): Promise<Message>;
}
```

**Example Usage:**
```typescript
// Get member
const member = await guild.members.fetch('123456789');

// Add role
await member.roles.add('role_id');

// Remove role
await member.roles.remove('role_id');

// Timeout (mute)
await member.timeout(60000, 'Spamming'); // 1 minute

// Kick
await member.kick('Violation of rules');

// Ban
await member.ban({ reason: 'Spam', deleteMessageSeconds: 86400 });

// Check permissions
if (member.permissions.has('Administrator')) {
    console.log('Member is admin');
}
```

---

#### **Message**

```typescript
interface Message {
    id: Snowflake;
    content: string;
    author: User;
    member: GuildMember | null;
    channel: TextBasedChannel;
    guild: Guild | null;
    attachments: Collection<Snowflake, Attachment>;
    embeds: Embed[];
    mentions: MessageMentions;
    createdTimestamp: number;
    editedTimestamp: number | null;
    
    // Methods
    reply(options: MessageReplyOptions): Promise<Message>;
    edit(options: MessageEditOptions): Promise<Message>;
    delete(): Promise<Message>;
    react(emoji: EmojiIdentifierResolvable): Promise<MessageReaction>;
    pin(): Promise<Message>;
    unpin(): Promise<Message>;
    fetch(force?: boolean): Promise<Message>;
}
```

**Example Usage:**
```typescript
// Reply to message
await message.reply('Hello!');

// Edit message
await message.edit('New content');

// Delete message
await message.delete();

// React to message
await message.react('👍');
await message.react('<:custom:123456789>');

// Pin message
await message.pin();

// Check content
if (message.content.includes('spam')) {
    await message.delete();
}
```

---

#### **Channel Types**

**TextChannel:**
```typescript
interface TextChannel {
    id: Snowflake;
    name: string;
    type: ChannelType.GuildText;
    topic: string | null;
    nsfw: boolean;
    
    // Methods
    send(options: MessageCreateOptions): Promise<Message>;
    bulkDelete(messages: number | Message[]): Promise<Collection<Snowflake, Message>>;
    setTopic(topic: string): Promise<TextChannel>;
    setNSFW(nsfw: boolean): Promise<TextChannel>;
}
```

**VoiceChannel:**
```typescript
interface VoiceChannel {
    id: Snowflake;
    name: string;
    type: ChannelType.GuildVoice;
    bitrate: number;
    userLimit: number;
    members: Collection<Snowflake, GuildMember>;
    
    // Methods
    setBitrate(bitrate: number): Promise<VoiceChannel>;
    setUserLimit(userLimit: number): Promise<VoiceChannel>;
}
```

**Example Usage:**
```typescript
// Send message to text channel
const channel = await client.channels.fetch('123456789') as TextChannel;
await channel.send('Hello!');

// Bulk delete messages
await channel.bulkDelete(10); // Delete last 10 messages

// Set channel topic
await channel.setTopic('New topic');

// Get voice channel members
const voiceChannel = guild.channels.cache.get('123456789') as VoiceChannel;
console.log(`Users in VC: ${voiceChannel.members.size}`);
```

---

#### **Role**

```typescript
interface Role {
    id: Snowflake;
    name: string;
    color: number;
    hoist: boolean;
    position: number;
    permissions: PermissionsBitField;
    mentionable: boolean;
    
    // Methods
    edit(options: RoleEditOptions): Promise<Role>;
    delete(reason?: string): Promise<Role>;
    setName(name: string): Promise<Role>;
    setColor(color: ColorResolvable): Promise<Role>;
    setPermissions(permissions: PermissionResolvable): Promise<Role>;
}
```

**Example Usage:**
```typescript
// Create role
const role = await guild.roles.create({
    name: 'Moderator',
    color: 'Blue',
    permissions: ['KickMembers', 'BanMembers']
});

// Edit role
await role.edit({
    name: 'Admin',
    color: 'Red',
    permissions: ['Administrator']
});

// Delete role
await role.delete('No longer needed');

// Check role permissions
if (role.permissions.has('Administrator')) {
    console.log('Admin role');
}
```

---

### **5. MANAGERS**

#### **GuildManager**

```typescript
// Fetch guild
const guild = await client.guilds.fetch('123456789');

// Cache access
const guilds = client.guilds.cache;

// Create guild (bots can't create guilds normally)
// Only available for whitelisted bots
```

---

#### **GuildMemberManager**

```typescript
// Fetch member
const member = await guild.members.fetch('123456789');

// Fetch all members
await guild.members.fetch();

// Search members
const members = await guild.members.search({
    query: 'username',
    limit: 10
});

// Ban member
await guild.members.ban('123456789', { reason: 'Spam' });

// Unban member
await guild.members.unban('123456789');

// Kick member
const member = await guild.members.fetch('123456789');
await guild.members.kick(member);
```

---

#### **ChannelManager**

```typescript
// Fetch channel
const channel = await client.channels.fetch('123456789');

// Create text channel
const textChannel = await guild.channels.create({
    name: 'general',
    type: ChannelType.GuildText,
    topic: 'General discussion'
});

// Create voice channel
const voiceChannel = await guild.channels.create({
    name: 'Voice Chat',
    type: ChannelType.GuildVoice,
    bitrate: 96000
});

// Create category
const category = await guild.channels.create({
    name: 'Category',
    type: ChannelType.GuildCategory
});

// Delete channel
await channel.delete();
```

---

#### **RoleManager**

```typescript
// Fetch role
const role = guild.roles.cache.get('123456789');

// Create role
const role = await guild.roles.create({
    name: 'Member',
    color: 'Blue',
    permissions: ['SendMessages', 'ViewChannels']
});

// Delete role
await guild.roles.delete('123456789');

// Get highest role
const highest = guild.roles.highest;

// Get everyone role
const everyone = guild.roles.everyone;
```

---

### **6. PERMISSIONS**

```typescript
enum PermissionFlagsBits {
    CreateInstantInvite = 1n << 0n,
    KickMembers = 1n << 1n,
    BanMembers = 1n << 2n,
    Administrator = 1n << 3n,
    ManageChannels = 1n << 4n,
    ManageGuild = 1n << 5n,
    AddReactions = 1n << 6n,
    ViewAuditLog = 1n << 7n,
    PrioritySpeaker = 1n << 8n,
    Stream = 1n << 9n,
    ViewChannel = 1n << 10n,
    SendMessages = 1n << 11n,
    SendTTSMessages = 1n << 12n,
    ManageMessages = 1n << 13n,
    EmbedLinks = 1n << 14n,
    AttachFiles = 1n << 15n,
    ReadMessageHistory = 1n << 16n,
    MentionEveryone = 1n << 17n,
    UseExternalEmojis = 1n << 18n,
    ViewGuildInsights = 1n << 19n,
    Connect = 1n << 20n,
    Speak = 1n << 21n,
    MuteMembers = 1n << 22n,
    DeafenMembers = 1n << 23n,
    MoveMembers = 1n << 24n,
    UseVAD = 1n << 25n,
    ChangeNickname = 1n << 26n,
    ManageNicknames = 1n << 27n,
    ManageRoles = 1n << 28n,
    ManageWebhooks = 1n << 29n,
    ManageGuildExpressions = 1n << 30n,
    UseApplicationCommands = 1n << 31n,
    RequestToSpeak = 1n << 32n,
    ManageEvents = 1n << 33n,
    ManageThreads = 1n << 34n,
    CreatePublicThreads = 1n << 35n,
    CreatePrivateThreads = 1n << 36n,
    UseExternalStickers = 1n << 37n,
    SendMessagesInThreads = 1n << 38n,
    UseEmbeddedActivities = 1n << 39n,
    ModerateMembers = 1n << 40n,
    ViewCreatorMonetizationAnalytics = 1n << 41n,
    UseSoundboard = 1n << 42n,
    UseExternalSounds = 1n << 45n,
    SendVoiceMessages = 1n << 46n
}
```

**Check Permissions:**
```typescript
// Check if member has permission
if (member.permissions.has(PermissionFlagsBits.Administrator)) {
    console.log('Member is admin');
}

// Check multiple permissions
if (member.permissions.has([
    PermissionFlagsBits.KickMembers,
    PermissionFlagsBits.BanMembers
])) {
    console.log('Member can moderate');
}

// Check channel permissions
const permissions = channel.permissionsFor(member);
if (permissions.has(PermissionFlagsBits.SendMessages)) {
    console.log('Member can send messages in this channel');
}
```

**Set Permissions:**
```typescript
// Role permissions
await role.setPermissions([
    PermissionFlagsBits.ViewChannel,
    PermissionFlagsBits.SendMessages
]);

// Channel permission overwrites
await channel.permissionOverwrites.create(role, {
    ViewChannel: true,
    SendMessages: true,
    AttachFiles: false
});

await channel.permissionOverwrites.create(member, {
    ViewChannel: true,
    SendMessages: false
});
```

---

### **7. COLLECTORS**

#### **Message Collector**

```typescript
// Collect messages
const filter = m => m.author.id === message.author.id;
const collector = channel.createMessageCollector({
    filter,
    time: 60000, // 1 minute
    max: 5 // Max 5 messages
});

collector.on('collect', message => {
    console.log(`Collected: ${message.content}`);
});

collector.on('end', collected => {
    console.log(`Collected ${collected.size} messages`);
});
```

#### **Reaction Collector**

```typescript
// Collect reactions
const filter = (reaction, user) => {
    return reaction.emoji.name === '👍' && user.id === message.author.id;
};

const collector = message.createReactionCollector({
    filter,
    time: 60000,
    max: 1
});

collector.on('collect', (reaction, user) => {
    console.log(`${user.tag} reacted with ${reaction.emoji.name}`);
});

collector.on('end', collected => {
    console.log(`Collected ${collected.size} reactions`);
});
```

#### **Interaction Collector**

```typescript
// Collect button clicks
const filter = i => i.customId === 'button_id' && i.user.id === message.author.id;

const collector = message.createMessageComponentCollector({
    filter,
    time: 60000
});

collector.on('collect', async interaction => {
    await interaction.reply('Button clicked!');
});

collector.on('end', collected => {
    console.log(`Collected ${collected.size} interactions`);
});
```

---

### **8. ADVANCED FEATURES**

#### **Webhooks**

```typescript
// Create webhook
const webhook = await channel.createWebhook({
    name: 'My Webhook',
    avatar: 'https://example.com/avatar.png'
});

// Send via webhook
await webhook.send({
    content: 'Hello from webhook!',
    username: 'Custom Name',
    avatarURL: 'https://example.com/avatar.png'
});

// Fetch webhook
const webhook = await client.fetchWebhook('webhook_id');

// Delete webhook
await webhook.delete();
```

---

#### **Embeds (using EmbedBuilder)**

```typescript
import { EmbedBuilder } from 'discord.js';

const embed = new EmbedBuilder()
    .setTitle('Title')
    .setDescription('Description')
    .setColor('Blue')
    .addFields(
        { name: 'Field 1', value: 'Value 1', inline: true },
        { name: 'Field 2', value: 'Value 2', inline: true }
    )
    .setImage('https://example.com/image.png')
    .setThumbnail('https://example.com/thumb.png')
    .setFooter({ text: 'Footer', iconURL: 'https://example.com/icon.png' })
    .setTimestamp();

await channel.send({ embeds: [embed] });
```

---

#### **Buttons & Components**

```typescript
import { ActionRowBuilder, ButtonBuilder, ButtonStyle } from 'discord.js';

const row = new ActionRowBuilder()
    .addComponents(
        new ButtonBuilder()
            .setCustomId('button_1')
            .setLabel('Click Me')
            .setStyle(ButtonStyle.Primary),
        new ButtonBuilder()
            .setLabel('Visit')
            .setURL('https://discord.js.org')
            .setStyle(ButtonStyle.Link)
    );

await channel.send({
    content: 'Choose:',
    components: [row]
});
```

---

#### **Slash Commands**

```typescript
import { SlashCommandBuilder } from 'discord.js';

// Define command
const command = new SlashCommandBuilder()
    .setName('ping')
    .setDescription('Replies with Pong!');

// Register command
await client.application?.commands.create(command);

// Handle command
client.on('interactionCreate', async interaction => {
    if (!interaction.isChatInputCommand()) return;
    
    if (interaction.commandName === 'ping') {
        await interaction.reply('Pong!');
    }
});
```

---

#### **Modals**

```typescript
import { ModalBuilder, TextInputBuilder, TextInputStyle, ActionRowBuilder } from 'discord.js';

// Create modal
const modal = new ModalBuilder()
    .setCustomId('my_modal')
    .setTitle('Feedback Form');

const input = new TextInputBuilder()
    .setCustomId('feedback')
    .setLabel('Your Feedback')
    .setStyle(TextInputStyle.Paragraph)
    .setRequired(true);

const row = new ActionRowBuilder().addComponents(input);
modal.addComponents(row);

// Show modal
await interaction.showModal(modal);

// Handle modal submit
if (interaction.isModalSubmit() && interaction.customId === 'my_modal') {
    const feedback = interaction.fields.getTextInputValue('feedback');
    await interaction.reply(`Thanks for feedback: ${feedback}`);
}
```

---

## 📊 COMMON PATTERNS

### **Command Handler**

```typescript
const commands = new Collection();

// Load commands
commands.set('ping', {
    name: 'ping',
    execute: async (message) => {
        message.reply('Pong!');
    }
});

// Handle commands
client.on('messageCreate', async message => {
    if (!message.content.startsWith('!')) return;
    
    const args = message.content.slice(1).split(' ');
    const commandName = args.shift();
    
    const command = commands.get(commandName);
    if (!command) return;
    
    try {
        await command.execute(message, args);
    } catch (error) {
        console.error(error);
        message.reply('Error executing command!');
    }
});
```

---

### **Error Handling**

```typescript
// Catch all errors
client.on('error', error => {
    console.error('Discord client error:', error);
});

// Handle process errors
process.on('unhandledRejection', error => {
    console.error('Unhandled promise rejection:', error);
});

// Interaction error handling
client.on('interactionCreate', async interaction => {
    try {
        // Your code
    } catch (error) {
        console.error(error);
        
        if (interaction.replied || interaction.deferred) {
            await interaction.followUp({ content: 'Error!', ephemeral: true });
        } else {
            await interaction.reply({ content: 'Error!', ephemeral: true });
        }
    }
});
```

---

### **Presence Management**

```typescript
// Set presence
client.user?.setPresence({
    activities: [{
        name: 'with Discord.js',
        type: ActivityType.Playing
    }],
    status: 'online'
});

// Activity types
enum ActivityType {
    Playing = 0,   // Playing {name}
    Streaming = 1, // Streaming {name}
    Listening = 2, // Listening to {name}
    Watching = 3,  // Watching {name}
    Custom = 4,    // {name}
    Competing = 5  // Competing in {name}
}

// Statuses
type PresenceStatus = 'online' | 'idle' | 'dnd' | 'invisible';
```

---

## 🔧 UTILITIES

### **Colors**

```typescript
import { Colors } from 'discord.js';

Colors.Default   // 0x000000
Colors.White     // 0xFFFFFF
Colors.Aqua      // 0x1ABC9C
Colors.Green     // 0x57F287
Colors.Blue      // 0x3498DB
Colors.Yellow    // 0xFEE75C
Colors.Purple    // 0x9B59B6
Colors.Red       // 0xED4245
Colors.Orange    // 0xE67E22
```

---

### **Time Utilities**

```typescript
import { time, TimestampStyles } from 'discord.js';

// Format timestamp
const timestamp = Date.now();
time(timestamp, TimestampStyles.ShortTime);      // 3:20 PM
time(timestamp, TimestampStyles.LongTime);       // 3:20:00 PM
time(timestamp, TimestampStyles.ShortDate);      // 01/09/2026
time(timestamp, TimestampStyles.LongDate);       // January 9, 2026
time(timestamp, TimestampStyles.ShortDateTime);  // January 9, 2026 3:20 PM
time(timestamp, TimestampStyles.LongDateTime);   // Thursday, January 9, 2026 3:20 PM
time(timestamp, TimestampStyles.RelativeTime);   // 2 hours ago
```

---

### **Mentions**

```typescript
import { userMention, channelMention, roleMention } from 'discord.js';

// Format mentions
userMention('123456789')    // <@123456789>
channelMention('123456789') // <#123456789>
roleMention('123456789')    // <@&123456789>
```

---

## 📚 BEST PRACTICES

### **1. Use Intents Wisely**

Only request intents you need to minimize data usage and comply with Discord policies.

### **2. Handle Rate Limits**

Discord.js handles rate limits automatically, but avoid unnecessary API calls.

### **3. Cache Management**

```typescript
// Clear cache periodically
setInterval(() => {
    client.guilds.cache.sweep(guild => guild.available);
    client.channels.cache.sweep(channel => !channel.isTextBased());
}, 3600000); // Every hour
```

### **4. Use Partials for Uncached Data**

```typescript
const client = new Client({
    intents: [...],
    partials: [
        Partials.Message,
        Partials.Channel,
        Partials.Reaction,
        Partials.GuildMember,
        Partials.User
    ]
});
```

### **5. Proper Error Handling**

Always wrap async operations in try-catch blocks.

---

## 🔗 RESOURCES

- **Documentation:** https://discord.js.org/
- **Guide:** https://discordjs.guide/
- **GitHub:** https://github.com/discordjs/discord.js
- **Discord Server:** https://discord.gg/djs

---

**Created:** 2025-01-09  
**Version:** Discord.js v14+  
**Last Updated:** 2025-01-09
