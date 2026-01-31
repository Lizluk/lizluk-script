--// LIZLUK ALL-IN-ONE 🦋 FULL
-- Executor only

------------------ LOCK ------------------
local ALLOWED_USERID = 123456789 -- <<< ĐỔI THÀNH USERID CỦA BẠN
local Players = game:GetService("Players")
local player = Players.LocalPlayer
if player.UserId ~= ALLOWED_USERID then return end

------------------ CONFIG ------------------
_G.LIZLUK_CONFIG = {
    Core = {
        Health = 5000, MaxHealth = 5000,
        BaseDamage = 1, AttackSpeed = 1.5,
        Stamina = 10000, MaxStamina = 10000
    },
    AttributesGui = {
        Dexterity = 20, Regeneration = 20, Stamina = 10000,
        Strength = 500, Stride = 20, StrikeGuard = 20, Vitality = 5000
    },
    StatisticsGui = {
        Enabled = true,
        TotalBossDamageGiven = 585308,
        TotalDamageGiven = 1457890
    }
}
local DATA = _G.LIZLUK_CONFIG

------------------ APPLY ON RESPAWN ------------------
local function applyOnce(char)
    if not char then return end
    for k,v in pairs(DATA.Core) do pcall(function() char:SetAttribute(k,v) end) end
    for _,d in ipairs(char:GetDescendants()) do
        if (d:IsA("NumberValue") or d:IsA("IntValue")) and DATA.Core[d.Name] then
            d.Value = DATA.Core[d.Name]
        end
    end
    local hum = char:FindFirstChildOfClass("Humanoid")
    if hum then hum.MaxHealth = DATA.Core.MaxHealth; hum.Health = DATA.Core.Health end
    pcall(function()
        local g = player:WaitForChild("PlayerGui")
        local attr = g.Gui.PlayerFolder.AccountStats.Attributes
        for k,v in pairs(DATA.AttributesGui) do
            local o = attr:FindFirstChild(k)
            if o and o:IsA("ValueBase") then o.Value = v end
        end
    end)
end

player.CharacterAdded:Connect(function(c) task.wait(1.2); applyOnce(c) end)
if player.Character then applyOnce(player.Character) end

------------------ STAT LOOP ------------------
task.spawn(function()
    while player and player.Parent do
        if DATA.StatisticsGui.Enabled then
            pcall(function()
                local g = player:FindFirstChild("PlayerGui"); if not g then return end
                local stats = g.Gui.PlayerFolder.AccountStats.Statistics
                for k,v in pairs(DATA.StatisticsGui) do
                    if k~="Enabled" then
                        local o = stats:FindFirstChild(k)
                        if o and o:IsA("ValueBase") then o.Value = v end
                    end
                end
            end)
        end
        task.wait(1)
    end
end)

------------------ GUI ------------------
local gui = Instance.new("ScreenGui", player.PlayerGui)
gui.Name="LizlukGui"; gui.ResetOnSpawn=false

local main = Instance.new("Frame", gui)
main.Size=UDim2.fromScale(0.36,0.64)
main.Position=UDim2.fromScale(0.32,0.18)
main.BackgroundColor3=Color3.fromRGB(8,8,12)
main.BorderSizePixel=0
Instance.new("UICorner",main).CornerRadius=UDim.new(0,28)

local stroke=Instance.new("UIStroke",main)
stroke.Color=Color3.fromRGB(255,255,255); stroke.Transparency=0.5

-- Butterfly
local butterfly=Instance.new("ImageLabel",main)
butterfly.Image="rbxassetid://IMAGE_ID_BUTTERFLY" -- <<< ĐỔI ID
butterfly.Size=UDim2.fromScale(0.28,0.22)
butterfly.Position=UDim2.fromScale(0.36,0.03)
butterfly.BackgroundTransparency=1

-- Butterfly animation (wing + glow)
task.spawn(function()
    local t=0
    while butterfly.Parent do
        t+=0.05
        butterfly.Rotation=math.sin(t)*5
        stroke.Transparency=0.35+math.abs(math.sin(t))*0.3
        task.wait(0.03)
    end
end)

