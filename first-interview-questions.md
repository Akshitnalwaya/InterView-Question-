# 📘 JavaScript: Using Objects as Keys (Detailed Summary)

## 🔑 1. Can You Use an Object as a Key in Another Object?

In plain JavaScript objects `{}`, keys must be **strings or symbols**.  
When you use an object as a key, JavaScript automatically converts it to a string using `.toString()`.

```js
const obj = {};
const a = { key: "a" };
obj[a] = 123;

console.log(obj); // { '[object Object]': 123 }
📦 2. What is [object Object]?
This is the default string representation of any object:

js
Copy
Edit
const a = { key: "a" };
console.log(a.toString()); // "[object Object]"
This comes from:

js
Copy
Edit
Object.prototype.toString.call(a); // "[object Object]"
It combines:

"object" (type)

"Object" (internal class)

⚠️ 3. Why Can This Cause Bugs?
If you use two different objects as keys:

js
Copy
Edit
const obj = {};
const a = { key: "a" };
const b = { key: "b" };

obj[a] = 123;
obj[b] = 456;

console.log(obj[a]); // 456 (not 123!)
Why? Because both a and b become the same key:

js
Copy
Edit
a.toString() === b.toString(); // true → "[object Object]"
So the value gets overwritten.

✅ 4. Why Does obj[a] = 123 Work Even If obj is Empty?
JavaScript objects are dynamic.
You can add keys anytime — even if they don't exist yet.

js
Copy
Edit
const obj = {};
obj["newKey"] = 100;  // Adds a key dynamically
Same applies when a is an object:

js
Copy
Edit
obj[a] = 123;  // key created as "[object Object]"
🛠️ 5. Proper Way to Use Object as a Key: Use Map
JavaScript's Map allows real object references as keys:

js
Copy
Edit
const a = { key: "a" };
const b = { key: "b" };
const map = new Map();

map.set(a, 123);
map.set(b, 456);

console.log(map.get(a)); // 123
console.log(map.get(b)); // 456
No string conversion — actual object identity is preserved.

✨ 6. Customize .toString() (Optional)
You can override .toString() in your object to change what gets used as a key:

js
Copy
Edit
const user = {
  name: "Akshit",
  toString() {
    return `User:${this.name}`;
  }
};

const obj = {};
obj[user] = "Hello";

console.log(obj); // { 'User:Akshit': 'Hello' }
✅ Final Summary
Concept	Explanation
obj[a] = value	Adds a new key dynamically
Objects as keys	Converted to strings → [object Object]
Overwriting issue	Different objects stringify the same way
Safer alternative	Use Map to preserve object identity

💡 Rule of Thumb: Never use objects as keys in plain objects. Use Map instead to avoid bugs and key collisions.
