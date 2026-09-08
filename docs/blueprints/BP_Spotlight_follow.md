# BP_Spotlight_follow

Smooth spotlight tracking inside a trigger.

[Back to walkthrough](README.md) · [Open Blueprint asset](../../Content/blueprints/props/BP_Spotlight_follow.uasset)

The node IDs below are export indices in this saved asset. Diagrams are reconstructed from serialized Blueprint pin links, not Unreal Editor screenshots. Solid arrows show execution; dotted arrows show data. Empty event nodes remain visible in the tables.

## EventGraph

```mermaid
flowchart TD
  n11["11: K2_SetWorldRotation"]
  n12["12: FindLookAtRotation"]
  n13["13: K2_GetComponentLocation"]
  n14["14: K2_GetActorLocation"]
  n15["15: GetPlayerPawn"]
  n16["16: RInterpTo"]
  n17["17: K2_GetComponentRotation"]
  n18["18: ReceiveActorBeginOverlap"]
  n19["19: ReceiveTick"]
  n20["20: ReceiveActorEndOverlap"]
  n22["22: Branch 0"]
  n23["23: isplayer"]
  n24["24: isplayer"]
  n25["25: Get IsPlayerOverlapping"]
  n26["26: Get SpotLight"]
  n27["27: Get SpotLight"]
  n28["28: Get SpotLight"]
  n29["29: Set IsPlayerOverlapping"]
  n30["30: Set IsPlayerOverlapping"]
  n12 -.->|"ReturnValue → Target"| n16
  n13 -.->|"ReturnValue → Start"| n12
  n14 -.->|"ReturnValue → Target"| n12
  n15 -.->|"ReturnValue → self"| n14
  n16 -.->|"ReturnValue → NewRotation"| n11
  n17 -.->|"ReturnValue → Current"| n16
  n18 -->|"then → execute"| n23
  n18 -.->|"OtherActor → A"| n23
  n19 -->|"then → execute"| n22
  n19 -.->|"DeltaSeconds → DeltaTime"| n16
  n20 -->|"then → execute"| n24
  n20 -.->|"OtherActor → A"| n24
  n22 -->|"then → execute"| n11
  n23 -->|"true → execute"| n29
  n24 -->|"true → execute"| n30
  n25 -.->|"IsPlayerOverlapping → Condition"| n22
  n26 -.->|"SpotLight → self"| n11
  n27 -.->|"SpotLight → self"| n13
  n28 -.->|"SpotLight → self"| n17
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 11 | `K2_SetWorldRotation` | `execute` ← #22 `then`<br>`self` ← #26 `SpotLight`<br>`NewRotation` ← #16 `ReturnValue`<br>`bSweep` = `false`<br>`bTeleport` = `false` | Unconnected |
| 12 | `FindLookAtRotation` | `Start` ← #13 `ReturnValue`<br>`Target` ← #14 `ReturnValue` | `ReturnValue` → #16 `Target` |
| 13 | `K2_GetComponentLocation` | `self` ← #27 `SpotLight` | `ReturnValue` → #12 `Start` |
| 14 | `K2_GetActorLocation` | `self` ← #15 `ReturnValue` | `ReturnValue` → #12 `Target` |
| 15 | `GetPlayerPawn` | `PlayerIndex` = `0` | `ReturnValue` → #14 `self` |
| 16 | `RInterpTo` | `Current` ← #17 `ReturnValue`<br>`Target` ← #12 `ReturnValue`<br>`DeltaTime` ← #19 `DeltaSeconds`<br>`InterpSpeed` = `5.000000` | `ReturnValue` → #11 `NewRotation` |
| 17 | `K2_GetComponentRotation` | `self` ← #28 `SpotLight` | `ReturnValue` → #16 `Current` |
| 18 | `ReceiveActorBeginOverlap` | — | `then` → #23 `execute`<br>`OtherActor` → #23 `A` |
| 19 | `ReceiveTick` | — | `then` → #22 `execute`<br>`DeltaSeconds` → #16 `DeltaTime` |
| 20 | `ReceiveActorEndOverlap` | — | `then` → #24 `execute`<br>`OtherActor` → #24 `A` |
| 22 | `Branch 0` | `execute` ← #19 `then`<br>`Condition` ← #25 `IsPlayerOverlapping` | `then` → #11 `execute` |
| 23 | `isplayer` | `execute` ← #18 `then`<br>`A` ← #18 `OtherActor` | `true` → #29 `execute` |
| 24 | `isplayer` | `execute` ← #20 `then`<br>`A` ← #20 `OtherActor` | `true` → #30 `execute` |
| 25 | `Get IsPlayerOverlapping` | — | `IsPlayerOverlapping` → #22 `Condition` |
| 26 | `Get SpotLight` | — | `SpotLight` → #11 `self` |
| 27 | `Get SpotLight` | — | `SpotLight` → #13 `self` |
| 28 | `Get SpotLight` | — | `SpotLight` → #17 `self` |
| 29 | `Set IsPlayerOverlapping` | `execute` ← #23 `true`<br>`IsPlayerOverlapping` = `true` | Unconnected |
| 30 | `Set IsPlayerOverlapping` | `execute` ← #24 `true`<br>`IsPlayerOverlapping` = `false` | Unconnected |

## UserConstructionScript

No connected node flow in this graph.

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 21 | `UserConstructionScript` | — | Unconnected |

## Reading this export

A connected input gets its value from the linked node; its unused editor default is not shown in the table. Class defaults can be overridden by placed actors in a map. A disconnected output does not execute another node. This is a static graph inspection, not a runtime test.
