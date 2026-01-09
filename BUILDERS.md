# 📚 DISCORD.JS BUILDERS - COMPLETE DOCUMENTATION

**File:** `@discordjs/builders` Type Definitions  
**Version:** Latest  
**Purpose:** Type-safe builders for creating Discord API-compatible structures

---

## 🎯 MAIN FUNCTIONS

This library is used to **build** various Discord components in a **type-safe** and **API-compatible** way.

---

## 📋 COMPLETE CONTENTS

### 1. 🎨 EMBED BUILDERS

#### **EmbedBuilder**

**Function:** Create embeds (colored messages with formatting)

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `setTitle()` | `string \| null` | Set judul embed |
| `setDescription()` | `string \| null` | Set deskripsi embed |
| `setColor()` | `number \| RGBTuple \| null` | Set warna embed |
| `addFields()` | `...APIEmbedField[]` | Tambah fields (max 25) |
| `setFields()` | `...APIEmbedField[]` | Set/replace all fields |
| `spliceFields()` | `index, deleteCount, ...fields` | Modify fields array |
| `setAuthor()` | `EmbedAuthorOptions \| null` | Set author section |
| `setFooter()` | `EmbedFooterOptions \| null` | Set footer section |
| `setImage()` | `string \| null` | Set image URL |
| `setThumbnail()` | `string \| null` | Set thumbnail URL |
| `setTimestamp()` | `Date \| number \| null` | Set timestamp |
| `setURL()` | `string \| null` | Set embed URL |
| `toJSON()` | - | Serialize to API data |

**Example:**
```typescript
const embed = new EmbedBuilder()
    .setTitle("🎉 Welcome!")
    .setDescription("Welcome to our server!")
    .setColor(Colors.Blue)
    .addFields(
        { name: "Field 1", value: "Value 1", inline: true },
        { name: "Field 2", value: "Value 2", inline: true }
    )
    .setAuthor({ 
        name: "Bot Name", 
        iconURL: "https://..." 
    })
    .setFooter({ 
        text: "Footer text", 
        iconURL: "https://..." 
    })
    .setThumbnail("https://...")
    .setImage("https://...")
    .setTimestamp()
    .setURL("https://...");
```

**Types:**
```typescript
interface EmbedAuthorData {
    name: string;
    url?: string;
    iconURL?: string;
    proxyIconURL?: string;
}

interface EmbedFooterData {
    text: string;
    iconURL?: string;
    proxyIconURL?: string;
}

type RGBTuple = [red: number, green: number, blue: number];
```

**Limits:**
- Title: Max 256 characters
- Description: Max 4096 characters
- Fields: Max 25 fields
- Field name: Max 256 characters
- Field value: Max 1024 characters
- Footer text: Max 2048 characters
- Author name: Max 256 characters
- Total characters: Max 6000 characters

---

### 2. 🎮 COMPONENT BUILDERS

#### **A. ButtonBuilder**

**Function:** Create interactive buttons

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `setCustomId()` | `string` | Set custom ID (for non-link buttons) |
| `setLabel()` | `string` | Set button label text |
| `setStyle()` | `ButtonStyle` | Set button style |
| `setEmoji()` | `APIMessageComponentEmoji` | Set button emoji |
| `setDisabled()` | `boolean` | Set disabled state |
| `setURL()` | `string` | Set URL (for link buttons) |
| `setSKUId()` | `Snowflake` | Set SKU ID (for premium buttons) |
| `toJSON()` | - | Serialize to API data |

**Button Styles:**
```typescript
enum ButtonStyle {
    Primary = 1,    // Blurple (Discord blue)
    Secondary = 2,  // Gray
    Success = 3,    // Green
    Danger = 4,     // Red
    Link = 5,       // Gray with link
    Premium = 6     // Purple (premium)
}
```

**Examples:**

```typescript
// Primary Button (with custom ID)
const button1 = new ButtonBuilder()
    .setCustomId('primary_button')
    .setLabel('Click Me')
    .setStyle(ButtonStyle.Primary)
    .setEmoji({ name: '✨' });

// Link Button (with URL)
const button2 = new ButtonBuilder()
    .setLabel('Visit Website')
    .setStyle(ButtonStyle.Link)
    .setURL('https://discord.com');

// Disabled Button
const button3 = new ButtonBuilder()
    .setCustomId('disabled_button')
    .setLabel('Disabled')
    .setStyle(ButtonStyle.Secondary)
    .setDisabled(true);
```

