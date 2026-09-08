# BP_UI

Crystal counter, timer and win feedback.

[Back to walkthrough](README.md) · [Open Blueprint asset](../../Content/blueprints/props/BP_UI.uasset)

The node IDs below are export indices in this saved asset. Diagrams are reconstructed from serialized Blueprint pin links, not Unreal Editor screenshots. Solid arrows show execution; dotted arrows show data. Empty event nodes remain visible in the tables.

## EventGraph

```mermaid
flowchart TD
  n27["27: GetCurrentLevelName"]
  n39["39: Construct"]
  n40["40: Tick"]
  n49["49: Branch 0"]
  n51["51: Get_Game_Istance"]
  n52["52: Get_Game_Istance"]
  n55["55: Add_DoubleDouble"]
  n58["58: Get LevelTime"]
  n59["59: Get GameTime"]
  n60["60: Get IsWin"]
  n66["66: Set LevelName"]
  n67["67: Set LevelTime"]
  n68["68: Set LevelTime"]
  n27 -->|"then → execute"| n66
  n27 -.->|"ReturnValue → LevelName"| n66
  n39 -->|"then → execute"| n27
  n40 -->|"then → execute"| n49
  n40 -.->|"InDeltaTime → A"| n55
  n49 -->|"else → execute"| n67
  n51 -.->|"AsBP Game Instance → self"| n59
  n52 -.->|"AsBP Game Instance → self"| n60
  n55 -.->|"ReturnValue → LevelTime"| n67
  n58 -.->|"LevelTime → B"| n55
  n59 -.->|"GameTime → LevelTime"| n68
  n60 -.->|"IsWin → Condition"| n49
  n66 -->|"then → execute"| n68
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 27 | `GetCurrentLevelName` | `execute` ← #39 `then`<br>`bRemovePrefixString` = `true` | `then` → #66 `execute`<br>`ReturnValue` → #66 `LevelName` |
| 38 | `PreConstruct` | — | Unconnected |
| 39 | `Construct` | — | `then` → #27 `execute` |
| 40 | `Tick` | — | `then` → #49 `execute`<br>`InDeltaTime` → #55 `A` |
| 49 | `Branch 0` | `execute` ← #40 `then`<br>`Condition` ← #60 `IsWin` | `else` → #67 `execute` |
| 51 | `Get_Game_Istance` | — | `AsBP Game Instance` → #59 `self` |
| 52 | `Get_Game_Istance` | — | `AsBP Game Instance` → #60 `self` |
| 55 | `Add_DoubleDouble` | `A` ← #40 `InDeltaTime`<br>`B` ← #58 `LevelTime` | `ReturnValue` → #67 `LevelTime` |
| 58 | `Get LevelTime` | — | `LevelTime` → #55 `B` |
| 59 | `Get GameTime` | `self` ← #51 `AsBP Game Instance` | `GameTime` → #68 `LevelTime` |
| 60 | `Get IsWin` | `self` ← #52 `AsBP Game Instance` | `IsWin` → #49 `Condition` |
| 66 | `Set LevelName` | `execute` ← #27 `then`<br>`LevelName` ← #27 `ReturnValue` | `then` → #68 `execute` |
| 67 | `Set LevelTime` | `execute` ← #49 `else`<br>`LevelTime` ← #55 `ReturnValue` | Unconnected |
| 68 | `Set LevelTime` | `execute` ← #66 `then`<br>`LevelTime` ← #59 `GameTime` | Unconnected |

## Get_CrystalCollected_Text

```mermaid
flowchart TD
  n28["28: Conv_IntToString"]
  n29["29: Conv_IntToString"]
  n30["30: Conv_StringToText"]
  n35["35: Concat_StrStr"]
  n41["41: Get_CrystalCollected_Text"]
  n45["45: Get_CrystalCollected_Text"]
  n53["53: Get_Game_Istance"]
  n61["61: Get Collected_Cristal_Number"]
  n62["62: Get AllCristalNumber"]
  n28 -.->|"ReturnValue → A"| n35
  n29 -.->|"ReturnValue → C"| n35
  n30 -.->|"ReturnValue → ReturnValue"| n45
  n35 -.->|"ReturnValue → InString"| n30
  n41 -->|"then → execute"| n45
  n53 -.->|"AsBP Game Instance → self"| n61
  n53 -.->|"AsBP Game Instance → self"| n62
  n61 -.->|"Collected_Cristal_Number → InInt"| n28
  n62 -.->|"AllCristalNumber → InInt"| n29
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 28 | `Conv_IntToString` | `InInt` ← #61 `Collected_Cristal_Number` | `ReturnValue` → #35 `A` |
| 29 | `Conv_IntToString` | `InInt` ← #62 `AllCristalNumber` | `ReturnValue` → #35 `C` |
| 30 | `Conv_StringToText` | `InString` ← #35 `ReturnValue` | `ReturnValue` → #45 `ReturnValue` |
| 35 | `Concat_StrStr` | `A` ← #28 `ReturnValue`<br>`B` = `/`<br>`C` ← #29 `ReturnValue` | `ReturnValue` → #30 `InString` |
| 41 | `Get_CrystalCollected_Text` | — | `then` → #45 `execute` |
| 45 | `Get_CrystalCollected_Text` | `execute` ← #41 `then`<br>`ReturnValue` ← #30 `ReturnValue` | Unconnected |
| 53 | `Get_Game_Istance` | — | `AsBP Game Instance` → #61 `self`, #62 `self` |
| 61 | `Get Collected_Cristal_Number` | `self` ← #53 `AsBP Game Instance` | `Collected_Cristal_Number` → #28 `InInt` |
| 62 | `Get AllCristalNumber` | `self` ← #53 `AsBP Game Instance` | `AllCristalNumber` → #29 `InInt` |

