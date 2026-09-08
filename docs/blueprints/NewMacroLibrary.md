# NewMacroLibrary

Shared player checks, camera fades and restart logic.

[Back to walkthrough](README.md) · [Open Blueprint asset](../../Content/blueprints/props/NewMacroLibrary.uasset)

The node IDs below are export indices in this saved asset. Diagrams are reconstructed from serialized Blueprint pin links, not Unreal Editor screenshots. Solid arrows show execution; dotted arrows show data. Empty event nodes remain visible in the tables.

## ConvertGameInstance

```mermaid
flowchart TD
  n7["7: GetGameInstance"]
  n15["15: Cast 0"]
  n20["20: Graph boundary 0"]
  n21["21: Graph boundary 1"]
  n7 -.->|"ReturnValue → Object"| n15
  n15 -->|"then → then"| n21
  n15 -.->|"AsBP Game Instance → AsBP Game Instance"| n21
  n20 -->|"execute → execute"| n15
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 7 | `GetGameInstance` | — | `ReturnValue` → #15 `Object` |
| 15 | `Cast 0` | `execute` ← #20 `execute`<br>`Object` ← #7 `ReturnValue` | `then` → #21 `then`<br>`AsBP Game Instance` → #21 `AsBP Game Instance` |
| 20 | `Graph boundary 0` | — | `execute` → #15 `execute` |
| 21 | `Graph boundary 1` | `then` ← #15 `then`<br>`AsBP Game Instance` ← #15 `AsBP Game Instance` | Unconnected |

## isplayer

```mermaid
flowchart TD
  n8["8: GetPlayerPawn"]
  n16["16: Branch 0"]
  n19["19: EqualEqual_ObjectObject"]
  n22["22: Graph boundary 0"]
  n23["23: Graph boundary 1"]
  n8 -.->|"ReturnValue → B"| n19
  n16 -->|"then → true"| n23
  n16 -->|"else → false"| n23
  n19 -.->|"ReturnValue → Condition"| n16
  n22 -->|"execute → execute"| n16
  n22 -.->|"A → A"| n19
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 8 | `GetPlayerPawn` | `PlayerIndex` = `0` | `ReturnValue` → #19 `B` |
| 16 | `Branch 0` | `execute` ← #22 `execute`<br>`Condition` ← #19 `ReturnValue` | `then` → #23 `true`<br>`else` → #23 `false` |
| 19 | `EqualEqual_ObjectObject` | `A` ← #22 `A`<br>`B` ← #8 `ReturnValue` | `ReturnValue` → #16 `Condition` |
| 22 | `Graph boundary 0` | — | `execute` → #16 `execute`<br>`A` → #19 `A` |
| 23 | `Graph boundary 1` | `true` ← #16 `then`<br>`false` ← #16 `else` | Unconnected |

## MakeCameraDark

```mermaid
flowchart TD
  n9["9: GetPlayerCameraManager"]
  n10["10: StartCameraFade"]
  n24["24: Graph boundary 0"]
  n25["25: Graph boundary 1"]
  n9 -.->|"ReturnValue → self"| n10
  n10 -->|"then → then"| n25
  n24 -->|"execute → execute"| n10
  n24 -.->|"FromAlpha → FromAlpha"| n10
  n24 -.->|"ToAlpha → ToAlpha"| n10
  n24 -.->|"Duration → Duration"| n10
  n24 -.->|"Color → Color"| n10
  n24 -.->|"bShouldFadeAudio → bShouldFadeAudio"| n10
  n24 -.->|"bHoldWhenFinished → bHoldWhenFinished"| n10
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 9 | `GetPlayerCameraManager` | `PlayerIndex` = `0` | `ReturnValue` → #10 `self` |
| 10 | `StartCameraFade` | `execute` ← #24 `execute`<br>`self` ← #9 `ReturnValue`<br>`FromAlpha` ← #24 `FromAlpha`<br>`ToAlpha` ← #24 `ToAlpha`<br>`Duration` ← #24 `Duration`<br>`Color` ← #24 `Color`<br>`bShouldFadeAudio` ← #24 `bShouldFadeAudio`<br>`bHoldWhenFinished` ← #24 `bHoldWhenFinished` | `then` → #25 `then` |
| 17 | `RestartLevel` | — | Unconnected |
| 24 | `Graph boundary 0` | — | `execute` → #10 `execute`<br>`FromAlpha` → #10 `FromAlpha`<br>`ToAlpha` → #10 `ToAlpha`<br>`Duration` → #10 `Duration`<br>`Color` → #10 `Color`<br>`bShouldFadeAudio` → #10 `bShouldFadeAudio`<br>`bHoldWhenFinished` → #10 `bHoldWhenFinished` |
| 25 | `Graph boundary 1` | `then` ← #10 `then` | Unconnected |

## RestartLevel

```mermaid
flowchart TD
  n11["11: GetCurrentLevelName"]
  n12["12: OpenLevel"]
  n13["13: Conv_StringToName"]
  n14["14: MakeLiteralInt"]
  n18["18: ConvertGameInstance"]
  n26["26: Graph boundary 0"]
  n27["27: Graph boundary 1"]
  n28["28: Set Collected_Cristal_Number"]
  n11 -->|"then → execute"| n12
  n11 -.->|"ReturnValue → InString"| n13
  n12 -->|"then → execute"| n18
  n13 -.->|"ReturnValue → LevelName"| n12
  n14 -.->|"ReturnValue → Collected_Cristal_Number"| n28
  n18 -->|"then → execute"| n28
  n18 -.->|"AsBP Game Instance → self"| n28
  n26 -->|"Execute → execute"| n11
  n28 -->|"then → then"| n27
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 11 | `GetCurrentLevelName` | `execute` ← #26 `Execute`<br>`bRemovePrefixString` = `true` | `then` → #12 `execute`<br>`ReturnValue` → #13 `InString` |
| 12 | `OpenLevel` | `execute` ← #11 `then`<br>`LevelName` ← #13 `ReturnValue`<br>`bAbsolute` = `true` | `then` → #18 `execute` |
| 13 | `Conv_StringToName` | `InString` ← #11 `ReturnValue` | `ReturnValue` → #12 `LevelName` |
| 14 | `MakeLiteralInt` | `Value` = `0` | `ReturnValue` → #28 `Collected_Cristal_Number` |
| 18 | `ConvertGameInstance` | `execute` ← #12 `then` | `then` → #28 `execute`<br>`AsBP Game Instance` → #28 `self` |
| 26 | `Graph boundary 0` | — | `Execute` → #11 `execute` |
| 27 | `Graph boundary 1` | `then` ← #28 `then` | Unconnected |
| 28 | `Set Collected_Cristal_Number` | `execute` ← #18 `then`<br>`Collected_Cristal_Number` ← #14 `ReturnValue`<br>`self` ← #18 `AsBP Game Instance` | `then` → #27 `then` |

## Reading this export

A connected input gets its value from the linked node; its unused editor default is not shown in the table. Class defaults can be overridden by placed actors in a map. A disconnected output does not execute another node. This is a static graph inspection, not a runtime test.