**Limits:**
- Label: Max 80 characters
- Custom ID: Max 100 characters
- Max 5 buttons per action row

---

#### **B. Select Menu Builders**

##### **StringSelectMenuBuilder**

**Function:** Dropdown menu with string options

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `setCustomId()` | `string` | Set custom ID |
| `setPlaceholder()` | `string` | Set placeholder text |
| `setMinValues()` | `number` | Set minimum selections |
| `setMaxValues()` | `number` | Set maximum selections |
| `setDisabled()` | `boolean` | Set disabled state |
| `setRequired()` | `boolean` | Set required (modals only) |
| `addOptions()` | `...StringSelectMenuOptionBuilder[]` | Add options |
| `setOptions()` | `...StringSelectMenuOptionBuilder[]` | Set/replace options |
| `spliceOptions()` | `index, deleteCount, ...options` | Modify options |
| `toJSON()` | - | Serialize to API data |

**Example:**
```typescript
const select = new StringSelectMenuBuilder()
    .setCustomId('color_select')
    .setPlaceholder('Choose a color')
    .setMinValues(1)
    .setMaxValues(3)
    .addOptions(
        new StringSelectMenuOptionBuilder()
            .setLabel('Red')
            .setValue('red')
            .setDescription('The color red')
            .setEmoji({ name: '🔴' }),
        new StringSelectMenuOptionBuilder()
            .setLabel('Blue')
            .setValue('blue')
            .setDescription('The color blue')
            .setEmoji({ name: '🔵' })
            .setDefault(true),
        new StringSelectMenuOptionBuilder()
            .setLabel('Green')
            .setValue('green')
            .setDescription('The color green')
            .setEmoji({ name: '🟢' })
    );
```

##### **UserSelectMenuBuilder**

**Function:** Select menu for choosing users

**Example:**
```typescript
const userSelect = new UserSelectMenuBuilder()
    .setCustomId('user_select')
    .setPlaceholder('Select users')
    .setMinValues(1)
    .setMaxValues(5)
    .addDefaultUsers('123456789', '987654321'); // Pre-selected users
```

##### **RoleSelectMenuBuilder**

**Function:** Select menu for choosing roles

**Example:**
```typescript
const roleSelect = new RoleSelectMenuBuilder()
    .setCustomId('role_select')
    .setPlaceholder('Select roles')
    .addDefaultRoles('123456789', '987654321'); // Pre-selected roles
```

##### **ChannelSelectMenuBuilder**

**Function:** Select menu for choosing channels

**Example:**
```typescript
const channelSelect = new ChannelSelectMenuBuilder()
    .setCustomId('channel_select')
    .setPlaceholder('Select a channel')
    .addChannelTypes(ChannelType.GuildText, ChannelType.GuildVoice)
    .addDefaultChannels('123456789'); // Pre-selected channel
```

##### **MentionableSelectMenuBuilder**

**Function:** Select menu for choosing users or roles

**Example:**
```typescript
const mentionableSelect = new MentionableSelectMenuBuilder()
    .setCustomId('mentionable_select')
    .setPlaceholder('Select users or roles')
    .addDefaultUsers('123456789')
    .addDefaultRoles('987654321');
```

**Limits (All Select Menus):**
- Placeholder: Max 150 characters
- Custom ID: Max 100 characters
- Options: Min 1, Max 25 (String Select only)
- Min/Max Values: 0-25

---

#### **C. TextInputBuilder**

**Function:** Text input field (for modals)

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `setCustomId()` | `string` | Set custom ID |
| `setLabel()` | `string` | Set label text |
| `setStyle()` | `TextInputStyle` | Set input style |
| `setPlaceholder()` | `string` | Set placeholder |
| `setMinLength()` | `number` | Set minimum length |
| `setMaxLength()` | `number` | Set maximum length |
| `setValue()` | `string` | Set default value |
| `setRequired()` | `boolean` | Set required |
| `toJSON()` | - | Serialize to API data |

