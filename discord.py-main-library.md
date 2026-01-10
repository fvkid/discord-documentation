# 📚 discord.py - PYTHON DISCORD LIBRARY DOCUMENTATION

**Package:** `discord.py`  
**Version:** v2.3+  
**Purpose:** Complete Discord bot framework for Python

---

## 🎯 OVERVIEW

Discord.py is a modern, easy-to-use, feature-rich, and async-ready API wrapper for Discord written in Python. It provides a clean and pythonic interface to interact with the Discord API.

---

## 📦 INSTALLATION

```bash
# Stable version
pip install discord.py

# With voice support
pip install discord.py[voice]

# Development version
pip install git+https://github.com/Rapptz/discord.py
```

---

## 🚀 BASIC SETUP

### **Simple Bot**

```python
import discord
from discord.ext import commands

# Create bot instance
bot = commands.Bot(command_prefix='!', intents=discord.Intents.default())

@bot.event
async def on_ready():
    print(f'Logged in as {bot.user.name} ({bot.user.id})')

@bot.command()
async def ping(ctx):
    await ctx.send('Pong!')

# Run bot
bot.run('YOUR_BOT_TOKEN')
```

### **With Intents**

```python
import discord
from discord.ext import commands

# Configure intents
intents = discord.Intents.default()
intents.message_content = True
intents.members = True

bot = commands.Bot(command_prefix='!', intents=intents)

@bot.event
async def on_ready():
    print(f'Logged in as {bot.user}')

bot.run('YOUR_BOT_TOKEN')
```

---

## 📋 MAIN COMPONENTS

### **1. CLIENT & BOT**

#### **discord.Client**

Base class for Discord bots.

```python
import discord

client = discord.Client(intents=discord.Intents.default())

@client.event
async def on_ready():
    print(f'Logged in as {client.user}')

@client.event
async def on_message(message):
    if message.author == client.user:
        return
    
    if message.content == '!ping':
        await message.channel.send('Pong!')

client.run('TOKEN')
```

#### **commands.Bot**

Extended client with command framework.

```python
from discord.ext import commands

bot = commands.Bot(command_prefix='!', intents=discord.Intents.default())

@bot.command()
async def hello(ctx):
    """Says hello"""
    await ctx.send(f'Hello {ctx.author.mention}!')

@bot.command()
async def add(ctx, a: int, b: int):
    """Adds two numbers"""
    await ctx.send(f'{a} + {b} = {a + b}')

bot.run('TOKEN')
```

---

### **2. INTENTS**

Intents define which events your bot receives.

```python
import discord

# All intents (not recommended for production)
intents = discord.Intents.all()

# Default intents
intents = discord.Intents.default()

# Custom intents
intents = discord.Intents.default()
intents.message_content = True      # Privileged
intents.members = True               # Privileged
intents.presences = True             # Privileged
intents.guilds = True
intents.messages = True
intents.reactions = True
intents.voice_states = True

bot = commands.Bot(command_prefix='!', intents=intents)
```

**Privileged Intents** (require approval for 100+ servers):
- `message_content` - Read message content
- `members` - Member join/leave events
- `presences` - Presence updates

---

### **3. EVENTS**

#### **Common Events**

```python
@bot.event
async def on_ready():
    """Called when bot is ready"""
    print(f'Bot is ready: {bot.user}')

@bot.event
async def on_message(message):
    """Called when message is sent"""
    if message.author.bot:
        return
    
    print(f'{message.author}: {message.content}')
    
    # Process commands
    await bot.process_commands(message)

@bot.event
async def on_message_delete(message):
    """Called when message is deleted"""
    print(f'Message deleted: {message.content}')

@bot.event
async def on_message_edit(before, after):
    """Called when message is edited"""
    print(f'Edited: {before.content} -> {after.content}')

@bot.event
async def on_member_join(member):
    """Called when member joins"""
    channel = member.guild.system_channel
    if channel:
        await channel.send(f'Welcome {member.mention}!')

@bot.event
async def on_member_remove(member):
    """Called when member leaves"""
    print(f'{member} left {member.guild.name}')

@bot.event
async def on_member_update(before, after):
    """Called when member is updated"""
    if before.roles != after.roles:
        print(f'{after} roles changed')

@bot.event
async def on_guild_join(guild):
    """Called when bot joins a guild"""
    print(f'Joined guild: {guild.name}')

@bot.event
async def on_guild_remove(guild):
    """Called when bot leaves a guild"""
    print(f'Left guild: {guild.name}')

@bot.event
async def on_reaction_add(reaction, user):
    """Called when reaction is added"""
    print(f'{user} reacted with {reaction.emoji}')

@bot.event
async def on_reaction_remove(reaction, user):
    """Called when reaction is removed"""
    print(f'{user} removed reaction {reaction.emoji}')

@bot.event
async def on_voice_state_update(member, before, after):
    """Called when voice state changes"""
    if before.channel is None and after.channel is not None:
        print(f'{member} joined {after.channel}')
    elif before.channel is not None and after.channel is None:
        print(f'{member} left {before.channel}')

@bot.event
async def on_error(event, *args, **kwargs):
    """Called when error occurs"""
    import traceback
    print(f'Error in {event}:')
    traceback.print_exc()
```

