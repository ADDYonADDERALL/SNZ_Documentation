# SNZ UI

## Shared
### Config
***
#### Framework
Used to set the framework that your RP server is using available options are:
* ESX
* QBCore

##### Example
```
Config.Framework = 'ESX'
```
***
#### HandlerName
Used to set the resource name of the SNZ Handler.

##### Example
```
Config.HandlerName = 'SNZ_Handler'
```
***
#### SettingsButton
Sets default keybind for setting.

##### Example
```
Config.SettingsButton = 'F9'
```
***
#### SeatBeltButton
Sets default keybind to toggle seatbelt.

##### Example
```
Config.SeatBeltButton = 'B'
```
***
#### Voice
Set your current voice resource available options are:
* esx
* tokovoip
* mumblevoip
* pmavoice
##### Example
```
Config.Voice = 'esx'
```
***
#### Units
Changes the speedometer from kmh to mph available options are:
* kmh
* mph
##### Example
```
Config.Units = 'kmh'
```
***
#### UseFuel
If you are using a fuel resource available options are:
* false
* true
##### Example
```
Config.UseFuel = true
```
***
#### FuelScript
Fuel resource you are using available options are:
* legacyfuel
##### Example
```
Config.FuelScript = 'legacyfuel'
```
***
## HTML

## Client
***
### Exports
#### ToggleUI
Toggles UI on and off.  
Accepts Boolean
##### Example
```
if something then
	exports['SNZ_UI']:ToggleUI(true)
end
```
***
#### GetUIState
Get current UI state (if toggled on or off).  
Returns Boolean
##### Example
```
local UIstate = exports['SNZ_UI']:GetUIState()
if UIstate then
	-- do something
end
```
***
#### AddNotification
Toggles a notification accepts:    
1 - header (string)   
2 - text (string)  
3 - duration (int in ms)  
4 - icon (string)  
##### Example
```
if something then
	exports['SNZ_UI']:AddNotification('something', 'something else', 5000, 'fas fa-inbox')
end
```
***
#### SetVoice
Toggles voice distance accepts:    
1 - Distance (int)  
##### Example
```
if something then
	exports['SNZ_UI']:SetVoice(1)
end
```
***
### Events
***
#### SNZ_UI:ToggleUI
Toggles UI on and off.  
Accepts Boolean
##### Example
```
if something then
	TriggerEvent('SNZ_UI:ToggleUI', true)
end
```
***
#### SNZ_UI:AddNotification
Toggles a notification accepts:    
1 - header (string)   
2 - text (string)  
3 - duration (int in ms)  
4 - icon (string)  
##### Example
```
if something then
	TriggerEvent('SNZ_UI:AddNotification', 'something', 'something else', 5000, 'fas fa-inbox')
end
```
***
#### SNZ_UI:SetVoice
Toggles voice distance accepts:    
1 - Distance (int)  
##### Example
```
if something then
	TriggerEvent('SNZ_UI:SetVoice', 1)
end
```
***