**Text Input Styles:**
```typescript
enum TextInputStyle {
    Short = 1,      // Single line
    Paragraph = 2   // Multi-line
}
```

**Example:**
```typescript
// Short input
const shortInput = new TextInputBuilder()
    .setCustomId('username_input')
    .setLabel('Username')
    .setStyle(TextInputStyle.Short)
    .setPlaceholder('Enter your username')
    .setMinLength(3)
    .setMaxLength(20)
    .setRequired(true);

// Paragraph input
const paragraphInput = new TextInputBuilder()
    .setCustomId('bio_input')
    .setLabel('Tell us about yourself')
    .setStyle(TextInputStyle.Paragraph)
    .setPlaceholder('Write a short bio...')
    .setMaxLength(500)
    .setRequired(false)
    .setValue('Default bio text');
```

**Limits:**
- Label: Max 45 characters
- Custom ID: Max 100 characters
- Placeholder: Max 100 characters
- Min length: 0-4000
- Max length: 1-4000
- Value: Max 4000 characters

---

#### **D. ActionRowBuilder**

**Function:** Container for components

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `addComponents()` | `...ComponentType[]` | Add components |
| `setComponents()` | `...ComponentType[]` | Set/replace components |
| `toJSON()` | - | Serialize to API data |

**Example:**
```typescript
// Action row with buttons
const row1 = new ActionRowBuilder<ButtonBuilder>()
    .addComponents(button1, button2, button3);

// Action row with select menu
const row2 = new ActionRowBuilder<StringSelectMenuBuilder>()
    .addComponents(selectMenu);

// Action row with text input (for modals)
const row3 = new ActionRowBuilder<TextInputBuilder>()
    .addComponents(textInput);

// Multiple action rows
await interaction.reply({
    content: 'Choose an option',
    components: [row1, row2]
});
```

**Limits:**
- Max 5 action rows per message
- Max 5 buttons per action row
- Max 1 select menu per action row
- Max 1 text input per action row (modals)

---

### 3. 🪟 MODAL BUILDERS

#### **ModalBuilder**

**Function:** Create modal popup (form dialog)

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `setTitle()` | `string` | Set modal title |
| `setCustomId()` | `string` | Set custom ID |
| `addComponents()` | `...ActionRowBuilder[]` | Add action rows |
| `setComponents()` | `...ActionRowBuilder[]` | Set action rows |
| `toJSON()` | - | Serialize to API data |

**Example:**
```typescript
const modal = new ModalBuilder()
    .setCustomId('feedback_modal')
    .setTitle('Feedback Form')
    .addComponents(
        new ActionRowBuilder<TextInputBuilder>().addComponents(
            new TextInputBuilder()
                .setCustomId('name_input')
                .setLabel('Your Name')
                .setStyle(TextInputStyle.Short)
                .setRequired(true)
        ),
        new ActionRowBuilder<TextInputBuilder>().addComponents(
            new TextInputBuilder()
                .setCustomId('feedback_input')
                .setLabel('Your Feedback')
                .setStyle(TextInputStyle.Paragraph)
                .setPlaceholder('Tell us what you think...')
                .setRequired(true)
        )
    );

// Show modal
await interaction.showModal(modal);
```

**Limits:**
- Title: Max 45 characters
- Custom ID: Max 100 characters
- Max 5 action rows (each with 1 text input)

---

### 4. ⚔️ SLASH COMMAND BUILDERS

#### **SlashCommandBuilder**

**Function:** Create slash commands

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `setName()` | `string` | Set command name |
| `setDescription()` | `string` | Set description |
| `setNameLocalization()` | `locale, name` | Set name translation |
| `setDescriptionLocalization()` | `locale, description` | Set description translation |
| `setDefaultMemberPermissions()` | `Permissions` | Set required permissions |
| `setDMPermission()` | `boolean` | Allow in DMs (deprecated) |
| `setContexts()` | `...InteractionContextType[]` | Set contexts |
| `setIntegrationTypes()` | `...ApplicationIntegrationType[]` | Set integration types |
| `setNSFW()` | `boolean` | Set NSFW |
| `addStringOption()` | `fn` | Add string option |
| `addIntegerOption()` | `fn` | Add integer option |
| `addBooleanOption()` | `fn` | Add boolean option |
| `addUserOption()` | `fn` | Add user option |
| `addRoleOption()` | `fn` | Add role option |
| `addChannelOption()` | `fn` | Add channel option |
| `addMentionableOption()` | `fn` | Add mentionable option |
| `addNumberOption()` | `fn` | Add number option |
| `addAttachmentOption()` | `fn` | Add attachment option |
| `addSubcommand()` | `fn` | Add subcommand |
| `addSubcommandGroup()` | `fn` | Add subcommand group |
| `toJSON()` | - | Serialize to API data |

