local Library = loadstring(game:HttpGetAsync("https://github.com/ActualMasterOogway/Fluent-Renewed/releases/latest/download/Fluent.luau"))()
local SaveManager = loadstring(game:HttpGetAsync("https://raw.githubusercontent.com/ActualMasterOogway/Fluent-Renewed/master/Addons/SaveManager.luau"))()
local InterfaceManager = loadstring(game:HttpGetAsync("https://raw.githubusercontent.com/ActualMasterOogway/Fluent-Renewed/master/Addons/InterfaceManager.luau"))()

-- Create the Fluent Window
local Window = Library:CreateWindow{
    Title = `Slash Hub`,
    SubTitle = "",
    TabWidth = 160,
    Size = UDim2.fromOffset(830, 525),
    Resize = true, -- Resize this ^ Size according to a 1920x1080 screen, good for mobile users but may look weird on some devices
    MinSize = Vector2.new(470, 380),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.RightControl -- Used when theres no MinimizeKeybind
}

-- Create Tabs
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

-- Create Transparent Toggle Icon
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Create ImageButton (Icon) to toggle transparency
local toggleButton = Instance.new("ImageButton")
toggleButton.Size = UDim2.new(0, 50, 0, 50)  -- Size of the icon
toggleButton.Position = UDim2.new(0.5, -25, 0.5, -25)  -- Center the icon
toggleButton.BackgroundTransparency = 1  -- No background
toggleButton.Image = "rbxassetid://95279616007991"  -- Your custom icon ID
toggleButton.Parent = screenGui

local isTransparent = false  -- Variable to track transparency state

-- Function to toggle the transparency of the main window
local function toggleTransparency()
    if isTransparent then
        -- Set the Fluent window to visible
        Window.BackgroundTransparency = 0
    else
        -- Set the Fluent window to transparent
        Window.BackgroundTransparency = 1
    end
    isTransparent = not isTransparent  -- Toggle state
end

-- Connect the button to the toggle function
toggleButton.MouseButton1Click:Connect(toggleTransparency)

-- Track window minimize state
local isMinimized = false

-- Function to handle when the window is minimized
Window.Minimize = function()
    isMinimized = true
    -- When minimized, set Fluent window to transparent
    Window.BackgroundTransparency = 1
end

-- Function to restore visibility when the icon is clicked
toggleButton.MouseButton1Click:Connect(function()
    if isMinimized then
        -- If minimized, restore the window and make it visible
        isMinimized = false
        Window.BackgroundTransparency = 0
    end
    -- If not minimized, toggle the transparency as usual
    toggleTransparency()
end)

-- Your existing AutoFarm, Killing, and other sections would go here...

-- Example AutoFarm Toggle
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
            -- Start killing the selected player (teleport their HumanoidRootPart continuously to your RightHand)
            while KillPlayerToggle.Value and targetCharacter do
                local humanoidRootPart = targetCharacter:FindFirstChild("HumanoidRootPart")
                if humanoidRootPart then
                    -- Teleport the HumanoidRootPart to the RightHand continuously
                    humanoidRootPart.CFrame = game.Players.LocalPlayer.Character["RightHand"].CFrame
                end
                wait(0.1)  -- This will teleport every 0.1 seconds, adjust the speed if needed
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
