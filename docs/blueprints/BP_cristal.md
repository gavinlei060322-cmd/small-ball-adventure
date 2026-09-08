# BP_cristal

Crystal collection and animation.

[Back to walkthrough](README.md) · [Open Blueprint asset](../../Content/blueprints/props/BP_cristal.uasset)

The node IDs below are export indices in this saved asset. Diagrams are reconstructed from serialized Blueprint pin links, not Unreal Editor screenshots. Solid arrows show execution; dotted arrows show data. Empty event nodes remain visible in the tables.

## EventGraph

```mermaid
flowchart TD
  n20["20: get location"]
  n21["21: K2_DestroyActor"]
  n22["22: scale_animation"]
  n23["23: intensity_animation"]
  n24["24: Increase_number_Crystal"]
  n35["35: Collapsed graph 0"]
  n36["36: ReceiveActorBeginOverlap"]
  n41["41: ConvertGameInstance"]
  n42["42: isplayer"]
  n43["43: Timeline 0"]
  n20 -->|"then → execute"| n22
  n22 -->|"then → execute"| n23
  n24 -->|"then → execute"| n21
  n35 -->|"then → Play"| n43
  n36 -->|"then → execute"| n42
  n36 -.->|"OtherActor → A"| n42
  n41 -->|"then → execute"| n24
  n41 -.->|"AsBP Game Instance → self"| n24
  n42 -->|"true → execute"| n35
  n43 -->|"Update → execute"| n20
  n43 -->|"Finished → execute"| n41
  n43 -.->|"location alpha → Alpha"| n20
  n43 -.->|"scale alpha → Alpha"| n22
  n43 -.->|"NewTrack_2 → Alpha"| n23
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 20 | `get location` | `execute` ← #43 `Update`<br>`Alpha` ← #43 `location alpha` | `then` → #22 `execute` |
| 21 | `K2_DestroyActor` | `execute` ← #24 `then` | Unconnected |
| 22 | `scale_animation` | `execute` ← #20 `then`<br>`Alpha` ← #43 `scale alpha` | `then` → #23 `execute` |
| 23 | `intensity_animation` | `execute` ← #22 `then`<br>`Alpha` ← #43 `NewTrack_2` | Unconnected |
| 24 | `Increase_number_Crystal` | `execute` ← #41 `then`<br>`self` ← #41 `AsBP Game Instance` | `then` → #21 `execute` |
| 35 | `Collapsed graph 0` | `execute` ← #42 `true` | `then` → #43 `Play` |
| 36 | `ReceiveActorBeginOverlap` | — | `then` → #42 `execute`<br>`OtherActor` → #42 `A` |
| 41 | `ConvertGameInstance` | `execute` ← #43 `Finished` | `then` → #24 `execute`<br>`AsBP Game Instance` → #24 `self` |
| 42 | `isplayer` | `execute` ← #36 `then`<br>`A` ← #36 `OtherActor` | `true` → #35 `execute` |
| 43 | `Timeline 0` | `Play` ← #35 `then`<br>`NewTime` = `0.0` | `Update` → #20 `execute`<br>`Finished` → #41 `execute`<br>`location alpha` → #20 `Alpha`<br>`scale alpha` → #22 `Alpha`<br>`NewTrack_2` → #23 `Alpha` |

## CollapseGraph

```mermaid
flowchart TD
  n25["25: K2_GetActorLocation"]
  n26["26: GetActorScale3D"]
  n44["44: Graph boundary 0"]
  n45["45: Graph boundary 1"]
  n46["46: Get PointLight"]
  n47["47: Get Intensity"]
  n52["52: Set startLocation"]
  n53["53: Set scale"]
  n54["54: Set intensity"]
  n25 -.->|"ReturnValue → startLocation"| n52
  n26 -.->|"ReturnValue → scale"| n53
  n44 -->|"execute → execute"| n52
  n46 -.->|"PointLight → self"| n47
  n47 -.->|"Intensity → intensity"| n54
  n52 -->|"then → execute"| n53
  n53 -->|"then → execute"| n54
  n54 -->|"then → then"| n45
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 25 | `K2_GetActorLocation` | — | `ReturnValue` → #52 `startLocation` |
| 26 | `GetActorScale3D` | — | `ReturnValue` → #53 `scale` |
| 44 | `Graph boundary 0` | — | `execute` → #52 `execute` |
| 45 | `Graph boundary 1` | `then` ← #54 `then` | Unconnected |
| 46 | `Get PointLight` | — | `PointLight` → #47 `self` |
| 47 | `Get Intensity` | `self` ← #46 `PointLight` | `Intensity` → #54 `intensity` |
| 52 | `Set startLocation` | `execute` ← #44 `execute`<br>`startLocation` ← #25 `ReturnValue` | `then` → #53 `execute` |
| 53 | `Set scale` | `execute` ← #52 `then`<br>`scale` ← #26 `ReturnValue` | `then` → #54 `execute` |
| 54 | `Set intensity` | `execute` ← #53 `then`<br>`intensity` ← #47 `Intensity` | `then` → #45 `then` |