**Example:**
```typescript
const command = new SlashCommandBuilder()
    .setName('ban')
    .setDescription('Ban a user from the server')
    .setDefaultMemberPermissions(PermissionFlagsBits.BanMembers)
    .setContexts(InteractionContextType.Guild)
    .setNSFW(false)
    .addUserOption(option =>
        option
            .setName('target')
            .setDescription('User to ban')
            .setRequired(true)
    )
    .addStringOption(option =>
        option
            .setName('reason')
            .setDescription('Ban reason')
            .setRequired(false)
            .setMaxLength(500)
    )
    .addIntegerOption(option =>
        option
            .setName('days')
            .setDescription('Days of messages to delete')
            .setRequired(false)
            .setMinValue(0)
            .setMaxValue(7)
            .addChoices(
                { name: 'None', value: 0 },
                { name: '1 Day', value: 1 },
                { name: '7 Days', value: 7 }
            )
    );
```

**Limits:**
- Name: 1-32 characters (lowercase, no spaces)
- Description: 1-100 characters
- Max 25 options per command
- Max 25 subcommands per group
- Max 25 subcommand groups per command

---

#### **Command Options:**

##### **SlashCommandStringOption**

**Methods:**
- `setName()`, `setDescription()`, `setRequired()`
- `setMinLength()`, `setMaxLength()`
- `addChoices()` - Add predefined choices
- `setAutocomplete()` - Enable autocomplete

**Example:**
```typescript
.addStringOption(option =>
    option
        .setName('color')
        .setDescription('Choose a color')
        .setRequired(true)
        .addChoices(
            { name: 'Red', value: 'red' },
            { name: 'Blue', value: 'blue' },
            { name: 'Green', value: 'green' }
        )
)
```

##### **SlashCommandIntegerOption**

**Methods:**
- `setName()`, `setDescription()`, `setRequired()`
- `setMinValue()`, `setMaxValue()`
- `addChoices()` - Add predefined choices
- `setAutocomplete()` - Enable autocomplete

**Example:**
```typescript
.addIntegerOption(option =>
    option
        .setName('level')
        .setDescription('Character level')
        .setRequired(true)
        .setMinValue(1)
        .setMaxValue(100)
)
```

##### **SlashCommandNumberOption**

**Similar to Integer but allows decimals**

**Example:**
```typescript
.addNumberOption(option =>
    option
        .setName('percentage')
        .setDescription('Enter percentage')
        .setRequired(true)
        .setMinValue(0.0)
        .setMaxValue(100.0)
)
```

##### **SlashCommandBooleanOption**

**Example:**
```typescript
.addBooleanOption(option =>
    option
        .setName('public')
        .setDescription('Make response public')
        .setRequired(false)
)
```

##### **SlashCommandUserOption**

**Example:**
```typescript
.addUserOption(option =>
    option
        .setName('target')
        .setDescription('Target user')
        .setRequired(true)
)
```

##### **SlashCommandRoleOption**

**Example:**
```typescript
.addRoleOption(option =>
    option
        .setName('role')
        .setDescription('Target role')
        .setRequired(true)
)
```

##### **SlashCommandChannelOption**

**Methods:**
- `addChannelTypes()` - Restrict channel types

**Example:**
```typescript
.addChannelOption(option =>
    option
        .setName('channel')
        .setDescription('Target channel')
        .setRequired(true)
        .addChannelTypes(
            ChannelType.GuildText,
            ChannelType.GuildVoice
        )
)
```

##### **SlashCommandMentionableOption**

**Can select users OR roles**

**Example:**
```typescript
.addMentionableOption(option =>
    option
        .setName('target')
        .setDescription('User or role')
        .setRequired(true)
)
```

