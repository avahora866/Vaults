# Genuine concept gaps (worth addressing)
**1. Livelock — completely missing**
Your old `Livelocks and starvation.md` covers it well, but it appears nowhere in your new notes. This is a real gap because livelock is conceptually distinct from deadlock (active vs blocked) and your trunk note doesn't mention it. The hallway sidestepping example, the "overly polite retry loop" pattern, and resolution strategies (randomization/backoff/jitter) aren't anywhere in the new structure.

**2. Starvation — only mentioned in passing**
Your new `Synchronisation.md` mentions starvation once, in the context of resource preemption during deadlock recovery. The old notes give it dedicated treatment: causes (priority scheduling without aging, non-fair locks, unbounded waiting) and resolution strategies (fair locks, aging, round-robin, bounded wait). Your trunk note even has a `[[Fairness]]` link that goes nowhere on this topic.

**3. Critical section — no dedicated treatment**
Your new `Synchronisation.md` uses the term casually but never defines it. The old `Critical section.md` covers the formal Entry/Critical/Exit/Remainder structure, the granularity tradeoff (smaller = more concurrency, larger = less risk), and — crucially — the **check-then-act anti-pattern** (`if counter < MAX: counter += 1` without a lock) which is one of the most common real-world race condition patterns and isn't in your new race conditions note.

**4. Priority inversion — not mentioned anywhere**
Old `Mutual exclusion mechanisms.md` lists this as a common lock challenge. It's missing entirely from new notes. Worth at least a paragraph since it's a real-world failure mode (Mars Pathfinder, etc.).

# Smaller gaps (your call whether to bother)

**5. Optimistic vs Pessimistic locking as a category** — Old `Lock.md` covers this. New notes only touch optimistic concurrency implicitly via CAS. Worth adding because it's how databases and version-control systems are commonly described.

**6. Race condition debugging tools** — ThreadSanitizer, static analyzers, stress testing. Practical and missing from new `Race Conditions.md`.

**7. Best practices for locks** — try/finally release, avoid nested locks, keep critical sections short. Old has these as an explicit section; new has them implied throughout but not consolidated.

**8. Immutability as race-prevention strategy** — Listed in old notes as one of the main prevention techniques. New `Race Conditions.md` only lists synchronised blocks, locks, and atomic variables — missing immutability and "avoid shared state entirely" as design-level approaches.

