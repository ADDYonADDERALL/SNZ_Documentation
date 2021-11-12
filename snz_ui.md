# SNZ UI

## How to install

#### Installation
###### ESX
Whole script is plug&play all you need to do is edit your server.cfg
```
ensure SNZ_Handler
ensure SNZ_UI
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

#### Boot Menu Pictures

In file: 
```
SNZ_UI/client/html/index.html
```
Code:
```
<div class="background-image">
	<img src="https://game-insider.com/wp-content/uploads/gta5-grand-theft-auto-v-grand-theft-auto-hollywood-wallpaper.jpg" />
</div>
```

You need to only change source of <img>. 

## Client
***
### Voice Setup
If you want your HUD to display correct micrphone distance values you will need to edit your voice resource.
You will need to add this line in your script based on the resource that you are using.
#### ESX
```
TriggerEvent('SNZ_UI:SetVoice', voice.current)
```
In file:
```
esx_voice/client/main.lua (45)
```
Like shown in the example below
```
Citizen.CreateThread(function()
	while true do
		Citizen.Wait(1)

		if isLoaded == true then
			if IsControlJustPressed(1, Keys['H']) and IsControlPressed(1, Keys['LEFTSHIFT']) then
				voice.current = (voice.current + 1) % 3
				if voice.current == 0 then
					NetworkSetTalkerProximity(voice.default)
					voice.level = _U('normal')
				elseif voice.current == 1 then
					NetworkSetTalkerProximity(voice.shout)
					voice.level = _U('shout')
				elseif voice.current == 2 then
					NetworkSetTalkerProximity(voice.whisper)
					voice.level = _U('whisper')
				end
				TriggerEvent('SNZ_UI:SetVoice', voice.current)
			end

			if voice.current == 0 then
				voice.level = _U('normal')
			elseif voice.current == 1 then
				voice.level = _U('shout')
			elseif voice.current == 2 then
				voice.level = _U('whisper')
			end

			if NetworkIsPlayerTalking(PlayerId()) then
				drawLevel(41, 128, 185, 255)
			elseif not NetworkIsPlayerTalking(PlayerId()) then
				drawLevel(185, 185, 185, 255)
			end
		end
	end
end)
```
***
#### Tokovoip
```
TriggerEvent('SNZ_UI:SetVoice', self.mode)
```
In file:
```
tokovoip_script/src/c_TokoVoip.lua (101)
```
Like shown in the example below
```
function TokoVoip.initialize(self)
	self:updateConfig();
	self:updatePlugin("initializeSocket", self.wsServer);
	Citizen.CreateThread(function()
		while (true) do
			Citizen.Wait(5);

			if ((self.keySwitchChannelsSecondary and IsControlPressed(0, self.keySwitchChannelsSecondary)) or not self.keySwitchChannelsSecondary) then -- Switch radio channels
				if (IsControlJustPressed(0, self.keySwitchChannels) and tablelength(self.myChannels) > 0) then
					local myChannels = {};
					local currentChannel = 0;
					local currentChannelID = 0;

					for channel, _ in pairs(self.myChannels) do
						if (channel == self.plugin_data.radioChannel) then
							currentChannel = #myChannels + 1;
							currentChannelID = channel;
						end
						myChannels[#myChannels + 1] = {channelID = channel};
					end
					if (currentChannel == #myChannels) then
						currentChannelID = myChannels[1].channelID;
					else
						currentChannelID = myChannels[currentChannel + 1].channelID;
					end
					self.plugin_data.radioChannel = currentChannelID;
					setPlayerData(self.serverId, "radio:channel", currentChannelID, true);
					self:updateTokoVoipInfo();
				end
			elseif (IsControlJustPressed(0, self.keyProximity)) then -- Switch proximity modes (normal / whisper / shout)
				if (not self.mode) then
					self.mode = 1;
				end
				self.mode = self.mode + 1;
				if (self.mode > 3) then
					self.mode = 1;
				end
				setPlayerData(self.serverId, "voip:mode", self.mode, true);
				self:updateTokoVoipInfo();
				TriggerEvent('SNZ_UI:SetVoice', self.mode)
			end


			if (IsControlPressed(0, self.radioKey) and self.plugin_data.radioChannel ~= -1 and self.config.radioEnabled) then -- Talk on radio
				self.plugin_data.radioTalking = true;
				self.plugin_data.localRadioClicks = true;
				if (self.plugin_data.radioChannel > self.config.radioClickMaxChannel) then
					self.plugin_data.localRadioClicks = false;
				end
				if (not getPlayerData(self.serverId, "radio:talking")) then
					setPlayerData(self.serverId, "radio:talking", true, true);
				end
				self:updateTokoVoipInfo();
				if (lastTalkState == false and self.myChannels[self.plugin_data.radioChannel] and self.config.radioAnim) then
					if (not string.match(self.myChannels[self.plugin_data.radioChannel].name, "Call") and not IsPedSittingInAnyVehicle(PlayerPedId())) then
						RequestAnimDict("random@arrests");
						while not HasAnimDictLoaded("random@arrests") do
							Wait(5);
						end
						TaskPlayAnim(PlayerPedId(),"random@arrests","generic_radio_chatter", 8.0, 0.0, -1, 49, 0, 0, 0, 0);
					end
					lastTalkState = true
				end
			else
				self.plugin_data.radioTalking = false;
				if (getPlayerData(self.serverId, "radio:talking")) then
					setPlayerData(self.serverId, "radio:talking", false, true);
				end
				self:updateTokoVoipInfo();

				if lastTalkState == true and self.config.radioAnim then
					lastTalkState = false
					StopAnimTask(PlayerPedId(), "random@arrests","generic_radio_chatter", -4.0);
				end
			end
		end
	end);
end
```
***
#### MumbleVoip
```
TriggerEvent('SNZ_UI:SetVoice', voiceMode)
```
In file:
```
mumble-voip/client.lua (756)
```
Like shown in the example below
```
Citizen.CreateThread(function()
	while true do
		Citizen.Wait(0)

		if initialised then
			local playerData = voiceData[playerServerId]

			if not playerData then
				playerData = {
					mode = 2,
					radio = 0,
					radioActive = false,
					call = 0,
					callSpeaker = false,
					speakerTargets = {},
				}
			end

			if playerData.radioActive then -- Force PTT enabled
				SetControlNormal(0, 249, 1.0)
				SetControlNormal(1, 249, 1.0)
				SetControlNormal(2, 249, 1.0)
			end

			if IsControlJustPressed(0, mumbleConfig.controls.proximity.key) then
				local secondaryPressed = true

				if mumbleConfig.controls.speaker.key ~= mumbleConfig.controls.proximity.key or mumbleConfig.controls.speaker.secondary == nil then
					secondaryPressed = false
				else
					secondaryPressed = IsControlPressed(0, mumbleConfig.controls.speaker.secondary) and (playerData.call > 0)
				end

				if not secondaryPressed then
					local voiceMode = playerData.mode
				
					local newMode = voiceMode + 1
				
					if newMode > #mumbleConfig.voiceModes then
						voiceMode = 1
					else
						voiceMode = newMode
					end
					
					NetworkSetTalkerProximity(mumbleConfig.voiceModes[voiceMode][1] + 0.0)

					SetVoiceData("mode", voiceMode)
					playerData.mode = voiceMode
					TriggerEvent('SNZ_UI:SetVoice', voiceMode)
				end
			end

			if mumbleConfig.radioEnabled then
				if not mumbleConfig.controls.radio.pressed then
					if IsControlJustPressed(0, mumbleConfig.controls.radio.key) and IsInputDisabled(0) then
						if playerData.radio > 0 then
							SetVoiceData("radioActive", true)
							playerData.radioActive = true
							SetPlayerTargets(callTargets, speakerTargets, vehicleTargets, radioTargets) -- Send voice to everyone in the radio and call
							PlayMicClick(playerData.radio, true)
							if mumbleConfig.showRadioList then
								SendNUIMessage({ radioId = playerServerId, radioTalking = true }) -- Set client talking in radio list
							end
							mumbleConfig.controls.radio.pressed = true

							Citizen.CreateThread(function()
								while IsControlPressed(0, mumbleConfig.controls.radio.key) and IsInputDisabled(0) do
									Citizen.Wait(0)
								end

								SetVoiceData("radioActive", false)
								SetPlayerTargets(callTargets, speakerTargets, vehicleTargets) -- Stop sending voice to everyone in the radio
								PlayMicClick(playerData.radio, false)
								if mumbleConfig.showRadioList then
									SendNUIMessage({ radioId = playerServerId, radioTalking = false }) -- Set client talking in radio list
								end
								playerData.radioActive = false
								mumbleConfig.controls.radio.pressed = false
							end)
						end
					end
				end
			else
				if playerData.radioActive then
					SetVoiceData("radioActive", false)
					playerData.radioActive = false
				end
			end

			if mumbleConfig.callSpeakerEnabled then
				local secondaryPressed = false

				if mumbleConfig.controls.speaker.secondary ~= nil then
					secondaryPressed = IsControlPressed(0, mumbleConfig.controls.speaker.secondary)
				else
					secondaryPressed = true
				end

				if IsControlJustPressed(0, mumbleConfig.controls.speaker.key) and secondaryPressed then
					if playerData.call > 0 then
						SetVoiceData("callSpeaker", not playerData.callSpeaker)
						playerData.callSpeaker = not playerData.callSpeaker
					end
				end
			end
		end
	end
end)
```
***
#### PMA-Voice
```
TriggerEvent('SNZ_UI:SetVoice', voiceMode)
```
In file:
```
pma-voice/client/commands.lua (18)
```
Like shown in the example below
```
RegisterCommand('cycleproximity', function()
	if GetConvarInt('voice_enableProximity', 1) ~= 1 then return end
	if playerMuted then return end

	local voiceMode = mode
	local newMode = voiceMode + 1

	voiceMode = (newMode <= #Cfg.voiceModes and newMode) or 1
	local voiceModeData = Cfg.voiceModes[voiceMode]
	MumbleSetAudioInputDistance(voiceModeData[1] + 0.0)
	mode = voiceMode
	LocalPlayer.state:set('proximity', {
		index = voiceMode,
		distance =  voiceModeData[1],
		mode = voiceModeData[2],
	}, GetConvarInt('voice_syncData', 1) == 1)
	-- make sure we update the UI to the latest voice mode
	SendNUIMessage({
		voiceMode = voiceMode - 1
	})
	TriggerEvent('pma-voice:setTalkingMode', voiceMode)
	TriggerEvent('SNZ_UI:SetVoice', voiceMode)
end, false)
```
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
