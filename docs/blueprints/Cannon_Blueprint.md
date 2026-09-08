# Cannon_Blueprint

Repeated cannon firing and recoil animation.

[Back to walkthrough](README.md) · [Open Blueprint asset](../../Content/blueprints/props/Model_Cannon/Cannon_Blueprint.uasset)

The node IDs below are export indices in this saved asset. Diagrams are reconstructed from serialized Blueprint pin links, not Unreal Editor screenshots. Solid arrows show execution; dotted arrows show data. Empty event nodes remain visible in the tables.

## bulletCreate

```mermaid
flowchart TD
  n16["16: Conv_DoubleToVector"]
  n17["17: K2_GetComponentLocation"]
  n18["18: K2_GetComponentRotation"]
  n26["26: bulletCreate"]
  n30["30: Spawn Actor 0"]
  n32["32: Get projectile point"]
  n33["33: Get projectileScale"]
  n16 -.->|"ReturnValue → SpawnTransform_Scale"| n30
  n17 -.->|"ReturnValue → SpawnTransform_Location"| n30
  n18 -.->|"ReturnValue → SpawnTransform_Rotation"| n30
  n26 -->|"then → execute"| n30
  n32 -.->|"projectile point → self"| n17
  n32 -.->|"projectile point → self"| n18
  n33 -.->|"projectileScale → InDouble"| n16
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 16 | `Conv_DoubleToVector` | `InDouble` ← #33 `projectileScale` | `ReturnValue` → #30 `SpawnTransform_Scale` |
| 17 | `K2_GetComponentLocation` | `self` ← #32 `projectile point` | `ReturnValue` → #30 `SpawnTransform_Location` |
| 18 | `K2_GetComponentRotation` | `self` ← #32 `projectile point` | `ReturnValue` → #30 `SpawnTransform_Rotation` |
| 26 | `bulletCreate` | — | `then` → #30 `execute` |
| 30 | `Spawn Actor 0` | `execute` ← #26 `then`<br>`Class` = `cannon_bullet_C`<br>`SpawnTransform_Location` ← #17 `ReturnValue`<br>`SpawnTransform_Rotation` ← #18 `ReturnValue`<br>`SpawnTransform_Scale` ← #16 `ReturnValue`<br>`CollisionHandlingOverride` = `Undefined`<br>`TransformScaleMethod` = `MultiplyWithRoot` | Unconnected |
| 32 | `Get projectile point` | — | `projectile point` → #17 `self`, #18 `self` |
| 33 | `Get projectileScale` | — | `projectileScale` → #16 `InDouble` |

## cannonLocation

```mermaid
flowchart TD
  n19["19: K2_SetRelativeLocation"]
  n27["27: cannonLocation"]
  n34["34: Get Cannon_StaticMeshComponent0"]
  n27 -->|"then → execute"| n19
  n27 -.->|"NewLocation_Y → NewLocation_Y"| n19
  n34 -.->|"Cannon_StaticMeshComponent0 → self"| n19
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 19 | `K2_SetRelativeLocation` | `execute` ← #27 `then`<br>`self` ← #34 `Cannon_StaticMeshComponent0`<br>`NewLocation` = `0, 0, 0`<br>`NewLocation_X` = `0.0`<br>`NewLocation_Y` ← #27 `NewLocation_Y`<br>`NewLocation_Z` = `0.0`<br>`bSweep` = `false`<br>`bTeleport` = `false` | Unconnected |
| 27 | `cannonLocation` | — | `then` → #19 `execute`<br>`NewLocation_Y` → #19 `NewLocation_Y` |
| 34 | `Get Cannon_StaticMeshComponent0` | — | `Cannon_StaticMeshComponent0` → #19 `self` |

## EventGraph

```mermaid
flowchart TD
  n20["20: Activate"]
  n21["21: Delay"]
  n22["22: bulletCreate"]
  n23["23: cannonLocation"]
  n24["24: ReceiveBeginPlay"]
  n25["25: Sequence 0"]
  n29["29: Reroute 0"]
  n31["31: Timeline 0"]
  n35["35: Get explosion"]
  n36["36: Get duration"]
  n20 -->|"then → execute"| n22
  n21 -->|"then → execute"| n25
  n22 -->|"then → InputPin"| n29
  n24 -->|"then → execute"| n21
  n25 -->|"then_0 → execute"| n20
  n25 -->|"then_1 → PlayFromStart"| n31
  n29 -->|"OutputPin → execute"| n21
  n31 -->|"Update → execute"| n23
  n31 -.->|"shooting animation → NewLocation_Y"| n23
  n35 -.->|"explosion → self"| n20
  n36 -.->|"duration → Duration"| n21
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 20 | `Activate` | `execute` ← #25 `then_0`<br>`self` ← #35 `explosion`<br>`bReset` = `true` | `then` → #22 `execute` |
| 21 | `Delay` | `execute` ← #24 `then`, #29 `OutputPin`<br>`Duration` ← #36 `duration` | `then` → #25 `execute` |
| 22 | `bulletCreate` | `execute` ← #20 `then` | `then` → #29 `InputPin` |
| 23 | `cannonLocation` | `execute` ← #31 `Update`<br>`NewLocation_Y` ← #31 `shooting animation` | Unconnected |
| 24 | `ReceiveBeginPlay` | — | `then` → #21 `execute` |
| 25 | `Sequence 0` | `execute` ← #21 `then` | `then_0` → #20 `execute`<br>`then_1` → #31 `PlayFromStart` |
| 29 | `Reroute 0` | `InputPin` ← #22 `then` | `OutputPin` → #21 `execute` |
| 31 | `Timeline 0` | `PlayFromStart` ← #25 `then_1`<br>`NewTime` = `0.0` | `Update` → #23 `execute`<br>`shooting animation` → #23 `NewLocation_Y` |
| 35 | `Get explosion` | — | `explosion` → #20 `self` |
| 36 | `Get duration` | — | `duration` → #21 `Duration` |

## UserConstructionScript

No connected node flow in this graph.

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 28 | `UserConstructionScript` | — | Unconnected |

## Reading this export

A connected input gets its value from the linked node; its unused editor default is not shown in the table. Class defaults can be overridden by placed actors in a map. A disconnected output does not execute another node. This is a static graph inspection, not a runtime test.
