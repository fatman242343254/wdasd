local FillColor = Color3.fromRGB(175,25,255)
local DepthMode = "AlwaysOnTop"
local FillTransparency = 0.5
local OutlineColor = Color3.fromRGB(255,255,255)
local OutlineTransparency = 0

local Players = game:GetService("Players")
local Storage = Instance.new("Folder")
Storage.Parent = game:GetService("CoreGui")
Storage.Name = "Highlight_Storage"

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

Players.PlayerAdded:Connect(Highlight)
for _, player in ipairs(Players:GetPlayers()) do
    Highlight(player)
end

Players.PlayerRemoving:Connect(function(plr)
    local plrname = plr.Name
    local highlight = Storage:FindFirstChild(plrname)
    if highlight then
        highlight:Destroy()
    end
end)
