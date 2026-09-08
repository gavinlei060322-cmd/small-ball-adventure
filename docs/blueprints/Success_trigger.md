# Success_trigger

Collection gate, level progression and final victory.

[Back to walkthrough](README.md) · [Open Blueprint asset](../../Content/blueprints/props/Success_trigger.uasset)

The node IDs below are export indices in this saved asset. Diagrams are reconstructed from serialized Blueprint pin links, not Unreal Editor screenshots. Solid arrows show execution; dotted arrows show data. Empty event nodes remain visible in the tables.

## EventGraph

```mermaid
flowchart TD
  n15["15: Array_Length"]
  n19["19: Delay"]
  n20["20: MakeLiteralDouble"]
  n21["21: OpenLevel"]
  n22["22: GetAllWidgetsOfClass"]
  n23["23: GetAllActorsOfClass"]
  n24["24: PlayFlicerAnimation"]
  n25["25: IsLastLevel"]
  n26["26: GetCurrentLevelIndex"]
  n27["27: SaveLevelTime"]
  n33["33: ReceiveActorBeginOverlap"]
  n35["35: ReceiveBeginPlay"]
  n42["42: Array Get 0"]
  n43["43: Array Get 1"]
  n45["45: Branch 0"]
  n46["46: Branch 2"]
  n47["47: ConvertGameInstance"]
  n48["48: ConvertGameInstance"]
  n49["49: MakeCameraDark"]
  n50["50: IncrementInt"]
  n51["51: ConvertGameInstance"]
  n52["52: ConvertGameInstance"]
  n53["53: isplayer"]
  n55["55: EqualEqual_IntInt"]
  n57["57: Get AllCristalNumber"]
  n58["58: Get Collected_Cristal_Number"]
  n59["59: Get LevelNameList"]
  n63["63: Set IsWin"]
  n64["64: Set AllCristalNumber"]
  n65["65: Set Collected_Cristal_Number"]
  n15 -.->|"ReturnValue → AllCristalNumber"| n64
  n19 -->|"then → execute"| n21
  n20 -.->|"ReturnValue → Duration"| n49
  n20 -.->|"ReturnValue → Duration"| n19
  n21 -->|"then → execute"| n52
  n22 -->|"then → execute"| n24
  n22 -.->|"FoundWidgets → Array"| n43
  n23 -->|"then → execute"| n48
  n23 -.->|"OutActors → TargetArray"| n15
  n25 -->|"then → execute"| n46
  n25 -.->|"result → Condition"| n46
  n26 -->|"then →  "| n50
  n26 -.->|"level name → Value"| n50
  n27 -->|"then → execute"| n49
  n33 -->|"then → execute"| n53
  n33 -.->|"OtherActor → A"| n53
  n35 -->|"then → execute"| n23
  n42 -.->|"Output → LevelName"| n21
  n43 -.->|"Output → self"| n24
  n45 -->|"then → execute"| n25
  n45 -->|"else → execute"| n22
  n46 -->|"then → execute"| n51
  n46 -->|"else → execute"| n26
  n47 -->|"then → execute"| n45
  n47 -.->|"AsBP Game Instance → self"| n57
  n47 -.->|"AsBP Game Instance → self"| n58
  n48 -->|"then → execute"| n64
  n48 -.->|"AsBP Game Instance → self"| n64
  n49 -->|"then → execute"| n19
  n50 -->|"   → execute"| n27
  n50 -.->|"Result → Dimension 1"| n42
  n51 -->|"then → execute"| n63
  n51 -.->|"AsBP Game Instance → self"| n63
  n52 -->|"then → execute"| n65
  n52 -.->|"AsBP Game Instance → self"| n65
  n53 -->|"true → execute"| n47
  n55 -.->|"ReturnValue → Condition"| n45
  n57 -.->|"AllCristalNumber → A"| n55
  n58 -.->|"Collected_Cristal_Number → B"| n55
  n59 -.->|"LevelNameList → Array"| n42
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 15 | `Array_Length` | `TargetArray` ← #23 `OutActors` | `ReturnValue` → #64 `AllCristalNumber` |
| 19 | `Delay` | `execute` ← #49 `then`<br>`Duration` ← #20 `ReturnValue` | `then` → #21 `execute` |
| 20 | `MakeLiteralDouble` | `Value` = `1.000000` | `ReturnValue` → #49 `Duration`, #19 `Duration` |
| 21 | `OpenLevel` | `execute` ← #19 `then`<br>`LevelName` ← #42 `Output`<br>`bAbsolute` = `true` | `then` → #52 `execute` |
| 22 | `GetAllWidgetsOfClass` | `execute` ← #45 `else`<br>`WidgetClass` = `BP_UI_C`<br>`TopLevelOnly` = `true` | `then` → #24 `execute`<br>`FoundWidgets` → #43 `Array` |
| 23 | `GetAllActorsOfClass` | `execute` ← #35 `then`<br>`ActorClass` = `BP_cristal_C` | `then` → #48 `execute`<br>`OutActors` → #15 `TargetArray` |
| 24 | `PlayFlicerAnimation` | `execute` ← #22 `then`<br>`self` ← #43 `Output` | Unconnected |
| 25 | `IsLastLevel` | `execute` ← #45 `then` | `then` → #46 `execute`<br>`result` → #46 `Condition` |
| 26 | `GetCurrentLevelIndex` | `execute` ← #46 `else` | `then` → #50 ` `<br>`level name` → #50 `Value` |
| 27 | `SaveLevelTime` | `execute` ← #50 `  ` | `then` → #49 `execute` |
| 33 | `ReceiveActorBeginOverlap` | — | `then` → #53 `execute`<br>`OtherActor` → #53 `A` |
| 34 | `ReceiveTick` | — | Unconnected |
| 35 | `ReceiveBeginPlay` | — | `then` → #23 `execute` |
| 42 | `Array Get 0` | `Array` ← #59 `LevelNameList`<br>`Dimension 1` ← #50 `Result` | `Output` → #21 `LevelName` |
| 43 | `Array Get 1` | `Array` ← #22 `FoundWidgets`<br>`Dimension 1` = `0` | `Output` → #24 `self` |
| 45 | `Branch 0` | `execute` ← #47 `then`<br>`Condition` ← #55 `ReturnValue` | `then` → #25 `execute`<br>`else` → #22 `execute` |
| 46 | `Branch 2` | `execute` ← #25 `then`<br>`Condition` ← #25 `result` | `then` → #51 `execute`<br>`else` → #26 `execute` |
| 47 | `ConvertGameInstance` | `execute` ← #53 `true` | `then` → #45 `execute`<br>`AsBP Game Instance` → #57 `self`, #58 `self` |
| 48 | `ConvertGameInstance` | `execute` ← #23 `then` | `then` → #64 `execute`<br>`AsBP Game Instance` → #64 `self` |
| 49 | `MakeCameraDark` | `execute` ← #27 `then`<br>`Duration` ← #20 `ReturnValue` | `then` → #19 `execute` |
| 50 | `IncrementInt` | ` ` ← #26 `then`<br>`Value` ← #26 `level name` | `  ` → #27 `execute`<br>`Result` → #42 `Dimension 1` |
| 51 | `ConvertGameInstance` | `execute` ← #46 `then` | `then` → #63 `execute`<br>`AsBP Game Instance` → #63 `self` |
| 52 | `ConvertGameInstance` | `execute` ← #21 `then` | `then` → #65 `execute`<br>`AsBP Game Instance` → #65 `self` |
| 53 | `isplayer` | `execute` ← #33 `then`<br>`A` ← #33 `OtherActor` | `true` → #47 `execute` |
| 55 | `EqualEqual_IntInt` | `A` ← #57 `AllCristalNumber`<br>`B` ← #58 `Collected_Cristal_Number` | `ReturnValue` → #45 `Condition` |
| 57 | `Get AllCristalNumber` | `self` ← #47 `AsBP Game Instance` | `AllCristalNumber` → #55 `A` |
| 58 | `Get Collected_Cristal_Number` | `self` ← #47 `AsBP Game Instance` | `Collected_Cristal_Number` → #55 `B` |
| 59 | `Get LevelNameList` | — | `LevelNameList` → #42 `Array` |
| 63 | `Set IsWin` | `execute` ← #51 `then`<br>`IsWin` = `true`<br>`self` ← #51 `AsBP Game Instance` | Unconnected |
| 64 | `Set AllCristalNumber` | `execute` ← #48 `then`<br>`AllCristalNumber` ← #15 `ReturnValue`<br>`self` ← #48 `AsBP Game Instance` | Unconnected |
| 65 | `Set Collected_Cristal_Number` | `execute` ← #52 `then`<br>`Collected_Cristal_Number` = `0`<br>`self` ← #52 `AsBP Game Instance` | Unconnected |

## GetCurrentLevelIndex

```mermaid
flowchart TD
  n16["16: Array_Find"]
  n28["28: GetCurrentLevelName"]
  n29["29: Conv_StringToName"]
  n36["36: GetCurrentLevelIndex"]
  n40["40: GetCurrentLevelIndex"]
  n60["60: Get LevelNameList"]
  n16 -.->|"ReturnValue → level name"| n40
  n28 -->|"then → execute"| n40
  n28 -.->|"ReturnValue → InString"| n29
  n29 -.->|"ReturnValue → ItemToFind"| n16
  n36 -->|"then → execute"| n28
  n60 -.->|"LevelNameList → TargetArray"| n16
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 16 | `Array_Find` | `TargetArray` ← #60 `LevelNameList`<br>`ItemToFind` ← #29 `ReturnValue` | `ReturnValue` → #40 `level name` |
| 28 | `GetCurrentLevelName` | `execute` ← #36 `then`<br>`bRemovePrefixString` = `true` | `then` → #40 `execute`<br>`ReturnValue` → #29 `InString` |
| 29 | `Conv_StringToName` | `InString` ← #28 `ReturnValue` | `ReturnValue` → #16 `ItemToFind` |
| 36 | `GetCurrentLevelIndex` | — | `then` → #28 `execute` |
| 40 | `GetCurrentLevelIndex` | `execute` ← #28 `then`<br>`level name` ← #16 `ReturnValue` | Unconnected |
| 60 | `Get LevelNameList` | — | `LevelNameList` → #16 `TargetArray` |

