# BA_Player

Player movement, camera setup, damage and win response.

[Back to walkthrough](README.md) · [Open Blueprint asset](../../Content/blueprints/props/BA_Player.uasset)

The node IDs below are export indices in this saved asset. Diagrams are reconstructed from serialized Blueprint pin links, not Unreal Editor screenshots. Solid arrows show execution; dotted arrows show data. Empty event nodes remain visible in the tables.

## EventGraph

```mermaid
flowchart TD
  n21["21: QuitGame"]
  n22["22: AddToViewport"]
  n23["23: Delay"]
  n24["24: SetSimulatePhysics"]
  n25["25: AddTorqueInRadians"]
  n26["26: AddTorqueInRadians"]
  n28["28: Create Widget 0"]
  n29["29: ReceiveBeginPlay"]
  n30["30: ReceiveAnyDamage"]
  n31["31: ReceiveTick"]
  n34["34: Branch 0"]
  n35["35: Branch 1"]
  n36["36: Branch 2"]
  n37["37: InpAxisEvt_Move_Horrizontal_K2Node_InputAxisEvent_0"]
  n38["38: InpAxisEvt_Move_Vertical_K2Node_InputAxisEvent_1"]
  n39["39: {'KeyName': 'Escape'}"]
  n40["40: MakeCameraDark"]
  n41["41: MakeCameraDark"]
  n42["42: RestartLevel"]
  n43["43: ConvertGameInstance"]
  n44["44: ConvertGameInstance"]
  n45["45: ConvertGameInstance"]
  n46["46: DoOnce"]
  n47["47: Multiply_DoubleDouble"]
  n48["48: Multiply_DoubleDouble"]
  n49["49: Get Player"]
  n50["50: Get Player"]
  n51["51: Get amplification"]
  n52["52: Get IsWin"]
  n53["53: Get IsWin"]
  n54["54: Get IsWin"]
  n55["55: Get Player"]
  n23 -->|"then → Execute"| n42
  n28 -->|"then → execute"| n22
  n28 -.->|"ReturnValue → self"| n22
  n29 -->|"then → execute"| n41
  n30 -->|"then → execute"| n40
  n31 -->|"then → execute"| n45
  n34 -->|"else → execute"| n25
  n35 -->|"else → execute"| n26
  n36 -->|"then → execute"| n46
  n37 -->|"then → execute"| n43
  n37 -.->|"AxisValue → A"| n48
  n38 -->|"then → execute"| n44
  n38 -.->|"AxisValue → A"| n47
  n39 -->|"Pressed → execute"| n21
  n40 -->|"then → execute"| n23
  n41 -->|"then → execute"| n28
  n43 -->|"then → execute"| n34
  n43 -.->|"AsBP Game Instance → self"| n52
  n44 -->|"then → execute"| n35
  n44 -.->|"AsBP Game Instance → self"| n53
  n45 -->|"then → execute"| n36
  n45 -.->|"AsBP Game Instance → self"| n54
  n46 -->|"Completed → execute"| n24
  n47 -.->|"ReturnValue → Torque_Y"| n26
  n48 -.->|"ReturnValue → Torque_X"| n25
  n49 -.->|"Player → self"| n25
  n50 -.->|"Player → self"| n26
  n51 -.->|"amplification → B"| n48
  n51 -.->|"amplification → B"| n47
  n52 -.->|"IsWin → Condition"| n34
  n53 -.->|"IsWin → Condition"| n35
  n54 -.->|"IsWin → Condition"| n36
  n55 -.->|"Player → self"| n24
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 21 | `QuitGame` | `execute` ← #39 `Pressed`<br>`QuitPreference` = `Quit`<br>`bIgnorePlatformRestrictions` = `false` | Unconnected |
| 22 | `AddToViewport` | `execute` ← #28 `then`<br>`self` ← #28 `ReturnValue`<br>`ZOrder` = `0` | Unconnected |
| 23 | `Delay` | `execute` ← #40 `then`<br>`Duration` = `1.000000` | `then` → #42 `Execute` |
| 24 | `SetSimulatePhysics` | `execute` ← #46 `Completed`<br>`self` ← #55 `Player`<br>`bSimulate` = `false` | Unconnected |
| 25 | `AddTorqueInRadians` | `execute` ← #34 `else`<br>`self` ← #49 `Player`<br>`Torque` = `0, 0, 0`<br>`Torque_X` ← #48 `ReturnValue`<br>`Torque_Y` = `0.0`<br>`Torque_Z` = `0.0`<br>`BoneName` = `None`<br>`bAccelChange` = `true` | Unconnected |
| 26 | `AddTorqueInRadians` | `execute` ← #35 `else`<br>`self` ← #50 `Player`<br>`Torque` = `0, 0, 0`<br>`Torque_X` = `0.0`<br>`Torque_Y` ← #47 `ReturnValue`<br>`Torque_Z` = `0.0`<br>`BoneName` = `None`<br>`bAccelChange` = `true` | Unconnected |
| 28 | `Create Widget 0` | `execute` ← #41 `then`<br>`Class` = `BP_UI_C` | `then` → #22 `execute`<br>`ReturnValue` → #22 `self` |
| 29 | `ReceiveBeginPlay` | — | `then` → #41 `execute` |
| 30 | `ReceiveAnyDamage` | — | `then` → #40 `execute` |
| 31 | `ReceiveTick` | — | `then` → #45 `execute` |
| 34 | `Branch 0` | `execute` ← #43 `then`<br>`Condition` ← #52 `IsWin` | `else` → #25 `execute` |
| 35 | `Branch 1` | `execute` ← #44 `then`<br>`Condition` ← #53 `IsWin` | `else` → #26 `execute` |
| 36 | `Branch 2` | `execute` ← #45 `then`<br>`Condition` ← #54 `IsWin` | `then` → #46 `execute` |
| 37 | `InpAxisEvt_Move_Horrizontal_K2Node_InputAxisEvent_0` | — | `then` → #43 `execute`<br>`AxisValue` → #48 `A` |
| 38 | `InpAxisEvt_Move_Vertical_K2Node_InputAxisEvent_1` | — | `then` → #44 `execute`<br>`AxisValue` → #47 `A` |
| 39 | `{'KeyName': 'Escape'}` | — | `Pressed` → #21 `execute` |
| 40 | `MakeCameraDark` | `execute` ← #30 `then`<br>`ToAlpha` = `1.000000`<br>`Duration` = `1.000000`<br>`bHoldWhenFinished` = `true` | `then` → #23 `execute` |
| 41 | `MakeCameraDark` | `execute` ← #29 `then`<br>`FromAlpha` = `1.000000`<br>`Duration` = `1.000000`<br>`bHoldWhenFinished` = `true` | `then` → #28 `execute` |
| 42 | `RestartLevel` | `Execute` ← #23 `then` | Unconnected |
| 43 | `ConvertGameInstance` | `execute` ← #37 `then` | `then` → #34 `execute`<br>`AsBP Game Instance` → #52 `self` |
| 44 | `ConvertGameInstance` | `execute` ← #38 `then` | `then` → #35 `execute`<br>`AsBP Game Instance` → #53 `self` |
| 45 | `ConvertGameInstance` | `execute` ← #31 `then` | `then` → #36 `execute`<br>`AsBP Game Instance` → #54 `self` |
| 46 | `DoOnce` | `execute` ← #36 `then` | `Completed` → #24 `execute` |
| 47 | `Multiply_DoubleDouble` | `A` ← #38 `AxisValue`<br>`B` ← #51 `amplification` | `ReturnValue` → #26 `Torque_Y` |
| 48 | `Multiply_DoubleDouble` | `A` ← #37 `AxisValue`<br>`B` ← #51 `amplification` | `ReturnValue` → #25 `Torque_X` |
| 49 | `Get Player` | — | `Player` → #25 `self` |
| 50 | `Get Player` | — | `Player` → #26 `self` |
| 51 | `Get amplification` | — | `amplification` → #48 `B`, #47 `B` |
| 52 | `Get IsWin` | `self` ← #43 `AsBP Game Instance` | `IsWin` → #34 `Condition` |
| 53 | `Get IsWin` | `self` ← #44 `AsBP Game Instance` | `IsWin` → #35 `Condition` |
| 54 | `Get IsWin` | `self` ← #45 `AsBP Game Instance` | `IsWin` → #36 `Condition` |
| 55 | `Get Player` | — | `Player` → #24 `self` |

## pointView

```mermaid
flowchart TD
  n27["27: K2_SetWorldRotation"]
  n32["32: pointView"]
  n56["56: Get SpringArm"]
  n57["57: Set TargetArmLength"]
  n32 -->|"then → execute"| n57
  n32 -.->|"Target Arm Length → TargetArmLength"| n57
  n32 -.->|"NewRotation → NewRotation"| n27
  n56 -.->|"SpringArm → self"| n57
  n56 -.->|"SpringArm → self"| n27
  n57 -->|"then → execute"| n27
```

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 27 | `K2_SetWorldRotation` | `execute` ← #57 `then`<br>`self` ← #56 `SpringArm`<br>`NewRotation` ← #32 `NewRotation`<br>`bSweep` = `false`<br>`bTeleport` = `false` | Unconnected |
| 32 | `pointView` | — | `then` → #57 `execute`<br>`Target Arm Length` → #57 `TargetArmLength`<br>`NewRotation` → #27 `NewRotation` |
| 56 | `Get SpringArm` | — | `SpringArm` → #57 `self`, #27 `self` |
| 57 | `Set TargetArmLength` | `execute` ← #32 `then`<br>`TargetArmLength` ← #32 `Target Arm Length`<br>`self` ← #56 `SpringArm` | `then` → #27 `execute` |

## UserConstructionScript

No connected node flow in this graph.

| ID | Node | Connected inputs and stored defaults | Outputs |
| --- | --- | --- | --- |
| 33 | `UserConstructionScript` | — | Unconnected |

## Reading this export

A connected input gets its value from the linked node; its unused editor default is not shown in the table. Class defaults can be overridden by placed actors in a map. A disconnected output does not execute another node. This is a static graph inspection, not a runtime test.
