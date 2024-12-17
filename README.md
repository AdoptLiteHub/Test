local Library = loadstring(game:HttpGetAsync("https://github.com/ActualMasterOogway/Fluent-Renewed/releases/latest/download/Fluent.luau"))()
local SaveManager = loadstring(game:HttpGetAsync("https://raw.githubusercontent.com/ActualMasterOogway/Fluent-Renewed/master/Addons/SaveManager.luau"))()
local InterfaceManager = loadstring(game:HttpGetAsync("https://raw.githubusercontent.com/ActualMasterOogway/Fluent-Renewed/master/Addons/InterfaceManager.luau"))()

local Window = Library:CreateWindow{
    Title = `Slash Hub`,
    SubTitle = "",
    TabWidth = 160,
    Size = UDim2.fromOffset(830, 525),
    Resize = true,
    MinSize = Vector2.new(470, 380),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.RightControl
}

-- Create tabs
local Tabs = {
    Main = Window:CreateTab{
        Title = "Home",
        Icon = ""
    },
    AutoFarm = Window:CreateTab{
        Title = "Auto Farm",
        Icon = ""
    },
    Killing = Window:CreateTab{
        Title = "Killing",
        Icon = ""
    }
}

local Options = Library.Options

Library:Notify{
    Title = "Notification",
    Content = "Welcome To Slash Hub",
    SubContent = "SubContent", -- Optional
    Duration = 5 -- Set to nil to make the notification not disappear
}

-- Auto Farm Section
-- Auto Weight Toggle
local AutoWeightToggle = Tabs.AutoFarm:CreateToggle("AutoWeight", {
    Title = "Auto Weight",
    Default = false
})

AutoWeightToggle:OnChanged(function()
    if AutoWeightToggle.Value then
        -- Start auto weight (equip tool and use it)
        while AutoWeightToggle.Value do
            local tool = game.Players.LocalPlayer.Backpack:FindFirstChild("Weight")
            if tool then
                tool.Parent = game.Players.LocalPlayer.Character
                -- Assuming the tool has an action to activate, you may need to simulate using the tool
                tool:Activate() -- This might differ depending on the actual tool's script
            end
            wait(0.3) -- Use tool every 0.3 seconds
        end
    end
end)

-- Auto Pushups Toggle
local AutoPushupsToggle = Tabs.AutoFarm:CreateToggle("AutoPushups", {
    Title = "Auto Pushups",
    Default = false
})

AutoPushupsToggle:OnChanged(function()
    if AutoPushupsToggle.Value then
        -- Start auto pushups (equip tool and use it)
        while AutoPushupsToggle.Value do
            local tool = game.Players.LocalPlayer.Backpack:FindFirstChild("Pushups")
            if tool then
                tool.Parent = game.Players.LocalPlayer.Character
                -- Assuming the tool has an action to activate, you may need to simulate using the tool
                tool:Activate() -- This might differ depending on the actual tool's script
            end
            wait(0.3) -- Use tool every 0.3 seconds
        end
    end
end)

-- Auto Handstands Toggle
local AutoHandstandsToggle = Tabs.AutoFarm:CreateToggle("AutoHandstands", {
    Title = "Auto Handstands",
    Default = false
})

AutoHandstandsToggle:OnChanged(function()
    if AutoHandstandsToggle.Value then
        -- Start auto handstands (equip tool and use it)
        while AutoHandstandsToggle.Value do
            local tool = game.Players.LocalPlayer.Backpack:FindFirstChild("Handstands")
            if tool then
                tool.Parent = game.Players.LocalPlayer.Character
                -- Assuming the tool has an action to activate, you may need to simulate using the tool
                tool:Activate() -- This might differ depending on the actual tool's script
            end
            wait(0.3) -- Use tool every 0.3 seconds
        end
    end
end)

-- Auto Situps Toggle
local AutoSitupsToggle = Tabs.AutoFarm:CreateToggle("AutoSitups", {
    Title = "Auto Situps",
    Default = false
})

