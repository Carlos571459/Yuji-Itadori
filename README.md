local og1 = "Normal Punch"
local og2 = "Consecutive Punches"
local og3 = "Shove"
local og4 = "Uppercut"

local mn1 = "Black Flash"
local mn2 = "Divergent Dam Combo"
local mn3 = "Black Flash is expelled"
local mn4 = "Divergent Punch"

local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local character = player.Character or player.CharacterAdded:Wait()

local t1 = {
    ["1"] = {original = og1, new = mn1},
    ["2"] = {original = og2, new = mn2},
    ["3"] = {original = og3, new = mn3},
    ["4"] = {original = og4, new = mn4}
}

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

local function N1()
    while character.Humanoid.Health > 0 do
        local hotbar = playerGui:FindFirstChild("Hotbar")
        if hotbar then
            local backpack = hotbar:FindFirstChild("Backpack")
            if backpack then
                local hotbarFrame = backpack:FindFirstChild("Hotbar")
                if hotbarFrame then
                    for buttonName, toolData in pairs(t1) do
                        local baseButton = hotbarFrame:FindFirstChild(buttonName)
                        baseButton = baseButton and baseButton:FindFirstChild("Base")
                        if baseButton then
                            local toolName = baseButton:FindFirstChild("ToolName")
                            if toolName and toolName.Text == toolData.original then
                                toolName.Text = toolData.new
                            end
                        end
                    end
                end
            end
        end
        updateHunterText()
        wait(0.1)
    end
end

coroutine.wrap(N1)()

local humanoid = character:WaitForChild("Humanoid")

local animationId = 10468665991

local function onAnimationPlayed(animationTrack)
    if animationTrack.Animation.AnimationId == "rbxassetid://" .. animationId then
        local p = game.Players.LocalPlayer
        local Humanoid = p.Character:WaitForChild("Humanoid")
        
        for _, animTrack in pairs(Humanoid:GetPlayingAnimationTracks()) do
            animTrack:Stop()
        end
        
        local AnimAnim = Instance.new("Animation")
        AnimAnim.AnimationId = "rbxassetid://17186602996"
        local Anim = Humanoid:LoadAnimation(AnimAnim)
        
        local startTime = 0
        
        Anim:Play()
        Anim:AdjustSpeed(0.1)
        Anim.TimePosition = startTime
        Anim:AdjustSpeed(1)
    end
end

humanoid.AnimationPlayed:Connect(onAnimationPlayed)

local animationId2 = 10466974800

local function onAnimationPlayed2(animationTrack)
    if animationTrack.Animation.AnimationId == "rbxassetid://" .. animationId2 then
        local p = game.Players.LocalPlayer
        local Humanoid = p.Character:WaitForChild("Humanoid")
        
        for _, animTrack in pairs(Humanoid:GetPlayingAnimationTracks()) do
            animTrack:Stop()
        end
        
        local AnimAnim = Instance.new("Animation")
        AnimAnim.AnimationId = "rbxassetid://13560306510"
        local Anim = Humanoid:LoadAnimation(AnimAnim)
        
        local startTime = 0
        
        Anim:Play()
        Anim:AdjustSpeed(0.1)
        Anim.TimePosition = startTime
        Anim:AdjustSpeed(3)
    end
end

humanoid.AnimationPlayed:Connect(onAnimationPlayed2)

local animationId3 = 10471336737

local function onAnimationPlayed3(animationTrack)
    if animationTrack.Animation.AnimationId == "rbxassetid://" .. animationId3 then
        local p = game.Players.LocalPlayer
        local Humanoid = p.Character:WaitForChild("Humanoid")
        
        for _, animTrack in pairs(Humanoid:GetPlayingAnimationTracks()) do
            animTrack:Stop()
        end
        
        local AnimAnim = Instance.new("Animation")
        AnimAnim.AnimationId = "rbxassetid://18249294373"
        local Anim = Humanoid:LoadAnimation(AnimAnim)
        
        local startTime = 0
        
        Anim:Play()
        Anim:AdjustSpeed(0.1)
        Anim.TimePosition = startTime
        Anim:AdjustSpeed(1)
    end
end

humanoid.AnimationPlayed:Connect(onAnimationPlayed3)

local animationId4 = 12510170988

local function onAnimationPlayed4(animationTrack)
    if animationTrack.Animation.AnimationId == "rbxassetid://" .. animationId4 then
        local p = game.Players.LocalPlayer
        local Humanoid = p.Character:WaitForChild("Humanoid")
        
        for _, animTrack in pairs(Humanoid:GetPlayingAnimationTracks()) do
            animTrack:Stop()
        end
        
        local AnimAnim = Instance.new("Animation")
        AnimAnim.AnimationId = "rbxassetid://18897119503"
        local Anim = Humanoid:LoadAnimation(AnimAnim)
        
        local startTime = 0
        
        Anim:Play()
        Anim:AdjustSpeed(0.1)
        Anim.TimePosition = startTime
        Anim:AdjustSpeed(1)
    end
end

humanoid.AnimationPlayed:Connect(onAnimationPlayed4)