##### **SlashCommandAttachmentOption**

**Example:**
```typescript
.addAttachmentOption(option =>
    option
        .setName('file')
        .setDescription('Upload a file')
        .setRequired(false)
)
```

---

#### **Subcommands:**

##### **SlashCommandSubcommandBuilder**

**Example:**
```typescript
const command = new SlashCommandBuilder()
    .setName('config')
    .setDescription('Configuration commands')
    .addSubcommand(subcommand =>
        subcommand
            .setName('set')
            .setDescription('Set configuration')
            .addStringOption(option =>
                option
                    .setName('key')
                    .setDescription('Config key')
                    .setRequired(true)
            )
            .addStringOption(option =>
                option
                    .setName('value')
                    .setDescription('Config value')
                    .setRequired(true)
            )
    )
    .addSubcommand(subcommand =>
        subcommand
            .setName('get')
            .setDescription('Get configuration')
            .addStringOption(option =>
                option
                    .setName('key')
                    .setDescription('Config key')
                    .setRequired(true)
            )
    );

// Usage: /config set key:prefix value:!
// Usage: /config get key:prefix
```

##### **SlashCommandSubcommandGroupBuilder**

**Example:**
```typescript
const command = new SlashCommandBuilder()
    .setName('settings')
    .setDescription('Settings commands')
    .addSubcommandGroup(group =>
        group
            .setName('user')
            .setDescription('User settings')
            .addSubcommand(subcommand =>
                subcommand
                    .setName('profile')
                    .setDescription('Edit profile')
            )
            .addSubcommand(subcommand =>
                subcommand
                    .setName('privacy')
                    .setDescription('Privacy settings')
            )
    )
    .addSubcommandGroup(group =>
        group
            .setName('server')
            .setDescription('Server settings')
            .addSubcommand(subcommand =>
                subcommand
                    .setName('moderation')
                    .setDescription('Moderation settings')
            )
    );

// Usage: /settings user profile
// Usage: /settings server moderation
```

---

### 5. 📱 CONTEXT MENU BUILDERS

#### **ContextMenuCommandBuilder**

**Function:** Create context menu commands (right-click menu)

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `setName()` | `string` | Set command name |
| `setType()` | `ContextMenuCommandType` | Set type (User/Message) |
| `setNameLocalization()` | `locale, name` | Set name translation |
| `setDefaultMemberPermissions()` | `Permissions` | Set required permissions |
| `setDMPermission()` | `boolean` | Allow in DMs (deprecated) |
| `setContexts()` | `...InteractionContextType[]` | Set contexts |
| `setIntegrationTypes()` | `...ApplicationIntegrationType[]` | Set integration types |
| `toJSON()` | - | Serialize to API data |

**Context Menu Types:**
```typescript
enum ApplicationCommandType {
    User = 2,     // Right-click on user
    Message = 3   // Right-click on message
}
```

**Example:**
```typescript
// User context menu
const userCommand = new ContextMenuCommandBuilder()
    .setName('User Info')
    .setType(ApplicationCommandType.User)
    .setDefaultMemberPermissions(PermissionFlagsBits.ModerateMembers);

// Message context menu
const messageCommand = new ContextMenuCommandBuilder()
    .setName('Report Message')
    .setType(ApplicationCommandType.Message);
```

---

### 6. 🎨 COMPONENTS V2 (NEW)

#### **ContainerBuilder**

**Function:** Container wrapper for components

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `setAccentColor()` | `number \| RGBTuple` | Set accent color |
| `clearAccentColor()` | - | Clear accent color |
| `setSpoiler()` | `boolean` | Set spoiler |
| `addActionRowComponents()` | `...ActionRowBuilder[]` | Add action rows |
| `addFileComponents()` | `...FileBuilder[]` | Add files |
| `addMediaGalleryComponents()` | `...MediaGalleryBuilder[]` | Add galleries |
| `addSectionComponents()` | `...SectionBuilder[]` | Add sections |
| `addSeparatorComponents()` | `...SeparatorBuilder[]` | Add separators |
| `addTextDisplayComponents()` | `...TextDisplayBuilder[]` | Add text displays |
| `spliceComponents()` | `index, deleteCount, ...components` | Modify components |
| `toJSON()` | - | Serialize to API data |