AutoSitupsToggle:OnChanged(function()
    if AutoSitupsToggle.Value then
        -- Start auto situps (equip tool and use it)
        while AutoSitupsToggle.Value do
            local tool = game.Players.LocalPlayer.Backpack:FindFirstChild("Situps")
            if tool then
                tool.Parent = game.Players.LocalPlayer.Character
                -- Assuming the tool has an action to activate, you may need to simulate using the tool
                tool:Activate() -- This might differ depending on the actual tool's script
            end
            wait(0.3) -- Use tool every 0.3 seconds
        end
    end
end)

-- Killing Section
-- Create Whitelist Dropdown in "Killing" Tab
local PlayersList = {}  -- Will hold the list of all players
for _, player in pairs(game.Players:GetPlayers()) do
    table.insert(PlayersList, player.Name)
end

local WhitelistDropdown = Tabs.Killing:CreateDropdown("Whitelist", {
    Title = "Whitelist A Player",
    Values = PlayersList,
    Multi = false,
    Default = 1,
})

-- Add Auto Killing
local AutoKillToggle = Tabs.Killing:CreateToggle("AutoKill", {
    Title = "Auto Kill",
    Default = false
})

AutoKillToggle:OnChanged(function()
    local whitelist = WhitelistDropdown.Value
    if AutoKillToggle.Value then
        -- Start auto killing other players
        while AutoKillToggle.Value do
            for _, player in pairs(game.Players:GetPlayers()) do
                if player.Name ~= whitelist then
                    -- Make the player's HumanoidRootPart invisible and teleport to right hand
                    local character = player.Character
                    if character and character:FindFirstChild("HumanoidRootPart") then
                        local humanoidRootPart = character.HumanoidRootPart
                        humanoidRootPart.CFrame = game.Players.LocalPlayer.Character["RightHand"].CFrame
                        humanoidRootPart.Transparency = 1
                    end
                end
            end
            wait(0.5)  -- Adjust the wait time as needed
        end
    end
end)

-- Add Target Player Killing
local TargetPlayerDropdown = Tabs.Killing:CreateDropdown("SelectPlayer", {
    Title = "Select Player",
    Values = PlayersList,
    Multi = false,
    Default = 1,
})

local KillPlayerToggle = Tabs.Killing:CreateToggle("KillPlayer", {
    Title = "Kill Player",
    Default = false
})

KillPlayerToggle:OnChanged(function()
    if KillPlayerToggle.Value then
        local targetPlayer = TargetPlayerDropdown.Value
        local targetCharacter = game.Players:FindFirstChild(targetPlayer) and game.Players[targetPlayer].Character
        if targetCharacter then
            -- Start killing the selected player
            while KillPlayerToggle.Value and targetCharacter do
                local humanoidRootPart = targetCharacter:FindFirstChild("HumanoidRootPart")
                if humanoidRootPart then
                    humanoidRootPart.CFrame = game.Players.LocalPlayer.Character["RightHand"].CFrame
                    humanoidRootPart.Transparency = 1
                end
                wait(0.5)  -- Adjust the wait time as needed
            end
        end
    end
end)

-- Add Spy Section
local SpyToggle = Tabs.Killing:CreateToggle("Spy", {
    Title = "Spy",
    Default = false
})

SpyToggle:OnChanged(function()
    if SpyToggle.Value then
        local targetPlayer = TargetPlayerDropdown.Value
        local targetCharacter = game.Players:FindFirstChild(targetPlayer) and game.Players[targetPlayer].Character
        if targetCharacter then
            local targetHumanoidRootPart = targetCharacter:FindFirstChild("HumanoidRootPart")
            if targetHumanoidRootPart then
                -- Move the camera to the target player's HumanoidRootPart (first-person POV)
                game.Workspace.CurrentCamera.CameraSubject = targetCharacter.Humanoid
                game.Workspace.CurrentCamera.CameraType = Enum.CameraType.Attach
            end
        end
    else
        -- Reset to normal camera mode
        game.Workspace.CurrentCamera.CameraType = Enum.CameraType.Custom
        game.Workspace.CurrentCamera.CameraSubject = game.Players.LocalPlayer.Character.Humanoid
    end
end)
