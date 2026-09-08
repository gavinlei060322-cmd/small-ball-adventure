# cannon_bullet

Projectile movement and hit handling.

[Back to walkthrough](README.md) · [Open Blueprint asset](../../Content/blueprints/props/cannon_bullet.uasset)

The node IDs below are export indices in this saved asset. Diagrams are reconstructed from serialized Blueprint pin links, not Unreal Editor screenshots. Solid arrows show execution; dotted arrows show data. Empty event nodes remain visible in the tables.

## EventGraph

```mermaid
flowchart TD
  n8["8: K2_DestroyActor"]
  n9["9: GetPlayerPawn"]
  n10["10: ApplyDamage"]
  n11["11: GetPlayerPawn"]
  n12["12: ReceiveHit"]
  n14["14: Branch 0"]
  n15["15: EqualEqual_ObjectObject"]
  n8 -->|"then → execute"| n14
  n9 -.->|"ReturnValue → B"| n15
  n11 -.->|"ReturnValue → DamagedActor"| n10
  n12 -->|"then → execute"| n8
  n12 -.->|"Other → A"| n15
  n14 -->|"then → execute"| n10
  n15 -.->|"ReturnValue → Condition"| n14
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 8 | `K2_DestroyActor` | `execute` ← #12 `then` | `then` → #14 `execute` |
| 9 | `GetPlayerPawn` | `PlayerIndex` = `0` | `ReturnValue` → #15 `B` |
| 10 | `ApplyDamage` | `execute` ← #14 `then`<br>`DamagedActor` ← #11 `ReturnValue`<br>`BaseDamage` = `1.000000` | Unconnected |
| 11 | `GetPlayerPawn` | `PlayerIndex` = `0` | `ReturnValue` → #10 `DamagedActor` |
| 12 | `ReceiveHit` | — | `then` → #8 `execute`<br>`Other` → #15 `A` |
| 14 | `Branch 0` | `execute` ← #8 `then`<br>`Condition` ← #15 `ReturnValue` | `then` → #10 `execute` |
| 15 | `EqualEqual_ObjectObject` | `A` ← #12 `Other`<br>`B` ← #9 `ReturnValue` | `ReturnValue` → #14 `Condition` |

## UserConstructionScript

No connected node flow in this graph.

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 13 | `UserConstructionScript` | — | Unconnected |

## Reading this export

A connected input gets its value from the linked node; its unused editor default is not shown in the table. Class defaults can be overridden by placed actors in a map. A disconnected output does not execute another node. This is a static graph inspection, not a runtime test.