---

### **4. COMMANDS**

#### **Basic Commands**

```python
@bot.command()
async def ping(ctx):
    """Check bot latency"""
    await ctx.send(f'Pong! {round(bot.latency * 1000)}ms')

@bot.command()
async def hello(ctx, *, name: str):
    """Say hello to someone"""
    await ctx.send(f'Hello {name}!')

@bot.command()
async def add(ctx, a: int, b: int):
    """Add two numbers"""
    result = a + b
    await ctx.send(f'{a} + {b} = {result}')
```

#### **Command with Aliases**

```python
@bot.command(aliases=['latency', 'ms'])
async def ping(ctx):
    """Check bot latency"""
    await ctx.send(f'Pong! {round(bot.latency * 1000)}ms')

# Can be called with: !ping, !latency, or !ms
```

#### **Command with Checks**

```python
from discord.ext import commands

@bot.command()
@commands.has_permissions(administrator=True)
async def clear(ctx, amount: int):
    """Delete messages (admin only)"""
    await ctx.channel.purge(limit=amount + 1)
    await ctx.send(f'Deleted {amount} messages', delete_after=5)

@bot.command()
@commands.is_owner()
async def shutdown(ctx):
    """Shutdown bot (owner only)"""
    await ctx.send('Shutting down...')
    await bot.close()

@bot.command()
@commands.cooldown(1, 5, commands.BucketType.user)
async def daily(ctx):
    """Daily reward (5s cooldown per user)"""
    await ctx.send('Here is your daily reward!')
```

#### **Error Handling**

```python
@bot.event
async def on_command_error(ctx, error):
    if isinstance(error, commands.MissingRequiredArgument):
        await ctx.send('Missing required argument!')
    elif isinstance(error, commands.CommandNotFound):
        await ctx.send('Command not found!')
    elif isinstance(error, commands.MissingPermissions):
        await ctx.send('You do not have permission!')
    elif isinstance(error, commands.CommandOnCooldown):
        await ctx.send(f'Cooldown! Try again in {error.retry_after:.2f}s')
    else:
        raise error
```

---

### **5. COGS (EXTENSIONS)**

Organize commands into modules.

#### **Creating a Cog**

```python
# cogs/moderation.py
from discord.ext import commands
import discord

class Moderation(commands.Cog):
    """Moderation commands"""
    
    def __init__(self, bot):
        self.bot = bot
    
    @commands.command()
    @commands.has_permissions(kick_members=True)
    async def kick(self, ctx, member: discord.Member, *, reason: str = None):
        """Kick a member"""
        await member.kick(reason=reason)
        await ctx.send(f'{member} has been kicked!')
    
    @commands.command()
    @commands.has_permissions(ban_members=True)
    async def ban(self, ctx, member: discord.Member, *, reason: str = None):
        """Ban a member"""
        await member.ban(reason=reason)
        await ctx.send(f'{member} has been banned!')
    
    @commands.Cog.listener()
    async def on_member_join(self, member):
        """Log when member joins"""
        print(f'{member} joined {member.guild}')

# Setup function (required)
async def setup(bot):
    await bot.add_cog(Moderation(bot))
```

#### **Loading Cogs**

```python
# main.py
import discord
from discord.ext import commands

bot = commands.Bot(command_prefix='!', intents=discord.Intents.default())

@bot.event
async def on_ready():
    print(f'Logged in as {bot.user}')

# Load cog
async def load_extensions():
    await bot.load_extension('cogs.moderation')
    await bot.load_extension('cogs.fun')
    await bot.load_extension('cogs.utility')

# Run
async def main():
    async with bot:
        await load_extensions()
        await bot.start('TOKEN')

# Python 3.10+
import asyncio
asyncio.run(main())

# Or older Python
bot.run('TOKEN')
```

