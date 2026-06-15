# Object-oriented programming ruleset

## Objects

1. **Object = isolated console program.** Talk to it only via public methods / explicit interface (its CLI). Never reach inside and grab/mutate state.
2. **One feature per object** (the `S` in SOLID).
3. **Self-sufficient after construction.** When constructor finishes, object holds all data + refs it needs. (Unity: inspector fields + `Awake`.)
4. **Object = behavior + state, together.** No behavior in a vacuum → if something happens, an object does it. No data-only objects → that's a struct. So MVC / MV* layer-splitting is a design error.
5. **Fail-fast.** Throw immediately on code-side wrong behavior. Never silent-fail / swallow. Do NOT throw on external faults (network timeout, dead wifi).

## Naming

6. **Name = 90% of the solution.** Be concrete, not abstract. Don't inflate value with clever names.
7. **No `Manager`, `Controller`, `Handler`, `Presenter`** and friends.
8. **No verb-derived names ending in `-er`** (`Loader`, `Spawner`...). Name objects by what they contain / what they are. Find the source of the behavior → that's the object.
9. **Class = exists (noun). Method = does (verb).** (Exceptions: established nouns like `Computer`, `User`.)

## Inheritance

10. **Inheritance = code reuse, NOT modeling.** "Can fly" is an interface, not a base class. Prefer interfaces over class hierarchies.

## Writing code

11. **App = composition of isolated objects.** The critical code is the glue wiring them; object internals are replaceable.
12. **Internals may be ugly** (hacks, private state, wrapped 3rd-party, even AI slop) — fine while the contract holds and it stays isolated in the object.
13. **Write from the end.** First write the call site for an algorithm that doesn't exist yet. Iterate the API until ergonomic (usually 3–5 tries). THEN implement the algorithm. Design contract before logic.

## Control flow & events

14. **Linear control flow.** Code should read top→bottom. Avoid event spaghetti / IoC inversion.
15. **UI is state, not source of truth.** Don't drive simulation from button/UI events. Simulation owns simulation.
16. **Events via Fan-out messaging.** Don't use direct callback subscriptions. Queue events; each subscriber reads its own queue in order, at its own pace (ECS-style).

## Unity specifics

17. **No statics / singletons.** Use a global prefab + ScriptableObject references (`GlobalPrefabAsset`, `ReferenceAsset`, `ReferenceSource`) instead of `static Instance`.
18. **No `FindObject(s)OfType` / `Physics.OverlapSphere` in `Update`.** Use `ReferenceAsset` / `ReferenceListAsset` (pre-registered refs).
19. **Pull a tool only when the problem materializes.** Don't pre-build frameworks for problems you don't have yet.

## Testing

20. **Testable by design.** No statics; depend on interfaces so any dep can be mocked.
21. **Run tests locally, immediately after a change** — not only on CI. CI is a gate, not a substitute.
22. **Don't over-TDD.** Not life-critical software; no extremes.
23. **PlayTesting:** let the game play itself (sped up) to confirm it broadly works + damage from changes is minimal. Doesn't need full coverage.
