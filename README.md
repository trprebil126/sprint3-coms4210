# AC Attacks vs. Side-Channel Attacks — Interactive Teaching Module

A single-file, browser-based visualization that makes two hard topics from the algorithmic-complexity and side-channel-attacks module easier to understand. Built with plain HTML/CSS/JavaScript — no build step, no dependencies.

## Run it

Open `index.html` in any standard browser. That's it.

> If your browser restricts `file://` pages, serve the folder instead:
>
> ```powershell
> python -m http.server 8765
> # then visit http://localhost:8765/index.html
> ```

## What's inside

The app has three tabs:

### 📊 Compare
An interactive side-by-side comparison of the two vulnerability classes across six dimensions — primary C-I-A target, flaw type, CWE mapping, attacker interaction, impact, and primary countermeasure — plus short sections on the key differences and the defensive trade-offs between them. Click any table row to highlight it.

| Dimension | AC Attacks | Side-Channel Attacks |
|---|---|---|
| Primary Target | **Availability** | **Confidentiality** |
| Flaw Type | Worst-case algorithmic complexity in logic | Unintended physical/behavioral traces |
| CWE | **CWE-407** Inefficient Algorithmic Complexity | **CWE-208** Observable Timing Discrepancy |
| Interaction | **Active** — crafted corner-case inputs | **Passive/Active** — measures timing, power, cache |
| Impact | Slowdown, thread locking, crash | Secret recovery (passwords, keys) |
| Countermeasure | Better algorithms, input sanitization, timeouts | **Constant-time** ops, padding, noise |

### 🔥 AC Attack Simulator
An interactive demo of an Algorithmic Complexity vulnerability (CWE-407), based on a real class exercise. A catalog links 6 shapes into a singly linked list and traverses it with `while(c != null)` — an O(n) walk. But `Rectangle` stores its `next` pointer in a **static** field shared by *all* Rectangles. Use the two dropdowns to place Rectangles in the list, then run the traversal. When two Rectangles are placed with the last one at index 5, the shared pointer forms a **cycle**, the "linear" traversal never terminates, and only the `totalShapesCount > shapes.length` timeout guard fires — turning O(n) into O(∞). Watch the step counter blow past the list size.

### 🕵️ Side-Channel Simulator
An interactive timing-attack demo (CWE-208). A server checks a 4-digit PIN one character at a time and returns as soon as a character is wrong — so guesses with more correct leading digits take measurably **longer**. Set a secret PIN and run the attacker: it measures response time for every digit at every position and recovers the PIN in at most 40 timed guesses instead of 10,000, observing nothing but time. Toggle **constant-time check** to see the countermeasure flatten the timing signal and defeat the attack.

## Files

- `index.html` — the entire app (markup, styles, and logic in one file)
- `README.md` — this file
- `reflection.md` — a one-page reflection on the build experience and what I learned

## What I learned

Building the simulators forced me to get the mechanics exactly right: the AC demo only cycles when the shared `static next` pointer is overwritten in a specific order, and the timing attack only works because of the early-exit comparison. Turning those into interactive, step-by-step visuals made the difference between "availability vs. confidentiality" and "intended logic vs. unintended side-effects" much more concrete than reading a table.