---

### **6. DISCORD OBJECTS**

#### **Guild (Server)**

```python
# Get guild by ID
guild = bot.get_guild(123456789)

# Guild properties
print(guild.name)           # Guild name
print(guild.id)             # Guild ID
print(guild.member_count)   # Member count
print(guild.owner)          # Guild owner
print(guild.icon.url)       # Icon URL

# Get channels
for channel in guild.channels:
    print(channel.name)

# Get members
for member in guild.members:
    print(member.name)

# Get roles
for role in guild.roles:
    print(role.name)

# Edit guild
await guild.edit(name='New Name', description='New description')

# Leave guild
await guild.leave()
```

#### **Member**

```python
# Member properties
print(member.name)          # Username
print(member.display_name)  # Display name (nickname or username)
print(member.id)            # User ID
print(member.joined_at)     # Join date
print(member.roles)         # List of roles
print(member.top_role)      # Highest role
print(member.guild)         # Guild object

# Check permissions
if member.guild_permissions.administrator:
    print('Member is admin')

# Modify member
await member.edit(nick='New Nickname')
await member.add_roles(role)
await member.remove_roles(role)
await member.kick(reason='Spam')
await member.ban(reason='Violation', delete_message_days=7)

# Timeout member (mute)
from datetime import timedelta
await member.timeout(timedelta(minutes=10), reason='Spamming')

# Remove timeout
await member.timeout(None)

# Send DM
await member.send('Hello!')
```

#### **Message**

```python
# Message properties
print(message.content)      # Message content
print(message.author)       # Author (Member/User)
print(message.channel)      # Channel
print(message.guild)        # Guild
print(message.created_at)   # Created timestamp
print(message.attachments)  # List of attachments
print(message.embeds)       # List of embeds

# Reply to message
await message.reply('Hello!')

# Edit message
await message.edit(content='New content')

# Delete message
await message.delete()

# Add reaction
await message.add_reaction('👍')
await message.add_reaction('<:custom:123456789>')

# Pin message
await message.pin()

# Unpin message
await message.unpin()
```

#### **Channel**

```python
# Text channel
channel = bot.get_channel(123456789)

# Send message
await channel.send('Hello!')

# Send embed
embed = discord.Embed(title='Title', description='Description')
await channel.send(embed=embed)

# Send file
file = discord.File('image.png')
await channel.send(file=file)

# Bulk delete messages
messages = await channel.purge(limit=10)

# Edit channel
await channel.edit(name='new-name', topic='New topic')

# Delete channel
await channel.delete()

# Get message history
async for message in channel.history(limit=100):
    print(message.content)

# Create invite
invite = await channel.create_invite(max_age=3600, max_uses=10)
```

#### **Role**

```python
# Role properties
print(role.name)            # Role name
print(role.id)              # Role ID
print(role.color)           # Role color
print(role.position)        # Role position
print(role.mentionable)     # Is mentionable
print(role.hoist)           # Is hoisted
print(role.permissions)     # Permissions

# Edit role
await role.edit(
    name='New Name',
    color=discord.Color.blue(),
    permissions=discord.Permissions(administrator=True)
)

# Delete role
await role.delete()

# Create role
new_role = await guild.create_role(
    name='Member',
    color=discord.Color.blue(),
    permissions=discord.Permissions(send_messages=True)
)
```

---

### **7. EMBEDS**

```python
import discord

# Basic embed
embed = discord.Embed(
    title='Embed Title',
    description='Embed description',
    color=discord.Color.blue()
)

# Add fields
embed.add_field(name='Field 1', value='Value 1', inline=True)
embed.add_field(name='Field 2', value='Value 2', inline=True)

# Set author
embed.set_author(name='Author Name', icon_url='https://...')

# Set footer
embed.set_footer(text='Footer text', icon_url='https://...')

# Set thumbnail
embed.set_thumbnail(url='https://...')

# Set image
embed.set_image(url='https://...')

# Set timestamp
from datetime import datetime
embed.timestamp = datetime.now()

# Send embed
await ctx.send(embed=embed)

# Multiple embeds
embed1 = discord.Embed(title='Embed 1')
embed2 = discord.Embed(title='Embed 2')
await ctx.send(embeds=[embed1, embed2])
```

#### **Embed Colors**