**Example:**
```typescript
const container = new ContainerBuilder()
    .setAccentColor([255, 0, 0]) // Red
    .addTextDisplayComponents(td =>
        td.setContent("# Welcome!\nThis is a container")
    )
    .addSeparatorComponents(sep =>
        sep.setSpacing(SeparatorSpacingSize.Small)
            .setDivider(true)
    )
    .addActionRowComponents(
        new ActionRowBuilder<ButtonBuilder>()
            .addComponents(button1, button2)
    );
```

---

#### **SeparatorBuilder**

**Function:** Line separator

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `setDivider()` | `boolean` | Show divider line |
| `setSpacing()` | `SeparatorSpacingSize` | Set spacing size |
| `clearSpacing()` | - | Clear spacing |
| `toJSON()` | - | Serialize to API data |

**Spacing Sizes:**
```typescript
enum SeparatorSpacingSize {
    Small = 0,
    Large = 1
}
```

**Example:**
```typescript
const separator = new SeparatorBuilder()
    .setDivider(true)
    .setSpacing(SeparatorSpacingSize.Large);
```

---

#### **TextDisplayBuilder**

**Function:** Display formatted text/markdown

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `setContent()` | `string` | Set markdown content |
| `toJSON()` | - | Serialize to API data |

**Example:**
```typescript
const textDisplay = new TextDisplayBuilder()
    .setContent("# Heading\n\nSome **bold** and *italic* text");
```

---

#### **SectionBuilder**

**Function:** Section with text and accessory

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `setButtonAccessory()` | `ButtonBuilder` | Set button accessory |
| `setThumbnailAccessory()` | `ThumbnailBuilder` | Set thumbnail accessory |
| `addTextDisplayComponents()` | `...TextDisplayBuilder[]` | Add text displays |
| `spliceTextDisplayComponents()` | `index, deleteCount, ...components` | Modify text displays |
| `toJSON()` | - | Serialize to API data |

**Example:**
```typescript
const section = new SectionBuilder()
    .addTextDisplayComponents(
        new TextDisplayBuilder()
            .setContent("## Section Title\nSection content")
    )
    .setButtonAccessory(
        new ButtonBuilder()
            .setCustomId('section_button')
            .setLabel('Action')
            .setStyle(ButtonStyle.Primary)
    );
```

---

#### **MediaGalleryBuilder**

**Function:** Image/media gallery

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `addItems()` | `...MediaGalleryItemBuilder[]` | Add items |
| `spliceItems()` | `index, deleteCount, ...items` | Modify items |
| `toJSON()` | - | Serialize to API data |

**Example:**
```typescript
const gallery = new MediaGalleryBuilder()
    .addItems(
        new MediaGalleryItemBuilder()
            .setURL('https://example.com/image1.png')
            .setDescription('Image 1')
            .setSpoiler(false),
        new MediaGalleryItemBuilder()
            .setURL('https://example.com/image2.png')
            .setDescription('Image 2')
    );
```

---

#### **FileBuilder**

**Function:** File attachment component

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `setURL()` | `string` | Set file URL |
| `setSpoiler()` | `boolean` | Set spoiler |
| `toJSON()` | - | Serialize to API data |

**Example:**
```typescript
const file = new FileBuilder()
    .setURL('attachment://file.pdf')
    .setSpoiler(false);
```

---

#### **ThumbnailBuilder**

**Function:** Thumbnail image

**Available Methods:**

| Method | Parameter | Description |
|--------|-----------|-------------|
| `setURL()` | `string` | Set image URL |
| `setDescription()` | `string` | Set alt text |
| `clearDescription()` | - | Clear description |
| `setSpoiler()` | `boolean` | Set spoiler |
| `toJSON()` | - | Serialize to API data |

**Example:**
```typescript
const thumbnail = new ThumbnailBuilder()
    .setURL('https://example.com/thumb.png')
    .setDescription('Thumbnail alt text')
    .setSpoiler(false);
```

---

#### **LabelBuilder & FileUploadBuilder**

**LabelBuilder:** Label for form components  
**FileUploadBuilder:** File upload input

