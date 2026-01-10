# 📚 py-cord - MAIN LIBRARY DOCUMENTATION

**Package:** `py-cord`  
**Version:** v2.4+  
**Purpose:** Modern, feature-rich Discord bot framework for Python

---

## 🎯 OVERVIEW

Py-cord is a modern, easy-to-use, feature-rich, and async-ready fork of discord.py. It offers the best developer experience with simpler slash commands, bridge commands, and better UI components support.

**Why py-cord over discord.py?**
- ✅ Simpler slash command syntax
- ✅ Bridge commands (work as both prefix & slash)
- ✅ Better UI components (buttons, selects, modals)
- ✅ More frequent updates
- ✅ Better documentation
- ✅ Active community

---

## 📦 INSTALLATION

```bash
# Stable version
pip install py-cord

# With voice support
pip install py-cord[voice]

# Development version
pip install git+https://github.com/Pycord-Development/pycord
```

---

## 🚀 BASIC SETUP

### **Simple Bot**

```python
import discord

bot = discord.Bot()

@bot.event
async def on_ready():
    print(f'{bot.user} is ready and online!')

@bot.slash_command(name="hello", description="Say hello")
async def hello(ctx):
    await ctx.respond("Hey!")

bot.run('YOUR_BOT_TOKEN')
```

### **With Intents**

```python
import discord

intents = discord.Intents.default()
intents.message_content = True

bot = discord.Bot(intents=intents)

@bot.event
async def on_ready():
    print(f'Logged in as {bot.user}')

bot.run('YOUR_BOT_TOKEN')
```

---

## 📋 SLASH COMMANDS (Simplified)

### **Basic Slash Command**

```python
@bot.slash_command(name="ping", description="Check bot latency")
async def ping(ctx):
    await ctx.respond(f'Pong! Latency: {round(bot.latency * 1000)}ms')
```

### **Slash Command with Options**

```python
@bot.slash_command(name="greet", description="Greet someone")
async def greet(
    ctx,
    member: discord.Option(discord.Member, "Choose a member"),
    message: discord.Option(str, "Your message")
):
    await ctx.respond(f'{member.mention}: {message}')
```

### **Slash Command with Choices**

```python
@bot.slash_command(name="fruit", description="Choose a fruit")
async def fruit(
    ctx,
    choice: discord.Option(
        str,
        "Pick your favorite",
        choices=["Apple", "Banana", "Orange"]
    )
):
    await ctx.respond(f'You chose: {choice}')
```

### **Slash Command with Integer Range**

```python
@bot.slash_command(name="level", description="Set your level")
async def level(
    ctx,
    level: discord.Option(int, "Your level", min_value=1, max_value=100)
):
    await ctx.respond(f'Level set to {level}!')
```

### **Slash Command with Autocomplete**

```python
async def get_fruits(ctx: discord.AutocompleteContext):
    """Returns a list of fruits that match the user's input"""
    fruits = ['Apple', 'Banana', 'Cherry', 'Dragon Fruit', 'Elderberry']
    return [fruit for fruit in fruits if ctx.value.lower() in fruit.lower()]

@bot.slash_command(name="search", description="Search for a fruit")
async def search(
    ctx,
    fruit: discord.Option(str, "Type to search", autocomplete=get_fruits)
):
    await ctx.respond(f'You selected: {fruit}')
```

---

## 🌉 BRIDGE COMMANDS

Bridge commands work as both prefix commands AND slash commands!

```python
from discord.ext import bridge

bot = bridge.Bot(command_prefix='!')

@bot.bridge_command(name="hello", description="Say hello")
async def hello(ctx):
    await ctx.respond("Hello!")

# Works as both:
# !hello (prefix command)
# /hello (slash command)
```

### **Bridge Command with Arguments**

```python
@bot.bridge_command()
async def add(ctx, a: int, b: int):
    """Add two numbers"""
    await ctx.respond(f'{a} + {b} = {a + b}')

# Usage:
# !add 5 10
# /add a:5 b:10
```

---

## 🎮 SLASH COMMAND GROUPS