```python
discord.Color.blue()
discord.Color.red()
discord.Color.green()
discord.Color.gold()
discord.Color.purple()
discord.Color.orange()
discord.Color.from_rgb(255, 0, 0)   # RGB
discord.Color(0xFF0000)              # Hex
```

---

### **8. VIEWS & BUTTONS**

#### **Buttons**

```python
import discord
from discord import ui

class MyView(ui.View):
    @ui.button(label='Click Me', style=discord.ButtonStyle.primary)
    async def button_callback(self, interaction: discord.Interaction, button: ui.Button):
        await interaction.response.send_message('Button clicked!', ephemeral=True)

@bot.command()
async def button(ctx):
    view = MyView()
    await ctx.send('Click the button:', view=view)
```

#### **Multiple Buttons**

```python
class GameView(ui.View):
    @ui.button(label='Rock', style=discord.ButtonStyle.secondary, emoji='🪨')
    async def rock(self, interaction: discord.Interaction, button: ui.Button):
        await interaction.response.send_message('You chose Rock!', ephemeral=True)
    
    @ui.button(label='Paper', style=discord.ButtonStyle.secondary, emoji='📄')
    async def paper(self, interaction: discord.Interaction, button: ui.Button):
        await interaction.response.send_message('You chose Paper!', ephemeral=True)
    
    @ui.button(label='Scissors', style=discord.ButtonStyle.secondary, emoji='✂️')
    async def scissors(self, interaction: discord.Interaction, button: ui.Button):
        await interaction.response.send_message('You chose Scissors!', ephemeral=True)

@bot.command()
async def rps(ctx):
    view = GameView()
    await ctx.send('Choose your move:', view=view)
```

#### **Button Styles**

```python
discord.ButtonStyle.primary     # Blue
discord.ButtonStyle.secondary   # Gray
discord.ButtonStyle.success     # Green
discord.ButtonStyle.danger      # Red
discord.ButtonStyle.link        # Link button
```

---

### **9. SELECT MENUS**

```python
class MySelect(ui.Select):
    def __init__(self):
        options = [
            discord.SelectOption(label='Red', emoji='🔴', description='Red color'),
            discord.SelectOption(label='Blue', emoji='🔵', description='Blue color'),
            discord.SelectOption(label='Green', emoji='🟢', description='Green color'),
        ]
        super().__init__(placeholder='Choose a color', options=options)
    
    async def callback(self, interaction: discord.Interaction):
        await interaction.response.send_message(
            f'You chose {self.values[0]}!',
            ephemeral=True
        )

class SelectView(ui.View):
    def __init__(self):
        super().__init__()
        self.add_item(MySelect())

@bot.command()
async def select(ctx):
    view = SelectView()
    await ctx.send('Choose an option:', view=view)
```

---

### **10. MODALS**

```python
class FeedbackModal(ui.Modal, title='Feedback Form'):
    name = ui.TextInput(label='Name', placeholder='Your name...')
    feedback = ui.TextInput(
        label='Feedback',
        style=discord.TextStyle.paragraph,
        placeholder='Your feedback...',
        max_length=500
    )
    
    async def on_submit(self, interaction: discord.Interaction):
        await interaction.response.send_message(
            f'Thanks {self.name.value}! Feedback: {self.feedback.value}',
            ephemeral=True
        )

@bot.command()
async def feedback(ctx):
    await ctx.send('Use `/feedback` slash command to open the form!')

# For slash command
@bot.tree.command()
async def feedback(interaction: discord.Interaction):
    await interaction.response.send_modal(FeedbackModal())
```

---

### **11. SLASH COMMANDS**

```python
from discord import app_commands

# Sync commands
@bot.event
async def on_ready():
    await bot.tree.sync()
    print(f'Logged in as {bot.user}')

# Simple slash command
@bot.tree.command()
async def hello(interaction: discord.Interaction):
    """Say hello"""
    await interaction.response.send_message(f'Hello {interaction.user.mention}!')

# Slash command with arguments
@bot.tree.command()
@app_commands.describe(member='The member to greet', message='The greeting message')
async def greet(interaction: discord.Interaction, member: discord.Member, message: str):
    """Greet a member"""
    await interaction.response.send_message(f'{member.mention}: {message}')

# Slash command with choices
@bot.tree.command()
@app_commands.describe(fruit='Choose a fruit')
@app_commands.choices(fruit=[
    app_commands.Choice(name='Apple', value='apple'),
    app_commands.Choice(name='Banana', value='banana'),
    app_commands.Choice(name='Orange', value='orange'),
])
async def fruit(interaction: discord.Interaction, fruit: app_commands.Choice[str]):
    """Choose a fruit"""
    await interaction.response.send_message(f'You chose: {fruit.name}')
```

