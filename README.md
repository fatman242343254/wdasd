local FillColor = Color3.fromRGB(175,25,255)
local DepthMode = "AlwaysOnTop"
local FillTransparency = 0.5
local OutlineColor = Color3.fromRGB(255,255,255)
local OutlineTransparency = 0

local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local UIS = game:GetService("UserInputService")

local Storage = Instance.new("Folder")
Storage.Parent = CoreGui
Storage.Name = "Highlight_Storage"

local ESPEnabled = true

local function Highlight(plr)
    local Highlight = Instance.new("Highlight")
    Highlight.Name = plr.Name
    Highlight.FillColor = FillColor
    Highlight.DepthMode = DepthMode
    Highlight.FillTransparency = FillTransparency
    Highlight.OutlineColor = OutlineColor
    Highlight.OutlineTransparency = OutlineTransparency
    Highlight.Parent = Storage
    
    local plrchar = plr.Character
    if plrchar then
        Highlight.Adornee = plrchar
    end

    plr.CharacterAdded:Connect(function(char)
        Highlight.Adornee = char
    end)
end

local function ToggleESP()
    ESPEnabled = not ESPEnabled
    if not ESPEnabled then
        for _, child in ipairs(Storage:GetChildren()) do
            child:Destroy()
        end
    else
        for _, player in ipairs(Players:GetPlayers()) do
            Highlight(player)
        end
    end
end

UIS.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.Z then
        ToggleESP()
    end
end)

Players.PlayerAdded:Connect(function(player)
    if ESPEnabled then
        Highlight(player)
    end
end)

Players.PlayerRemoving:Connect(function(player)
    local highlight = Storage:FindFirstChild(player.Name)
    if highlight then
        highlight:Destroy()
    end
end)
