local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/memejames/elerium-v2-ui-library/main/Library", true))()

local window = library:AddWindow("Test Script", {
    main_color = Color3.fromRGB(41, 74, 122), -- Color
    min_size = Vector2.new(250, 346), -- Size of the gui
    can_resize = false, -- true or false
})

local killTab = window:AddTab("Kill")

local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local backpack = player:WaitForChild("Backpack")
local punchTool = backpack:WaitForChild("Punch")
local punchRange = 50
local switch

local function getNearestPlayer()
    local closestPlayer = nil
    local shortestDistance = punchRange
    
    for _, otherPlayer in pairs(game.Players:GetPlayers()) do
        if otherPlayer ~= player then
            local character = otherPlayer.Character
            if character and character:FindFirstChild("HumanoidRootPart") then
                local distance = (character.HumanoidRootPart.Position - char.HumanoidRootPart.Position).Magnitude
                if distance < shortestDistance then
                    closestPlayer = otherPlayer
                    shortestDistance = distance
                end
            end
        end
    end

    return closestPlayer
end

local function usePunchOnTarget(targetPlayer)
    local targetCharacter = targetPlayer.Character
    if targetCharacter and targetCharacter:FindFirstChild("Humanoid") then
        punchTool:Activate()
    end
end

local function toggleAutoKill(bool)
    if bool then
        while switch:Get() do
            if punchTool.Parent == backpack then
                player.Character:FindFirstChild("Humanoid"):EquipTool(punchTool)
                wait(0.5)
                local targetPlayer = getNearestPlayer()
                if targetPlayer then
                    usePunchOnTarget(targetPlayer)
                end
            end
            wait(0.1)
        end
    end
end

switch = killTab:AddSwitch("Auto Kill", toggleAutoKill)
switch:Set(true)
