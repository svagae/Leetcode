

---

### 📌 Example

```java
candidates = [2, 3, 6, 7]
target = 7
```

We'll simulate the backtracking step by step, showing:

* The current path (combination)
* The remaining target
* When it hits `break` or `return`

---

### 🧪 Let's Start (sorted candidates: \[2, 3, 6, 7])

We'll keep track of the **call stack** like this:

```plaintext
Call: combinationSum([2, 3, 6, 7], 7)
```

---

### ✅ First Level: remaining = 7, path = \[]

Loop over candidates:

#### Try candidate 2:

```plaintext
→ Choose 2 → path = [2], remaining = 5
→ recurse
```

---

### ✅ Second Level: remaining = 5, path = \[2]

Try 2 again:

```plaintext
→ Choose 2 → path = [2, 2], remaining = 3
→ recurse
```

---

### ✅ Third Level: remaining = 3, path = \[2, 2]

Try 2 again:

```plaintext
→ Choose 2 → path = [2, 2, 2], remaining = 1
→ recurse
```

---

### ✅ Fourth Level: remaining = 1, path = \[2, 2, 2]

Try 2:

```plaintext
→ 2 > 1 → 🔴 break
→ Backtrack (remove last 2)
```

Try 3:

```plaintext
→ 3 > 1 → 🔴 break
→ return to third level
```

---

### Back to Third Level: path = \[2, 2]

Try 3:

```plaintext
→ Choose 3 → path = [2, 2, 3], remaining = 0
→ ✅ Found solution → add to result
→ return
```

→ Backtrack to path = \[2, 2]

Try 6, 7:

```plaintext
→ Both > remaining → 🔴 break
→ return
```

---

### Back to Second Level: path = \[2]

Try 3:

```plaintext
→ Choose 3 → path = [2, 3], remaining = 2
→ recurse
```

#### New Level: remaining = 2, path = \[2, 3]

Try 3:

```plaintext
→ 3 > 2 → 🔴 break
→ return
```

Try 6, 7:

```plaintext
→ > 2 → 🔴 break
→ return
```

---

### Back to Second Level: path = \[2]

Try 6:

```plaintext
→ 6 > 5 → 🔴 break
→ return
```

---

### Back to First Level: path = \[]

Try 3:

```plaintext
→ Choose 3 → path = [3], remaining = 4
→ recurse
```

#### New Level: path = \[3], remaining = 4

Try 3:

```plaintext
→ Choose 3 → path = [3, 3], remaining = 1
→ recurse
```

Try 3 again:

```plaintext
→ 3 > 1 → 🔴 break
→ return
```

Try 6, 7:

```plaintext
→ > 1 → 🔴 break
→ return
```

→ backtrack path = \[3]

Try 6, 7:

```plaintext
→ 6 > 4 → 🔴 break
→ return
```

---

### Back to First Level: path = \[]

Try 6:

```plaintext
→ Choose 6 → path = [6], remaining = 1
→ recurse
```

→ all > 1 → 🔴 break → return

---

### Try 7:

```plaintext
→ Choose 7 → path = [7], remaining = 0
→ ✅ Found solution
→ return
```

---

## ✅ Final Result:

```
[[2, 2, 3], [7]]
```

---

### 💡 Summary of Flow Keywords

| What Triggered                | Action   | Why?                            |
| ----------------------------- | -------- | ------------------------------- |
| `candidate > remainingTarget` | `break`  | Further values too big (sorted) |
| `remainingTarget == 0`        | `return` | Found valid combination         |
| End of loop                   | `return` | No more options to try          |

---