### **Simple Group**

```python
group = bot.create_group("config", "Configuration commands")

@group.command(name="set", description="Set a value")
async def config_set(ctx, key: str, value: str):
    await ctx.respond(f'Set {key} = {value}')

@group.command(name="get", description="Get a value")
async def config_get(ctx, key: str):
    await ctx.respond(f'Value of {key}')

# Usage: /config set key:prefix value:!
```

### **Nested Groups**

```python
settings = bot.create_group("settings", "Settings commands")
user_settings = settings.create_subgroup("user", "User settings")

@user_settings.command(name="profile", description="Edit profile")
async def profile(ctx):
    await ctx.respond("Profile editor")

@user_settings.command(name="privacy", description="Privacy settings")
async def privacy(ctx):
    await ctx.respond("Privacy settings")

# Usage: /settings user profile
```

---

## 🎨 UI COMPONENTS

### **Buttons**

```python
class MyView(discord.ui.View):
    @discord.ui.button(label="Click Me", style=discord.ButtonStyle.primary)
    async def button_callback(self, button, interaction):
        await interaction.response.send_message("You clicked the button!", ephemeral=True)

@bot.slash_command()
async def buttons(ctx):
    view = MyView()
    await ctx.respond("Click a button:", view=view)
```

### **Multiple Buttons**

```python
class GameView(discord.ui.View):
    @discord.ui.button(label="Rock", style=discord.ButtonStyle.secondary, emoji="🪨")
    async def rock(self, button, interaction):
        await interaction.response.send_message("You chose Rock!", ephemeral=True)
    
    @discord.ui.button(label="Paper", style=discord.ButtonStyle.secondary, emoji="📄")
    async def paper(self, button, interaction):
        await interaction.response.send_message("You chose Paper!", ephemeral=True)
    
    @discord.ui.button(label="Scissors", style=discord.ButtonStyle.secondary, emoji="✂️")
    async def scissors(self, button, interaction):
        await interaction.response.send_message("You chose Scissors!", ephemeral=True)

@bot.slash_command()
async def rps(ctx):
    view = GameView()
    await ctx.respond("Rock, Paper, Scissors!", view=view)
```

### **Persistent Buttons**

```python
class PersistentView(discord.ui.View):
    def __init__(self):
        super().__init__(timeout=None)  # Persistent across restarts
    
    @discord.ui.button(
        label="Verify", 
        style=discord.ButtonStyle.success,
        custom_id="verify_button"  # Custom ID for persistence
    )
    async def verify(self, button, interaction):
        role = interaction.guild.get_role(123456789)
        await interaction.user.add_roles(role)
        await interaction.response.send_message("Verified!", ephemeral=True)

# Register persistent view on startup
bot.add_view(PersistentView())
```

---

## 📝 SELECT MENUS

### **String Select**

```python
class MySelect(discord.ui.Select):
    def __init__(self):
        options = [
            discord.SelectOption(label="Red", emoji="🔴", description="Red color"),
            discord.SelectOption(label="Blue", emoji="🔵", description="Blue color"),
            discord.SelectOption(label="Green", emoji="🟢", description="Green color"),
        ]
        super().__init__(
            placeholder="Choose a color",
            min_values=1,
            max_values=1,
            options=options
        )
    
    async def callback(self, interaction):
        await interaction.response.send_message(
            f'You chose {self.values[0]}!',
            ephemeral=True
        )

class SelectView(discord.ui.View):
    def __init__(self):
        super().__init__()
        self.add_item(MySelect())

@bot.slash_command()
async def select(ctx):
    view = SelectView()
    await ctx.respond("Choose an option:", view=view)
```

### **User Select**

```python
class UserSelectView(discord.ui.View):
    @discord.ui.user_select(placeholder="Choose users", min_values=1, max_values=3)
    async def user_select(self, select, interaction):
        users = ", ".join([user.mention for user in select.values])
        await interaction.response.send_message(f'Selected: {users}', ephemeral=True)

@bot.slash_command()
async def pick_users(ctx):
    await ctx.respond("Pick users:", view=UserSelectView())
```

