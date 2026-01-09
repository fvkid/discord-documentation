# 📚 @discord.js/collection - EXTENDED MAP DOCUMENTATION

**Package:** `@discordjs/collection`  
**Version:** v2+  
**Purpose:** Extended JavaScript Map with utility methods

---

## 📦 INSTALLATION

```bash
npm install @discordjs/collection
```

---

## 🎯 OVERVIEW

Collection extends native JavaScript Map with array-like methods for easier data manipulation.

---

## 🏗️ BASIC USAGE

```typescript
import { Collection } from '@discordjs/collection';

const users = new Collection();

// Set values
users.set('123', { name: 'Alice', age: 25 });
users.set('456', { name: 'Bob', age: 30 });

// Get values
const user = users.get('123');  // { name: 'Alice', age: 25 }

// Check existence
users.has('123');  // true

// Delete
users.delete('123');

// Clear all
users.clear();

// Size
console.log(users.size);  // 2
```

---

## 📋 ARRAY-LIKE METHODS

### **filter()**
```typescript
const adults = users.filter(user => user.age >= 18);
// Returns new Collection with users age >= 18
```

### **map()**
```typescript
const names = users.map(user => user.name);
// ['Alice', 'Bob']
```

### **find()**
```typescript
const alice = users.find(user => user.name === 'Alice');
// { name: 'Alice', age: 25 }
```

### **findKey()**
```typescript
const aliceId = users.findKey(user => user.name === 'Alice');
// '123'
```

### **some()**
```typescript
const hasAdults = users.some(user => user.age >= 18);
// true
```

### **every()**
```typescript
const allAdults = users.every(user => user.age >= 18);
// true/false
```

### **reduce()**
```typescript
const totalAge = users.reduce((sum, user) => sum + user.age, 0);
// 55
```

---

## 🔧 UTILITY METHODS

### **first()**
```typescript
users.first();       // First value
users.first(2);      // First 2 values as array
```

### **last()**
```typescript
users.last();        // Last value
users.last(2);       // Last 2 values as array
```

### **random()**
```typescript
users.random();      // Random value
users.random(3);     // 3 random values
```

### **at()**
```typescript
users.at(0);         // First value
users.at(-1);        // Last value
```

### **keyAt()**
```typescript
users.keyAt(0);      // First key
users.keyAt(-1);     // Last key
```

---

## 🎨 ADVANCED METHODS

### **partition()**
```typescript
const [adults, minors] = users.partition(user => user.age >= 18);
// Returns [Collection<adults>, Collection<minors>]
```

### **sweep()**
```typescript
// Remove and return number of removed items
const removed = users.sweep(user => user.age < 18);
```

### **tap()**
```typescript
users.tap(coll => console.log(coll.size));
// Logs size and returns collection for chaining
```

### **clone()**
```typescript
const copy = users.clone();
// Creates shallow copy
```

### **concat()**
```typescript
const combined = users.concat(otherUsers, moreUsers);
// Combines multiple collections
```

### **difference()**
```typescript
const diff = users.difference(otherUsers);
// Returns items in users but not in otherUsers
```

### **intersect()**
```typescript
const common = users.intersect(otherUsers);
// Returns items in both collections
```

### **sorted()**
```typescript
const sortedUsers = users.sorted((a, b) => a.age - b.age);
// Returns new sorted collection
```

---

## 💡 PRACTICAL EXAMPLES

### **Guild Members by Role**
```typescript
const members = new Collection();

// Filter by role
const admins = members.filter(m => 
    m.roles.cache.has('admin_role_id')
);

// Get all usernames
const usernames = members.map(m => m.user.username);

// Find specific member
const owner = members.find(m => m.id === guild.ownerId);
```

### **Command Handler**
```typescript
const commands = new Collection();

// Set commands
commands.set('ping', pingCommand);
commands.set('help', helpCommand);

// Get command
const command = commands.get(commandName);

// Check existence
if (commands.has(commandName)) {
    await commands.get(commandName).execute(message);
}
```

### **Cache Management**
```typescript
const cache = new Collection();

// Add with expiry
cache.set('key', { data, timestamp: Date.now() });

// Clean old entries
cache.sweep(item => Date.now() - item.timestamp > 3600000);

// Get cache stats
console.log(`Cache size: ${cache.size}`);
```

---

## 📊 METHOD COMPARISON

| Method | Returns | Mutates Original |
|--------|---------|------------------|
| `filter()` | New Collection | No |
| `map()` | Array | No |
| `find()` | Value or undefined | No |
| `some()` | Boolean | No |
| `every()` | Boolean | No |
| `reduce()` | Any | No |
| `sweep()` | Number | Yes |
| `tap()` | Same Collection | No |
| `clone()` | New Collection | No |
| `sort()` | Same Collection | Yes |
| `sorted()` | New Collection | No |

---

## 🔗 RESOURCES

- **NPM:** https://www.npmjs.com/package/@discordjs/collection
- **GitHub:** https://github.com/discordjs/discord.js/tree/main/packages/collection

---

**Created:** 2025-01-09  
**Version:** @discordjs/collection v2+
EOFCOLL