**Example:**
```typescript
const label = new LabelBuilder()
    .setLabel('Upload Image')
    .setDescription('Optional description')
    .setFileUploadComponent(
        new FileUploadBuilder()
            .setCustomId('image_upload')
            .setMinValues(1)
            .setMaxValues(5)
            .setRequired(true)
    );
```

---

## 🔧 UTILITY FUNCTIONS

### **createComponentBuilder()**

Factory function untuk membuat components dari API data

**Example:**
```typescript
const component = createComponentBuilder(apiData);
// Returns appropriate builder based on component type
```

---

### **normalizeArray()**

Normalize variadic/array parameters

**Example:**
```typescript
function addItems(...items: RestOrArray<Item>) {
    const normalized = normalizeArray(items);
    // Always returns Item[]
}
```

---

### **resolveBuilder()**

Resolve builder function or instance

**Example:**
```typescript
const builder = resolveBuilder(
    (b) => b.setName('test'),
    ButtonBuilder
);
```

---

### **embedLength()**

Calculate total embed length

**Example:**
```typescript
const embed = new EmbedBuilder()
    .setTitle('Title')
    .setDescription('Description');

const length = embedLength(embed.toJSON());
// Returns: total character count
```

---

### **Validators:**

```typescript
// Enable/disable validation
enableValidators();
disableValidators();
isValidationEnabled();

// Individual validators
validateName(name);
validateDescription(description);
validateRequired(required);
validateChoicesLength(count, existing);
```

---

## 📊 USAGE PATTERNS

### **Pattern 1: Simple Message with Embed**

```typescript
const embed = new EmbedBuilder()
    .setTitle('Title')
    .setDescription('Description')
    .setColor(Colors.Blue);

await interaction.reply({ embeds: [embed] });
```

---

### **Pattern 2: Message with Buttons**

```typescript
const button1 = new ButtonBuilder()
    .setCustomId('accept')
    .setLabel('Accept')
    .setStyle(ButtonStyle.Success);

const button2 = new ButtonBuilder()
    .setCustomId('decline')
    .setLabel('Decline')
    .setStyle(ButtonStyle.Danger);

const row = new ActionRowBuilder<ButtonBuilder>()
    .addComponents(button1, button2);

await interaction.reply({
    content: 'Do you accept?',
    components: [row]
});
```

---

### **Pattern 3: Modal Form**

```typescript
const modal = new ModalBuilder()
    .setCustomId('feedback_modal')
    .setTitle('Feedback')
    .addComponents(
        new ActionRowBuilder<TextInputBuilder>().addComponents(
            new TextInputBuilder()
                .setCustomId('name')
                .setLabel('Name')
                .setStyle(TextInputStyle.Short)
                .setRequired(true)
        ),
        new ActionRowBuilder<TextInputBuilder>().addComponents(
            new TextInputBuilder()
                .setCustomId('message')
                .setLabel('Message')
                .setStyle(TextInputStyle.Paragraph)
                .setRequired(true)
        )
    );

await interaction.showModal(modal);
```

---

### **Pattern 4: Slash Command with Options**

```typescript
const command = new SlashCommandBuilder()
    .setName('ban')
    .setDescription('Ban a user')
    .addUserOption(option =>
        option
            .setName('user')
            .setDescription('User to ban')
            .setRequired(true)
    )
    .addStringOption(option =>
        option
            .setName('reason')
            .setDescription('Reason')
            .setRequired(false)
    );

// Register
await client.application?.commands.create(command.toJSON());
```

---

### **Pattern 5: Complex Layout (Components V2)**

```typescript
const container = new ContainerBuilder()
    .setAccentColor(Colors.Blue)
    .addTextDisplayComponents(td =>
        td.setContent('# Header\nContent here')
    )
    .addSeparatorComponents(sep =>
        sep.setDivider(true)
    )
    .addSectionComponents(section =>
        section
            .addTextDisplayComponents(td =>
                td.setContent('Section text')
            )
            .setButtonAccessory(
                new ButtonBuilder()
                    .setCustomId('btn')
                    .setLabel('Click')
                    .setStyle(ButtonStyle.Primary)
            )
    );

await interaction.reply({
    components: [container],
    flags: MessageFlags.IsComponentsV2
});
```

---

## ⚠️ IMPORTANT LIMITS