### **Role Select**

```python
class RoleSelectView(discord.ui.View):
    @discord.ui.role_select(placeholder="Choose roles", min_values=1, max_values=5)
    async def role_select(self, select, interaction):
        roles = ", ".join([role.mention for role in select.values])
        await interaction.response.send_message(f'Selected: {roles}', ephemeral=True)

@bot.slash_command()
async def pick_roles(ctx):
    await ctx.respond("Pick roles:", view=RoleSelectView())
```

### **Channel Select**

```python
class ChannelSelectView(discord.ui.View):
    @discord.ui.channel_select(
        placeholder="Choose channels",
        channel_types=[discord.ChannelType.text, discord.ChannelType.voice]
    )
    async def channel_select(self, select, interaction):
        channels = ", ".join([channel.mention for channel in select.values])
        await interaction.response.send_message(f'Selected: {channels}', ephemeral=True)

@bot.slash_command()
async def pick_channels(ctx):
    await ctx.respond("Pick channels:", view=ChannelSelectView())
```

---

## 🪟 MODALS

### **Simple Modal**

```python
class FeedbackModal(discord.ui.Modal):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        
        self.add_item(discord.ui.InputText(label="Name", placeholder="Your name"))
        self.add_item(discord.ui.InputText(
            label="Feedback",
            style=discord.InputTextStyle.long,
            placeholder="Your feedback...",
            max_length=500
        ))
    
    async def callback(self, interaction):
        name = self.children[0].value
        feedback = self.children[1].value
        await interaction.response.send_message(
            f'Thanks {name}! Feedback: {feedback}',
            ephemeral=True
        )

@bot.slash_command()
async def feedback(ctx):
    modal = FeedbackModal(title="Feedback Form")
    await ctx.send_modal(modal)
```

### **Modal with Custom Validation**

```python
class RegisterModal(discord.ui.Modal):
    def __init__(self):
        super().__init__(title="Registration Form")
        
        self.add_item(discord.ui.InputText(
            label="Username",
            placeholder="Enter username (min 3 chars)",
            min_length=3,
            max_length=20
        ))
        
        self.add_item(discord.ui.InputText(
            label="Email",
            placeholder="your@email.com"
        ))
    
    async def callback(self, interaction):
        username = self.children[0].value
        email = self.children[1].value
        
        # Validate email
        if '@' not in email:
            await interaction.response.send_message(
                'Invalid email!',
                ephemeral=True
            )
            return
        
        # Process registration
        await interaction.response.send_message(
            f'Registered: {username} ({email})',
            ephemeral=True
        )

@bot.slash_command()
async def register(ctx):
    await ctx.send_modal(RegisterModal())
```

---

## 🎭 COGS

### **Creating a Cog**

```python
# cogs/moderation.py
import discord
from discord.ext import commands

class Moderation(commands.Cog):
    """Moderation commands"""
    
    def __init__(self, bot):
        self.bot = bot
    
    @discord.slash_command(name="kick", description="Kick a member")
    @discord.default_permissions(kick_members=True)
    async def kick(
        self,
        ctx,
        member: discord.Option(discord.Member, "Member to kick"),
        reason: discord.Option(str, "Reason", required=False)
    ):
        await member.kick(reason=reason)
        await ctx.respond(f'{member.mention} has been kicked!')
    
    @discord.slash_command(name="ban", description="Ban a member")
    @discord.default_permissions(ban_members=True)
    async def ban(
        self,
        ctx,
        member: discord.Option(discord.Member, "Member to ban"),
        reason: discord.Option(str, "Reason", required=False)
    ):
        await member.ban(reason=reason)
        await ctx.respond(f'{member.mention} has been banned!')
    
    @commands.Cog.listener()
    async def on_member_join(self, member):
        """Log when member joins"""
        print(f'{member} joined {member.guild}')

def setup(bot):
    bot.add_cog(Moderation(bot))
```

### **Loading Cogs**

