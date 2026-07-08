# Artifacts & Relics

A scaffolded progression layer: **relics dropped from kills grant small passive bonuses**.

## How it works

- An **`ArtifactInventory` autoload** holds collected relics.
- Some enemies **carry** artifacts (the carry system, kept when recipes were removed; see [Enemies](../game/enemies.md)).
- Killing a carrier **drops** the relic → the player collects it → it **grants a passive bonus**.
- UI: a right-edge **✦ tab/panel** below the spellbook.

The **drop → grant → bonus** loop works, with gold and tower-HP bonuses live. A `DebugPanel` "Grant artifact" action exists for testing.

!!! warning "Scaffold: most content pending"
    This is an early scaffold. Still to do: tower markers, the real relic **catalogue**, and most of the bonus wiring beyond the couple that are live. Treat artifacts as a proven-loop prototype, not a finished system.

## Related

- [Enemies](../game/enemies.md): the carry system that feeds drops.
- [Backlog](../project/backlog.md): remaining artifact work.