### **Message Limits:**
- Max 10 embeds per message
- Max 2000 characters content
- Max 6000 characters total per embed
- Max 5 action rows per message

### **Embed Limits:**
- Title: 256 chars
- Description: 4096 chars
- Fields: 25 max
- Field name: 256 chars
- Field value: 1024 chars
- Footer: 2048 chars
- Author: 256 chars

### **Component Limits:**
- Custom ID: 100 chars
- Button label: 80 chars
- Select placeholder: 150 chars
- Select options: 25 max
- Buttons per row: 5 max
- Text input label: 45 chars

### **Command Limits:**
- Command name: 1-32 chars (lowercase, no spaces)
- Description: 1-100 chars
- Options: 25 max
- Subcommands: 25 max per group
- Subcommand groups: 25 max

---

## 💡 BEST PRACTICES

### **1. Always validate before sending:**
```typescript
const embed = new EmbedBuilder()
    .setTitle('Title')
    .setDescription('Description')
    .toJSON(); // This validates!
```

### **2. Use type inference:**
```typescript
const row = new ActionRowBuilder<ButtonBuilder>() // Type-safe!
    .addComponents(button1, button2);
```

### **3. Handle errors:**
```typescript
try {
    const command = new SlashCommandBuilder()
        .setName('invalid name') // Will throw!
        .toJSON();
} catch (error) {
    console.error('Invalid command:', error);
}
```

### **4. Reuse builders:**
```typescript
const baseEmbed = new EmbedBuilder()
    .setColor(Colors.Blue)
    .setFooter({ text: 'My Bot' });

// Clone and modify
const embed1 = EmbedBuilder.from(baseEmbed.toJSON())
    .setTitle('Title 1');

const embed2 = EmbedBuilder.from(baseEmbed.toJSON())
    .setTitle('Title 2');
```

### **5. Use helper functions:**
```typescript
function createSuccessEmbed(message: string): EmbedBuilder {
    return new EmbedBuilder()
        .setColor(Colors.Green)
        .setDescription(`✅ ${message}`);
}

function createErrorEmbed(message: string): EmbedBuilder {
    return new EmbedBuilder()
        .setColor(Colors.Red)
        .setDescription(`❌ ${message}`);
}
```

---

## 🎯 QUICK REFERENCE

### **Common Imports:**
```typescript
import {
    // Embeds
    EmbedBuilder,
    
    // Components
    ButtonBuilder,
    ActionRowBuilder,
    StringSelectMenuBuilder,
    StringSelectMenuOptionBuilder,
    TextInputBuilder,
    ModalBuilder,
    
    // Commands
    SlashCommandBuilder,
    ContextMenuCommandBuilder,
    
    // Components V2
    ContainerBuilder,
    SeparatorBuilder,
    TextDisplayBuilder,
    
    // Enums
    ButtonStyle,
    TextInputStyle,
    ApplicationCommandType,
    ChannelType,
    
    // Types
    Colors,
    PermissionFlagsBits,
    MessageFlags
} from 'discord.js';
```

### **Common Colors:**
```typescript
Colors.Default  // 0x000000
Colors.White    // 0xFFFFFF
Colors.Aqua     // 0x1ABC9C
Colors.Green    // 0x57F287
Colors.Blue     // 0x3498DB
Colors.Yellow   // 0xFEE75C
Colors.Purple   // 0x9B59B6
Colors.Red      // 0xED4245
Colors.Orange   // 0xE67E22
Colors.Fuchsia  // 0xEB459E
```

---

## 📚 RESOURCES

- **Discord.js Docs:** https://discord.js.org/
- **Discord API Docs:** https://discord.com/developers/docs
- **Builders GitHub:** https://github.com/discordjs/discord.js/tree/main/packages/builders

---

**Created:** 2025-01-09  
**Version:** Latest Discord.js v14+  
**Type Definitions:** @discordjs/builders

---

## 🏁 CONCLUSION

The `@discordjs/builders` library provides:

✅ **Type-safe** builders for Discord components  
✅ **Validated** data before sending to API  
✅ **Easy-to-use** chainable methods  
✅ **Complete** coverage of Discord features  
✅ **Auto-complete** in IDE  
✅ **Error prevention** through TypeScript

**Use it for:** Everything Discord bot UI/UX! 🚀
