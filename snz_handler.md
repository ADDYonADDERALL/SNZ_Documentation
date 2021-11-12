# SNZ Handler

## How to install

#### Installation
###### ESX
Whole script is plug&play all you need to do is edit your server.cfg
```
ensure SNZ_Handler
```
###### QBCORE
For QB you need to change fxmanifest.lua:

From:
```
Line 20:
shared_scripts {
    --'@qb-core/import.lua',
    'shared/config.lua',
}
```

To:
```
Line 20:
shared_scripts {
    '@qb-core/import.lua',
    'shared/config.lua',
}
```

## Shared
### Config
#### Framework
Used to set the framework that your RP server is using available options are:
* ESX
* QBCore

##### Example
```
Config.Framework = 'ESX'
```
***
#### PedDataRefreshInterval
Determines how often to refresh Ped Data (in ms); this includes:
* Coords
* Id
* Vehicle State
* etc.
##### Example
```
PedDataRefreshInterval = 100
```
This will refresh Ped Data every 100ms
***
#### ReycastDistance
How far to shoot a reycast (in units).
##### Example
```
Config.ReycastDistance = 40
```
***
#### MinimumCrashForce
Minimum force to be considered a crash.
##### Example
```
Config.MinimumCrashForce = 900
```
***
#### MinimumCrashSpeed
Minimum speed to be considered a crash.
##### Example
```
Config.MinimumCrashSpeed = 25
```
***
#### Directions
Name of the directions.
##### Example
```
[0] = 'N',
```
***
#### Zones
Name of the zones.
##### Example
```
['VINE'] = 'Vinewood',
```
***
## Client
***
### Exports
#### GetSharedObject
Pulls the SNZ Object which includes selected Framework Object.  
Returns Object
##### Example
```
SNZ = nil
Citizen.CreateThread(function()
	while not SNZ do
		SNZ = exports[Config.HandlerName]:GetSharedObject()
		Citizen.Wait(10)
	end
end)
```
***
#### GetTalking
Pulls value; true if player is talking; false if not.  
Returns Boolean
##### Example
```
local Talking = exports['SNZ_Handler']:GetTalking()
if Talking then
	-- do something
end
```
***
#### GetUnderWater
Pulls value; true if player is under water; false if not.  
Returns Boolean
##### Example
```
local UnderWater = exports['SNZ_Handler']:GetUnderWater()
if UnderWater then
	-- do something
end
```
***
#### GetSprinting
Pulls value; true if player is sprinting; false if not.  
Returns Boolean
##### Example
```
local Sprinting = exports['SNZ_Handler']:GetSprinting()
if Sprinting then
	-- do something
end
```
***
#### GetInVehilce
Pulls value; true if player is in vehicle; false if not.  
Returns Boolean
##### Example
```
local InVehicle = exports['SNZ_Handler']:GetInVehilce()
if InVehicle then
	-- do something
end
```
***
#### GetPedData
Pulls value; table contanings all Ped Data collected by the Handler.  
Returns Table
##### Example
```
local PedData = exports['SNZ_Handler']:GetPedData()
print(PedData.Id)
print(PedData.Ped)
print(PedData.Coords)
print(PedData.Heading)
print(PedData.Alive)
print(PedData.Health)
print(PedData.Armour)
print(PedData.Sprinting)
print(PedData.Stamina)
print(PedData.UnderWater)
print(PedData.UnderWaterTime)
print(PedData.Talking)
print(PedData.CameraRotation)
print(PedData.Reycast)
print(PedData.InVehicle)
print(PedData.IsEnteringVehicle)
print(PedData.CurrentVehicle)
print(PedData.VehicleClass)
print(PedData.Engine)
print(PedData.Speed)
print(PedData.RPMScale)
print(PedData.RPM)
print(PedData.Velocity)
print(PedData.SpeedVector)
print(PedData.Acceleration)
print(PedData.PrevSpeed)
print(PedData.PrevVelocity)
print(PedData.Zone)
print(PedData.StreetLabel.Direction)
print(PedData.StreetLabel.Zone)
print(PedData.StreetLabel.Street)
```
***
#### GetConfig
Pulls config.
Returns Table
##### Example
```
local Config = exports['SNZ_Handler']:GetConfig()
print(Config.Framework)
```
***
### Events
#### SNZ_Handler:OnVehicleEnter
Triggers whenever a player enters a vehicle.  
Returns Vehicle
##### Example
```
AddEventHandler('SNZ_Handler:OnVehicleEnter', function(vehicle)
	-- do something
end)
```
***
#### SNZ_Handler:OnVehicleExit
Triggers whenever a player exits a vehicle.  
Returns Vehicle
##### Example
```
AddEventHandler('SNZ_Handler:OnVehicleExit', function(vehicle)
	-- do something
end)
```
***
#### SNZ_Handler:OnVehicleHit
Triggers whenever a player crashes their vehicle with a suitable force.  
Returns:   
1 - Ped  
2 - Velocity (Table)  
3 - Coords (Table)  
4 - Vehicle  

##### Example
```
AddEventHandler('SNZ_Handler:OnVehicleHit', function(ped, velocity, coords, vehicle)
	-- do something
end)
```
***
#### SNZ_Handler:IsTalking
Triggers whenever player starts talking.
##### Example
```
AddEventHandler('SNZ_Handler:IsTalking', function()
	-- do something
end)
```
***
#### SNZ_Handler:IsNotTalking
Triggers whenever player stops talking.
##### Example
```
AddEventHandler('SNZ_Handler:IsNotTalking', function()
	-- do something
end)
```
***
#### SNZ_Handler:IsUnderWater
Triggers whenever player goes underwater.
##### Example
```
AddEventHandler('SNZ_Handler:IsUnderWater', function()
	-- do something
end)
```
***
#### SNZ_Handler:IsNotUnderWater
Triggers whenever player comes back from underwater.
##### Example
```
AddEventHandler('SNZ_Handler:IsNotUnderWater', function()
	-- do something
end)
```
***
#### SNZ_Handler:IsSprinting
Triggers whenever player starts sprinting.
##### Example
```
AddEventHandler('SNZ_Handler:IsSprinting', function()
	-- do something
end)
```
***
#### SNZ_Handler:IsNotSprinting
Triggers whenever player stops sprinting.
##### Example
```
AddEventHandler('SNZ_Handler:IsNotSprinting', function()
	-- do something
end)
```
***
## Server
***
### Exports
#### GetSharedObject
Pulls the SNZ Object which includes selected Framework Object.  
Returns Object
##### Example
```
SNZ = nil
SNZ = exports[Config.HandlerName]:GetSharedObject()
```
***
#### GetConfig
Pulls config.
Returns Table
##### Example
```
local Config = exports['SNZ_Handler']:GetConfig()
print(Config.Framework)
```
***
