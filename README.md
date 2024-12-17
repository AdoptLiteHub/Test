local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "Slash Hub",
    SubTitle = "by dawid",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

local Tabs = {
    Main = Window:AddTab({ Title = "Home", Icon = "" }),
    AutoFarm = Window:AddTab({ Title = "Auto Farm/Teleport", Icon = "" })
}

Tabs.Main:AddParagraph({
    Title = "Discord Server Link:",
    Content = "Click the button below to copy the Discord Server Link!\nLink: https://discord.gg/RHJECBgy"
})

Tabs.Main:AddButton({
    Title = "Copy Discord Server Link",
    Description = "Copies the Discord server link to your clipboard.",
    Callback = function()
        setclipboard("https://discord.gg/RHJECBgy")
        
        Window:Dialog({
            Title = "Link Copied",
            Content = "The Discord Server Link has been copied to your clipboard.",
            Buttons = {
                {
                    Title = "OK",
                    Callback = function() end
                }
            }
        })
    end
})

Tabs.Main:AddParagraph({
    Title = "LocalPlayer",
    Content = "Modify the settings below to adjust your character's movement and jump."
})

-- WalkSpeed Slider
local WalkSpeedSlider = Tabs.Main:AddSlider("WalkSpeed", {
    Title = "WalkSpeed",
    Description = "Set the WalkSpeed for the player.",
    Default = 16,
    Min = 16,
    Max = 1000,
    Rounding = 1,
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
    end
})

-- JumpPower Slider
local JumpPowerSlider = Tabs.Main:AddSlider("JumpPower", {
    Title = "JumpPower",
    Description = "Set the JumpPower for the player.",
    Default = 50,
    Min = 40,
    Max = 1000,
    Rounding = 1,
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = Value
    end
})

-- Infinite Jump Toggle
local InfiniteJumpToggle = Tabs.Main:AddToggle("InfiniteJump", {
    Title = "Infinite Jump",
    Description = "Enable/Disable Infinite Jump for the player.",
    Default = false,
    Callback = function(State)
        local humanoid = game.Players.LocalPlayer.Character:WaitForChild("Humanoid")
        if State then
            humanoid.JumpHeight = 50
            humanoid:GetPropertyChangedSignal("Jumping"):Connect(function()
                if humanoid.Jumping then
                    humanoid:ChangeState(Enum.HumanoidStateType.Physics)
                    humanoid:Move(Vector3.new(0, 0, 0))
                end
            end)
        else
            humanoid.JumpHeight = 50
        end
    end
})

-- Auto Farm/Teleport Tab
Tabs.AutoFarm:AddParagraph({
    Title = "Island Teleport",
    Content = "Select an island to teleport to and enable teleportation."
})

-- Dropdown to choose island
local IslandDropdown = Tabs.AutoFarm:AddDropdown("ChooseIsland", {
    Title = "Choose Island",
    Description = "Select the island you want to teleport to.",
    Options = {
        "Tiny Island",
        "Frozen Island",
        "Mythical Island",
        "Inferno Island",
        "Legends Island",
        "Muscle King Island",
        "Secret Area"
    },
    Default = "Tiny Island",
    Callback = function(selectedIsland)
        game.ReplicatedStorage.SelectedIsland = selectedIsland
    end
})

-- Teleport Toggle for Island
local TeleportToggle = Tabs.AutoFarm:AddToggle("TeleportToIsland", {
    Title = "Teleport To Island",
    Description = "Enable to teleport to the selected island.",
    Default = false,
    Callback = function(State)
        if State then
            local selectedIsland = game.ReplicatedStorage.SelectedIsland.Value
            local islandPosition

            if selectedIsland == "Tiny Island" then
                islandPosition = CFrame.new(-38, 5, 1884) 
            elseif selectedIsland == "Frozen Island" then
                islandPosition = CFrame.new(-2623, 5, -409)
            elseif selectedIsland == "Mythical Island" then
                islandPosition = CFrame.new(2251, 5, 1073)
            elseif selectedIsland == "Inferno Island" then
                islandPosition = CFrame.new(-6759, 5, -1285)
            elseif selectedIsland == "Legends Island" then
                islandPosition = CFrame.new(4603, 989, -3898)
            elseif selectedIsland == "Muscle King Island" then
                islandPosition = CFrame.new(-8626, 15, -5730)
            elseif selectedIsland == "Secret Area" then
                islandPosition = CFrame.new(-2596, -1, 5738)
            end

            game.Players.LocalPlayer.Character:MoveTo(islandPosition.Position)
        end
    end
})

-- Teleport To Rock Section
Tabs.AutoFarm:AddParagraph({
    Title = "Teleport To Rock",
    Content = "Select a rock to teleport to and enable teleportation."
})

-- Dropdown to select rock
local RockDropdown = Tabs.AutoFarm:AddDropdown("SelectRock", {
    Title = "Select Rock",
    Description = "Choose the rock you want to teleport to.",
    Options = {
        "Frozen Rock",
        "Mythical Rock",
        "Inferno Rock",
        "Legends Rock",
        "Muscle King Mountain"
    },
    Default = "Frozen Rock",
    Callback = function(selectedRock)
        game.ReplicatedStorage.SelectedRock = selectedRock
    end
})

-- Teleport Toggle for Rock
local TeleportRockToggle = Tabs.AutoFarm:AddToggle("TeleportToRock", {
    Title = "Teleport To Rock",
    Description = "Enable to teleport to the selected rock.",
    Default = false,
    Callback = function(State)
        if State then
            local selectedRock = game.ReplicatedStorage.SelectedRock.Value
            local rockPosition

            if selectedRock == "Frozen Rock" then
                rockPosition = game.Workspace:WaitForChild("Frozen Rock").CFrame
            elseif selectedRock == "Mythical Rock" then
                rockPosition = game.Workspace:WaitForChild("Mythical Rock").CFrame
            elseif selectedRock == "Inferno Rock" then
                rockPosition = game.Workspace:WaitForChild("Inferno Rock").CFrame
            elseif selectedRock == "Legends Rock" then
                rockPosition = game.Workspace:WaitForChild("Legends Rock").CFrame
            elseif selectedRock == "Muscle King Mountain" then
                rockPosition = game.Workspace:WaitForChild("Muscle King Mountain").CFrame
            end

            game.Players.LocalPlayer.Character:MoveTo(rockPosition.Position)
        end
    end
})