---

### **12. PERMISSIONS**

```python
# Check permissions
if ctx.author.guild_permissions.administrator:
    print('User is admin')

# Permission bits
discord.Permissions(
    administrator=True,
    kick_members=True,
    ban_members=True,
    manage_channels=True,
    manage_guild=True,
    manage_messages=True,
    send_messages=True,
    attach_files=True,
    mention_everyone=True,
)

# Check specific permission
permissions = ctx.author.guild_permissions
if permissions.administrator:
    print('Has administrator')
if permissions.kick_members:
    print('Can kick members')

# Channel permissions
permissions = ctx.channel.permissions_for(ctx.author)
if permissions.send_messages:
    print('Can send messages in this channel')
```

---

### **13. TASKS & LOOPS**

```python
from discord.ext import tasks

@tasks.loop(seconds=60)
async def my_task():
    """Run every 60 seconds"""
    print('Task running...')

@tasks.loop(minutes=5)
async def status_update():
    """Update bot status every 5 minutes"""
    await bot.change_presence(activity=discord.Game(name='with Python'))

@my_task.before_loop
async def before_my_task():
    """Wait until bot is ready"""
    await bot.wait_until_ready()

# Start task
my_task.start()
status_update.start()

# Stop task
my_task.stop()

# Cancel task
my_task.cancel()
```

---

## 💡 COMMON PATTERNS

### **Command Handler with Cogs**

```python
import os
from discord.ext import commands

bot = commands.Bot(command_prefix='!', intents=discord.Intents.default())

async def load_cogs():
    for filename in os.listdir('./cogs'):
        if filename.endswith('.py'):
            await bot.load_extension(f'cogs.{filename[:-3]}')

@bot.event
async def on_ready():
    await load_cogs()
    await bot.tree.sync()
    print(f'Bot is ready: {bot.user}')

bot.run('TOKEN')
```

### **Error Handling**

```python
@bot.event
async def on_command_error(ctx, error):
    if isinstance(error, commands.MissingRequiredArgument):
        await ctx.send('❌ Missing required argument!')
    elif isinstance(error, commands.BadArgument):
        await ctx.send('❌ Invalid argument!')
    elif isinstance(error, commands.CommandNotFound):
        await ctx.send('❌ Command not found!')
    elif isinstance(error, commands.MissingPermissions):
        await ctx.send('❌ You lack permissions!')
    elif isinstance(error, commands.CommandOnCooldown):
        await ctx.send(f'⏰ On cooldown! Retry in {error.retry_after:.2f}s')
    else:
        print(f'Error: {error}')
        raise error
```

### **Presence Management**

```python
@bot.event
async def on_ready():
    # Playing status
    await bot.change_presence(activity=discord.Game(name='with Python'))
    
    # Watching status
    await bot.change_presence(activity=discord.Activity(
        type=discord.ActivityType.watching,
        name='for commands'
    ))
    
    # Listening status
    await bot.change_presence(activity=discord.Activity(
        type=discord.ActivityType.listening,
        name='!help'
    ))
    
    # Streaming status
    await bot.change_presence(activity=discord.Streaming(
        name='Live Stream',
        url='https://twitch.tv/...'
    ))
```

---

## 📚 BEST PRACTICES

### **1. Use Intents Wisely**
Only request intents you need.

### **2. Use Cogs for Organization**
Organize commands into logical modules.

### **3. Handle Errors Properly**
Always implement error handlers.

### **4. Use Async/Await**
All Discord API calls are asynchronous.

### **5. Don't Block the Event Loop**
Use `asyncio` for long-running tasks.

### **6. Cache Management**
```python
# Disable member cache for large bots
intents = discord.Intents.default()
intents.members = False

bot = commands.Bot(
    command_prefix='!',
    intents=intents,
    member_cache_flags=discord.MemberCacheFlags.none()
)
```

---

## 🔗 RESOURCES

- **Documentation:** https://discordpy.readthedocs.io
- **Guide:** https://guide.pycord.dev
- **GitHub:** https://github.com/Rapptz/discord.py
- **Discord:** https://discord.gg/dpy

---

**Created:** 2025-01-09  
**Version:** discord.py v2.3+  
**Python:** 3.8+