## Get_Game_Istance

```mermaid
flowchart TD
  n31["31: GetGameInstance"]
  n37["37: Cast 0"]
  n57["57: Graph boundary 1"]
  n31 -.->|"ReturnValue → Object"| n37
  n37 -.->|"AsBP Game Instance → AsBP Game Instance"| n57
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 31 | `GetGameInstance` | — | `ReturnValue` → #37 `Object` |
| 37 | `Cast 0` | `Object` ← #31 `ReturnValue` | `AsBP Game Instance` → #57 `AsBP Game Instance` |
| 56 | `Graph boundary 0` | — | Unconnected |
| 57 | `Graph boundary 1` | `AsBP Game Instance` ← #37 `AsBP Game Instance` | Unconnected |

## Get_TimeText_Text

```mermaid
flowchart TD
  n32["32: TimeSecondsToString"]
  n33["33: Conv_StringToText"]
  n36["36: Concat_StrStr"]
  n42["42: Get_TimeText_Text"]
  n46["46: Get_TimeText_Text"]
  n63["63: Get LevelTime"]
  n32 -.->|"ReturnValue → B"| n36
  n33 -.->|"ReturnValue → ReturnValue"| n46
  n36 -.->|"ReturnValue → InString"| n33
  n42 -->|"then → execute"| n46
  n63 -.->|"LevelTime → InSeconds"| n32
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 32 | `TimeSecondsToString` | `InSeconds` ← #63 `LevelTime` | `ReturnValue` → #36 `B` |
| 33 | `Conv_StringToText` | `InString` ← #36 `ReturnValue` | `ReturnValue` → #46 `ReturnValue` |
| 36 | `Concat_StrStr` | `A` = `Time: `<br>`B` ← #32 `ReturnValue` | `ReturnValue` → #33 `InString` |
| 42 | `Get_TimeText_Text` | — | `then` → #46 `execute` |
| 46 | `Get_TimeText_Text` | `execute` ← #42 `then`<br>`ReturnValue` ← #33 `ReturnValue` | Unconnected |
| 63 | `Get LevelTime` | — | `LevelTime` → #32 `InSeconds` |

## PlayFlicerAnimation

```mermaid
flowchart TD
  n34["34: PlayAnimation"]
  n44["44: PlayFlicerAnimation"]
  n65["65: Get Flicker"]
  n44 -->|"then → execute"| n34
  n65 -.->|"Flicker → InAnimation"| n34
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 34 | `PlayAnimation` | `execute` ← #44 `then`<br>`InAnimation` ← #65 `Flicker`<br>`StartAtTime` = `0.000000`<br>`NumLoopsToPlay` = `1`<br>`PlayMode` = `Forward`<br>`PlaybackSpeed` = `1.000000`<br>`bRestoreState` = `false` | Unconnected |
| 44 | `PlayFlicerAnimation` | — | `then` → #34 `execute` |
| 65 | `Get Flicker` | — | `Flicker` → #34 `InAnimation` |

## Get_WINTEXT_Visibility

```mermaid
flowchart TD
  n43["43: Get_WINTEXT_Visibility"]
  n47["47: Get_WINTEXT_Visibility"]
  n48["48: Get_WINTEXT_Visibility"]
  n50["50: Branch 0"]
  n54["54: Get_Game_Istance"]
  n64["64: Get IsWin"]
  n43 -->|"then → execute"| n50
  n50 -->|"then → execute"| n47
  n50 -->|"else → execute"| n48
  n54 -.->|"AsBP Game Instance → self"| n64
  n64 -.->|"IsWin → Condition"| n50
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 43 | `Get_WINTEXT_Visibility` | — | `then` → #50 `execute` |
| 47 | `Get_WINTEXT_Visibility` | `execute` ← #50 `then`<br>`ReturnValue` = `Visible` | Unconnected |
| 48 | `Get_WINTEXT_Visibility` | `execute` ← #50 `else`<br>`ReturnValue` = `Hidden` | Unconnected |
| 50 | `Branch 0` | `execute` ← #43 `then`<br>`Condition` ← #64 `IsWin` | `then` → #47 `execute`<br>`else` → #48 `execute` |
| 54 | `Get_Game_Istance` | — | `AsBP Game Instance` → #64 `self` |
| 64 | `Get IsWin` | `self` ← #54 `AsBP Game Instance` | `IsWin` → #50 `Condition` |

## Reading this export

A connected input gets its value from the linked node; its unused editor default is not shown in the table. Class defaults can be overridden by placed actors in a map. A disconnected output does not execute another node. This is a static graph inspection, not a runtime test.