```python
# main.py
import discord
import os

bot = discord.Bot()

@bot.event
async def on_ready():
    print(f'{bot.user} is ready!')

# Load all cogs
for filename in os.listdir('./cogs'):
    if filename.endswith('.py'):
        bot.load_extension(f'cogs.{filename[:-3]}')

bot.run('TOKEN')
```

---

## 🎯 CONTEXT MENUS

### **User Context Menu**

```python
@bot.user_command(name="User Info")
async def user_info(ctx, member: discord.Member):
    """Right-click user menu"""
    embed = discord.Embed(
        title=f'Info: {member}',
        description=f'ID: {member.id}\nJoined: {member.joined_at}'
    )
    await ctx.respond(embed=embed, ephemeral=True)
```

### **Message Context Menu**

```python
@bot.message_command(name="Report Message")
async def report_message(ctx, message: discord.Message):
    """Right-click message menu"""
    await ctx.respond(
        f'Reported message from {message.author.mention}',
        ephemeral=True
    )
```

---

## 📊 EMBEDS

```python
import discord

@bot.slash_command()
async def embed_example(ctx):
    embed = discord.Embed(
        title="Embed Title",
        description="This is the description",
        color=discord.Color.blue()
    )
    
    embed.add_field(name="Field 1", value="Value 1", inline=True)
    embed.add_field(name="Field 2", value="Value 2", inline=True)
    
    embed.set_author(name=ctx.author.name, icon_url=ctx.author.display_avatar.url)
    embed.set_footer(text="Footer text")
    embed.set_thumbnail(url=ctx.guild.icon.url)
    embed.set_image(url="https://example.com/image.png")
    
    await ctx.respond(embed=embed)
```

---

## ⏰ TASKS & LOOPS

```python
from discord.ext import tasks

@tasks.loop(seconds=60)
async def my_task():
    """Runs every 60 seconds"""
    print('Task running...')

@tasks.loop(minutes=5)
async def status_update():
    """Update status every 5 minutes"""
    await bot.change_presence(
        activity=discord.Activity(
            type=discord.ActivityType.watching,
            name="for commands"
        )
    )

@my_task.before_loop
async def before_my_task():
    """Wait until bot is ready"""
    await bot.wait_until_ready()

# Start tasks
@bot.event
async def on_ready():
    my_task.start()
    status_update.start()
```

---

## 🔐 PERMISSIONS & CHECKS

### **Slash Command Permissions**

```python
@bot.slash_command()
@discord.default_permissions(administrator=True)
async def admin_command(ctx):
    """Admin only command"""
    await ctx.respond("Admin command executed!")

@bot.slash_command()
@discord.default_permissions(kick_members=True, ban_members=True)
async def mod_command(ctx):
    """Moderator command"""
    await ctx.respond("Mod command executed!")
```

### **Custom Checks**

```python
def is_owner(ctx):
    return ctx.author.id == 123456789

@bot.slash_command()
@discord.ext.commands.check(is_owner)
async def owner_command(ctx):
    """Owner only command"""
    await ctx.respond("Owner command!")
```

---

## 🎮 PAGINATOR

```python
from discord.ext import pages

@bot.slash_command()
async def paginator_example(ctx):
    # Create pages
    page_list = [
        "Page 1 content",
        "Page 2 content",
        "Page 3 content",
        "Page 4 content",
    ]
    
    # Or use embeds
    embed_pages = [
        discord.Embed(title=f"Page {i+1}", description=content)
        for i, content in enumerate(page_list)
    ]
    
    paginator = pages.Paginator(pages=embed_pages)
    await paginator.respond(ctx.interaction, ephemeral=False)
```

---

## 🔄 ERROR HANDLING

```python
@bot.event
async def on_application_command_error(ctx, error):
    if isinstance(error, discord.errors.ApplicationCommandInvokeError):
        await ctx.respond('An error occurred!', ephemeral=True)
    elif isinstance(error, discord.errors.CheckFailure):
        await ctx.respond('You lack permissions!', ephemeral=True)
    else:
        raise error
```

---

## 💾 EXAMPLE: FULL BOT