local title=Instance.new("TextLabel",main)
title.Text="lizluk"; title.Font=Enum.Font.GothamBold
title.TextSize=36; title.TextColor3=Color3.new(1,1,1)
title.TextStrokeTransparency=0.35
title.Size=UDim2.fromScale(1,0.1)
title.Position=UDim2.fromScale(0,0.26)
title.BackgroundTransparency=1

------------------ HELPERS ------------------
local function slider(label, min, max, value, cb, y)
    local frame=Instance.new("Frame",main)
    frame.Size=UDim2.fromScale(0.9,0.07)
    frame.Position=UDim2.fromScale(0.05,y)
    frame.BackgroundColor3=Color3.fromRGB(18,18,26)
    Instance.new("UICorner",frame)

    local txt=Instance.new("TextLabel",frame)
    txt.Text=label..": "..value
    txt.Size=UDim2.fromScale(1,0.45)
    txt.BackgroundTransparency=1
    txt.TextColor3=Color3.new(1,1,1)
    txt.TextSize=14

    local bar=Instance.new("Frame",frame)
    bar.Position=UDim2.fromScale(0.05,0.6)
    bar.Size=UDim2.fromScale(0.9,0.18)
    bar.BackgroundColor3=Color3.fromRGB(40,40,60)
    Instance.new("UICorner",bar)

    local fill=Instance.new("Frame",bar)
    fill.Size=UDim2.fromScale((value-min)/(max-min),1)
    fill.BackgroundColor3=Color3.fromRGB(200,200,255)
    Instance.new("UICorner",fill)

    local UIS=game:GetService("UserInputService")
    local dragging=false
    bar.InputBegan:Connect(function(i) if i.UserInputType==Enum.UserInputType.MouseButton1 then dragging=true end end)
    UIS.InputEnded:Connect(function(i) if i.UserInputType==Enum.UserInputType.MouseButton1 then dragging=false end end)
    UIS.InputChanged:Connect(function(i)
        if dragging and i.UserInputType==Enum.UserInputType.MouseMovement then
            local r=(i.Position.X-bar.AbsolutePosition.X)/bar.AbsoluteSize.X
            r=math.clamp(r,0,1)
            local v=math.floor(min+(max-min)*r)
            fill.Size=UDim2.fromScale(r,1)
            txt.Text=label..": "..v
            cb(v)
        end
    end)
end

local function input(label, value, cb, y)
    local b=Instance.new("TextBox",main)
    b.Text=tostring(value); b.PlaceholderText=label
    b.Size=UDim2.fromScale(0.9,0.065)
    b.Position=UDim2.fromScale(0.05,y)
    b.BackgroundColor3=Color3.fromRGB(20,20,30)
    b.TextColor3=Color3.new(1,1,1)
    b.ClearTextOnFocus=false
    Instance.new("UICorner",b)
    b.FocusLost:Connect(function(e)
        if e then local n=tonumber(b.Text); if n then cb(n) end end
    end)
end

------------------ ATTRIBUTES (SLIDERS) ------------------
local y=0.38
for k,v in pairs(DATA.AttributesGui) do
    slider(k,0,10000,v,function(n) DATA.AttributesGui[k]=n end,y)
    y+=0.075
end

------------------ STAT TOGGLE ------------------
local t=Instance.new("TextButton",main)
t.Size=UDim2.fromScale(0.9,0.07)
t.Position=UDim2.fromScale(0.05,y)
t.BackgroundColor3=Color3.fromRGB(35,35,55)
t.TextColor3=Color3.new(1,1,1)
t.Text="Statistics: ON"
Instance.new("UICorner",t)
t.MouseButton1Click:Connect(function()
    DATA.StatisticsGui.Enabled=not DATA.StatisticsGui.Enabled
    t.Text="Statistics: "..(DATA.StatisticsGui.Enabled and "ON" or "OFF")
end)

y+=0.08
for k,v in pairs(DATA.StatisticsGui) do
    if k~="Enabled" then
        input(k,v,function(n) DATA.StatisticsGui[k]=n end,y)
        y+=0.07
    end
end

------------------ INSERT TOGGLE ------------------
local UIS=game:GetService("UserInputService")
UIS.InputBegan:Connect(function(i,g)
    if not g and i.KeyCode==Enum.KeyCode.Insert then
        gui.Enabled=not gui.Enabled
    end
end)