## get location

```mermaid
flowchart TD
  n27["27: K2_SetActorLocation"]
  n28["28: VLerp"]
  n29["29: GetPlayerPawn"]
  n30["30: K2_GetActorLocation"]
  n37["37: get location"]
  n48["48: Get startLocation"]
  n28 -.->|"ReturnValue → NewLocation"| n27
  n29 -.->|"ReturnValue → self"| n30
  n30 -.->|"ReturnValue → B"| n28
  n37 -->|"then → execute"| n27
  n37 -.->|"Alpha → Alpha"| n28
  n48 -.->|"startLocation → A"| n28
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 27 | `K2_SetActorLocation` | `execute` ← #37 `then`<br>`NewLocation` ← #28 `ReturnValue`<br>`bSweep` = `false`<br>`bTeleport` = `false` | Unconnected |
| 28 | `VLerp` | `A` ← #48 `startLocation`<br>`B` ← #30 `ReturnValue`<br>`Alpha` ← #37 `Alpha` | `ReturnValue` → #27 `NewLocation` |
| 29 | `GetPlayerPawn` | `PlayerIndex` = `0` | `ReturnValue` → #30 `self` |
| 30 | `K2_GetActorLocation` | `self` ← #29 `ReturnValue` | `ReturnValue` → #28 `B` |
| 37 | `get location` | — | `then` → #27 `execute`<br>`Alpha` → #28 `Alpha` |
| 48 | `Get startLocation` | — | `startLocation` → #28 `A` |

## intensity_animation

```mermaid
flowchart TD
  n31["31: SetIntensity"]
  n32["32: Lerp"]
  n38["38: intensity_animation"]
  n49["49: Get PointLight"]
  n50["50: Get intensity"]
  n32 -.->|"ReturnValue → NewIntensity"| n31
  n38 -->|"then → execute"| n31
  n38 -.->|"Alpha → Alpha"| n32
  n49 -.->|"PointLight → self"| n31
  n50 -.->|"intensity → A"| n32
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 31 | `SetIntensity` | `execute` ← #38 `then`<br>`self` ← #49 `PointLight`<br>`NewIntensity` ← #32 `ReturnValue` | Unconnected |
| 32 | `Lerp` | `A` ← #50 `intensity`<br>`B` = `0.0`<br>`Alpha` ← #38 `Alpha` | `ReturnValue` → #31 `NewIntensity` |
| 38 | `intensity_animation` | — | `then` → #31 `execute`<br>`Alpha` → #32 `Alpha` |
| 49 | `Get PointLight` | — | `PointLight` → #31 `self` |
| 50 | `Get intensity` | — | `intensity` → #32 `A` |

## scale_animation

```mermaid
flowchart TD
  n33["33: SetActorScale3D"]
  n34["34: VLerp"]
  n39["39: scale_animation"]
  n51["51: Get scale"]
  n34 -.->|"ReturnValue → NewScale3D"| n33
  n39 -->|"then → execute"| n33
  n39 -.->|"Alpha → Alpha"| n34
  n51 -.->|"scale → A"| n34
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 33 | `SetActorScale3D` | `execute` ← #39 `then`<br>`NewScale3D` ← #34 `ReturnValue` | Unconnected |
| 34 | `VLerp` | `A` ← #51 `scale`<br>`B` = `0, 0, 0`<br>`Alpha` ← #39 `Alpha` | `ReturnValue` → #33 `NewScale3D` |
| 39 | `scale_animation` | — | `then` → #33 `execute`<br>`Alpha` → #34 `Alpha` |
| 51 | `Get scale` | — | `scale` → #34 `A` |

## UserConstructionScript

No connected node flow in this graph.

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 40 | `UserConstructionScript` | — | Unconnected |

## Reading this export

A connected input gets its value from the linked node; its unused editor default is not shown in the table. Class defaults can be overridden by placed actors in a map. A disconnected output does not execute another node. This is a static graph inspection, not a runtime test.
