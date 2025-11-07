local ATTACK_CONFIG = {
	["Normal Punch"] = {
		newName = "Black Flash",
		animationId = 15955393872,
		speed = 1,
		timePos = 0
	},
	["Consecutive Punches"] = {
		newName = "Barragem",
		animationId = 13560306510,
		speed = 3,
		timePos = 0
	},
	["Shove"] = {
		newName = "Divergent Punch",
		animationId = 16944265635,
		speed = 1,
		timePos = 0
	},
	["Uppercut"] = {
		newName = "Black Flash Spinning Kick",
		animationId = 18179181663,
		speed = 1,
		timePos = 0
	}
}

local ANIMATION_REPLACEMENTS = {
	[10468665991] = "Normal Punch",
	[10466974800] = "Consecutive Punches",
	[10471336737] = "Shove",
	[12510170988] = "Uppercut"
}

local fontConfig = {
	Enabled = false,
	Font = Enum.Font.Sarpanch,
	Weight = Enum.FontWeight.Thin,
	Style = Enum.FontStyle.Normal
}

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

local hotbarCache = {}
local isAlive = true

local t1 = {}

for i, attackName in ipairs({"Normal Punch", "Consecutive Punches", "Shove", "Uppercut"}) do
	local config = ATTACK_CONFIG[attackName]
	if config then
		t1[tostring(i)] = {original = attackName, new = config.newName}
	end
end

local function getHotbarPath()
	if hotbarCache.hotbar and hotbarCache.hotbar.Parent then
		return hotbarCache.hotbar, hotbarCache.backpack, hotbarCache.hotbarFrame
	end

	local hotbar = playerGui:FindFirstChild("Hotbar")
	if not hotbar then return nil, nil, nil end

	local backpack = hotbar:FindFirstChild("Backpack")
	if not backpack then return nil, nil, nil end

	local hotbarFrame = backpack:FindFirstChild("Hotbar")
	if not hotbarFrame then return nil, nil, nil end

	hotbarCache.hotbar = hotbar
	hotbarCache.backpack = backpack
	hotbarCache.hotbarFrame = hotbarFrame

	return hotbar, backpack, hotbarFrame
end

local function applyFont(label)
	if fontConfig.Enabled and label:IsA("TextLabel") then
		pcall(function()
			label.FontFace = Font.new(label.FontFace.Family, fontConfig.Weight, fontConfig.Style)
			label.Font = fontConfig.Font
		end)
	end
end

local function updateHotbarNames(toolTable)
	local _, _, hotbarFrame = getHotbarPath()
	if not hotbarFrame then return end

	for buttonName, toolData in pairs(toolTable) do
		local button = hotbarFrame:FindFirstChild(buttonName)
		if button then
			local baseButton = button:FindFirstChild("Base")
			if baseButton then
				local toolName = baseButton:FindFirstChild("ToolName")
				if toolName and toolName.Text == toolData.original then
					toolName.Text = toolData.new
					applyFont(toolName)
				end
			end
		end
	end
end

local function updateHunterText()
	local screenGui = playerGui:FindFirstChild("ScreenGui")
	if screenGui then
		local magicHealth = screenGui:FindFirstChild("MagicHealth")
		if magicHealth then
			local textLabel = magicHealth:FindFirstChild("TextLabel")
			if textLabel then
				textLabel.Text = "Hunter"
			end
		end
	end
end

local lastUpdate = 0
local UPDATE_INTERVAL = 0.1

RunService.Heartbeat:Connect(function()
	if not isAlive then return end

	local currentTime = tick()
	if currentTime - lastUpdate >= UPDATE_INTERVAL then
		lastUpdate = currentTime
		updateHotbarNames(t1)
		updateHunterText()
	end
end)

local function bindReplacement(animationId, replacementId, speed, timePos)
	humanoid.AnimationPlayed:Connect(function(animationTrack)
		if animationTrack.Animation.AnimationId == "rbxassetid://" .. animationId then
			task.spawn(function()
				task.wait()

				for _, track in pairs(humanoid:GetPlayingAnimationTracks()) do
					if track.Animation.AnimationId == "rbxassetid://" .. animationId then
						track:Stop()
					end
				end

				local AnimAnim = Instance.new("Animation")
				AnimAnim.AnimationId = "rbxassetid://" .. replacementId
				local Anim = humanoid:LoadAnimation(AnimAnim)
				Anim:Play()
				Anim:AdjustSpeed(0)
				Anim.TimePosition = timePos or 0
				Anim:AdjustSpeed(speed or 1)
			end)
		end
	end)
end

for originalAnim, attackData in pairs(ANIMATION_REPLACEMENTS) do
	if type(attackData) == "string" then
		local config = ATTACK_CONFIG[attackData]
		if config and config.animationId then
			bindReplacement(
				originalAnim,
				config.animationId,
				config.speed,
				config.timePos
			)
		end
	end
end

player.CharacterAdded:Connect(function(newCharacter)
	character = newCharacter
	humanoid = character:WaitForChild("Humanoid")
	isAlive = true
	hotbarCache = {}

	for originalAnim, attackData in pairs(ANIMATION_REPLACEMENTS) do
		if type(attackData) == "string" then
			local config = ATTACK_CONFIG[attackData]
			if config and config.animationId then
				bindReplacement(
					originalAnim,
					config.animationId,
					config.speed,
					config.timePos
				)
			end
		end
	end
end)

humanoid.Died:Connect(function()
	isAlive = false
end)