## IsLastLevel

```mermaid
flowchart TD
  n17["17: Array_Find"]
  n18["18: Array_LastIndex"]
  n30["30: GetCurrentLevelName"]
  n31["31: Conv_StringToName"]
  n37["37: IsLastLevel"]
  n41["41: IsLastLevel"]
  n56["56: EqualEqual_IntInt"]
  n61["61: Get LevelNameList"]
  n17 -.->|"ReturnValue → B"| n56
  n18 -.->|"ReturnValue → A"| n56
  n30 -->|"then → execute"| n41
  n30 -.->|"ReturnValue → InString"| n31
  n31 -.->|"ReturnValue → ItemToFind"| n17
  n37 -->|"then → execute"| n30
  n56 -.->|"ReturnValue → result"| n41
  n61 -.->|"LevelNameList → TargetArray"| n17
  n61 -.->|"LevelNameList → TargetArray"| n18
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 17 | `Array_Find` | `TargetArray` ← #61 `LevelNameList`<br>`ItemToFind` ← #31 `ReturnValue` | `ReturnValue` → #56 `B` |
| 18 | `Array_LastIndex` | `TargetArray` ← #61 `LevelNameList` | `ReturnValue` → #56 `A` |
| 30 | `GetCurrentLevelName` | `execute` ← #37 `then`<br>`bRemovePrefixString` = `true` | `then` → #41 `execute`<br>`ReturnValue` → #31 `InString` |
| 31 | `Conv_StringToName` | `InString` ← #30 `ReturnValue` | `ReturnValue` → #17 `ItemToFind` |
| 37 | `IsLastLevel` | — | `then` → #30 `execute` |
| 41 | `IsLastLevel` | `execute` ← #30 `then`<br>`result` ← #56 `ReturnValue` | Unconnected |
| 56 | `EqualEqual_IntInt` | `A` ← #18 `ReturnValue`<br>`B` ← #17 `ReturnValue` | `ReturnValue` → #41 `result` |
| 61 | `Get LevelNameList` | — | `LevelNameList` → #17 `TargetArray`, #18 `TargetArray` |

## SaveLevelTime

```mermaid
flowchart TD
  n32["32: GetAllWidgetsOfClass"]
  n38["38: SaveLevelTime"]
  n44["44: Array Get 0"]
  n54["54: ConvertGameInstance"]
  n62["62: Get LevelTime"]
  n66["66: Set GameTime"]
  n32 -->|"then → execute"| n54
  n32 -.->|"FoundWidgets → Array"| n44
  n38 -->|"then → execute"| n32
  n44 -.->|"Output → self"| n62
  n54 -->|"then → execute"| n66
  n54 -.->|"AsBP Game Instance → self"| n66
  n62 -.->|"LevelTime → GameTime"| n66
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 32 | `GetAllWidgetsOfClass` | `execute` ← #38 `then`<br>`WidgetClass` = `BP_UI_C`<br>`TopLevelOnly` = `true` | `then` → #54 `execute`<br>`FoundWidgets` → #44 `Array` |
| 38 | `SaveLevelTime` | — | `then` → #32 `execute` |
| 44 | `Array Get 0` | `Array` ← #32 `FoundWidgets`<br>`Dimension 1` = `0` | `Output` → #62 `self` |
| 54 | `ConvertGameInstance` | `execute` ← #32 `then` | `then` → #66 `execute`<br>`AsBP Game Instance` → #66 `self` |
| 62 | `Get LevelTime` | `self` ← #44 `Output` | `LevelTime` → #66 `GameTime` |
| 66 | `Set GameTime` | `execute` ← #54 `then`<br>`GameTime` ← #62 `LevelTime`<br>`self` ← #54 `AsBP Game Instance` | Unconnected |

## UserConstructionScript

No connected node flow in this graph.

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 39 | `UserConstructionScript` | — | Unconnected |

## Reading this export

A connected input gets its value from the linked node; its unused editor default is not shown in the table. Class defaults can be overridden by placed actors in a map. A disconnected output does not execute another node. This is a static graph inspection, not a runtime test.