```python
import discord
from discord.ext import commands, tasks
import os
from dotenv import load_dotenv

load_dotenv()

# Configure bot
intents = discord.Intents.default()
intents.message_content = True
intents.members = True

bot = discord.Bot(intents=intents)

# Events
@bot.event
async def on_ready():
    print(f'{bot.user} is online!')
    status_update.start()

# Tasks
@tasks.loop(minutes=5)
async def status_update():
    await bot.change_presence(
        activity=discord.Activity(
            type=discord.ActivityType.watching,
            name=f"{len(bot.guilds)} servers"
        )
    )

# Slash commands
@bot.slash_command(name="ping", description="Check bot latency")
async def ping(ctx):
    await ctx.respond(f'Pong! {round(bot.latency * 1000)}ms')

@bot.slash_command(name="hello", description="Say hello")
async def hello(ctx, name: discord.Option(str, "Your name")):
    await ctx.respond(f'Hello {name}!')

# Button example
class VerifyView(discord.ui.View):
    @discord.ui.button(label="Verify", style=discord.ButtonStyle.success)
    async def verify_button(self, button, interaction):
        role = interaction.guild.get_role(int(os.getenv('VERIFIED_ROLE_ID')))
        await interaction.user.add_roles(role)
        await interaction.response.send_message(
            'You have been verified!',
            ephemeral=True
        )

@bot.slash_command(name="verify", description="Verification system")
async def verify(ctx):
    view = VerifyView()
    await ctx.respond('Click to verify:', view=view)

# Load cogs
for filename in os.listdir('./cogs'):
    if filename.endswith('.py'):
        bot.load_extension(f'cogs.{filename[:-3]}')

# Run bot
bot.run(os.getenv('DISCORD_TOKEN'))
```

---

## 📊 PY-CORD VS DISCORD.PY

| Feature | py-cord | discord.py |
|---------|---------|------------|
| **Slash Commands** | ✅✅ Simple | ✅ Complex |
| **Bridge Commands** | ✅ Yes | ❌ No |
| **UI Components** | ✅✅ Better | ✅ Good |
| **Paginator** | ✅ Built-in | ❌ External |
| **Documentation** | ✅✅ Excellent | ✅ Good |
| **Updates** | ✅✅ Frequent | ✅ Regular |
| **Learning Curve** | ✅✅ Easy | ✅ Moderate |

---

## 🔗 RESOURCES

- **Documentation:** https://docs.pycord.dev
- **Guide:** https://guide.pycord.dev
- **GitHub:** https://github.com/Pycord-Development/pycord
- **Discord:** https://discord.gg/pycord
- **Examples:** https://github.com/Pycord-Development/pycord/tree/master/examples

---

## 💡 MIGRATION FROM DISCORD.PY

### **Main Differences:**

1. **Slash Commands:**
```python
# discord.py
@bot.tree.command()
async def hello(interaction: discord.Interaction):
    await interaction.response.send_message("Hello")

# py-cord
@bot.slash_command()
async def hello(ctx):
    await ctx.respond("Hello")
```

2. **Options:**
```python
# discord.py
@app_commands.describe(name="Your name")
async def hello(interaction, name: str):
    ...

# py-cord
async def hello(ctx, name: discord.Option(str, "Your name")):
    ...
```

3. **Bridge Commands:**
```python
# Only in py-cord
@bot.bridge_command()
async def ping(ctx):
    await ctx.respond("Pong!")
```

---

## 📚 BEST PRACTICES

1. ✅ Use slash commands for modern bots
2. ✅ Use bridge commands for backward compatibility
3. ✅ Always use `ctx.respond()` for slash commands
4. ✅ Set `ephemeral=True` for private responses
5. ✅ Use cogs to organize code
6. ✅ Add descriptions to all commands
7. ✅ Use `default_permissions` for permission checks
8. ✅ Handle errors properly

---

**Created:** 2025-01-09  
**Version:** py-cord v2.4+  
**Python:** 3.8+  
**Recommended for:** Modern Discord bots with slash commands