cat /home/claude/discord.js-collection.md
Output

# 📚 @DISCORDJS/COLLECTION - EXTENDED MAP DOCUMENTATION

**Package:** `@discordjs/collection`  
**Version:** v2+  
**Purpose:** Extended JavaScript Map with utility methods

---

## 📦 INSTALLATION

```bash
npm install @discordjs/collection
```

---

## 🎯 OVERVIEW

Collection extends native JavaScript Map with array-like methods for easier data manipulation.

---

## 🏗️ BASIC USAGE

```typescript
import { Collection } from '@discordjs/collection';

const users = new Collection();

// Set values
users.set('123', { name: 'Alice', age: 25 });
users.set('456', { name: 'Bob', age: 30 });

// Get values
const user = users.get('123');  // { name: 'Alice', age: 25 }

// Check existence
users.has('123');  // true

// Delete
users.delete('123');

// Clear all
users.clear();

// Size
console.log(users.size);  // 2
```

---

## 📋 ARRAY-LIKE METHODS

### **filter()**
```typescript
const adults = users.filter(user => user.age >= 18);
// Returns new Collection with users age >= 18
```

### **map()**
```typescript
const names = users.map(user => user.name);
// ['Alice', 'Bob']
```

### **find()**
```typescript
const alice = users.find(user => user.name === 'Alice');
// { name: 'Alice', age: 25 }
```

### **findKey()**
```typescript
const aliceId = users.findKey(user => user.name === 'Alice');
// '123'
```

### **some()**
```typescript
const hasAdults = users.some(user => user.age >= 18);
// true
```

### **every()**
```typescript
const allAdults = users.every(user => user.age >= 18);
// true/false
```

### **reduce()**
```typescript
const totalAge = users.reduce((sum, user) => sum + user.age, 0);
// 55
```

---

## 🔧 UTILITY METHODS

### **first()**
```typescript
users.first();       // First value
users.first(2);      // First 2 values as array
```

### **last()**
```typescript
users.last();        // Last value
users.last(2);       // Last 2 values as array
```

### **random()**
```typescript
users.random();      // Random value
users.random(3);     // 3 random values
```

### **at()**
```typescript
users.at(0);         // First value
users.at(-1);        // Last value
```

### **keyAt()**
```typescript
users.keyAt(0);      // First key
users.keyAt(-1);     // Last key
```

---

## 🎨 ADVANCED METHODS

### **partition()**
```typescript
const [adults, minors] = users.partition(user => user.age >= 18);
// Returns [Collection<adults>, Collection<minors>]
```

### **sweep()**
```typescript
// Remove and return number of removed items
const removed = users.sweep(user => user.age < 18);
```

### **tap()**
```typescript
users.tap(coll => console.log(coll.size));
// Logs size and returns collection for chaining
```

### **clone()**
```typescript
const copy = users.clone();
// Creates shallow copy
```

### **concat()**
```typescript
const combined = users.concat(otherUsers, moreUsers);
// Combines multiple collections
```

### **difference()**
```typescript
const diff = users.difference(otherUsers);
// Returns items in users but not in otherUsers
```

### **intersect()**
```typescript
const common = users.intersect(otherUsers);
// Returns items in both collections
```

### **sorted()**
```typescript
const sortedUsers = users.sorted((a, b) => a.age - b.age);
// Returns new sorted collection
```

---

## 💡 PRACTICAL EXAMPLES

### **Guild Members by Role**
```typescript
const members = new Collection();

// Filter by role
const admins = members.filter(m => 
    m.roles.cache.has('admin_role_id')
);

// Get all usernames
const usernames = members.map(m => m.user.username);

// Find specific member
const owner = members.find(m => m.id === guild.ownerId);
```

### **Command Handler**
```typescript
const commands = new Collection();

// Set commands
commands.set('ping', pingCommand);
commands.set('help', helpCommand);

// Get command
const command = commands.get(commandName);

// Check existence
if (commands.has(commandName)) {
    await commands.get(commandName).execute(message);
}
```

### **Cache Management**
```typescript
const cache = new Collection();

// Add with expiry
cache.set('key', { data, timestamp: Date.now() });

// Clean old entries
cache.sweep(item => Date.now() - item.timestamp > 3600000);

// Get cache stats
console.log(`Cache size: ${cache.size}`);
```

---

## 📊 METHOD COMPARISON

| Method | Returns | Mutates Original |
|--------|---------|------------------|
| `filter()` | New Collection | No |
| `map()` | Array | No |
| `find()` | Value or undefined | No |
| `some()` | Boolean | No |
| `every()` | Boolean | No |
| `reduce()` | Any | No |
| `sweep()` | Number | Yes |
| `tap()` | Same Collection | No |
| `clone()` | New Collection | No |
| `sort()` | Same Collection | Yes |
| `sorted()` | New Collection | No |

---

## 🔗 RESOURCES

- **NPM:** https://www.npmjs.com/package/@discordjs/collection
- **GitHub:** https://github.com/discordjs/discord.js/tree/main/packages/collection

---

**Created:** 2025-01-09  
**Version:** @discordjs/collection v2+
