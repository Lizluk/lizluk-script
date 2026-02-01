--// LIZLUK GHOST GUI 🦋 FULL FINAL (CORE EDITABLE)
-- Executor only

repeat task.wait() until game:IsLoaded()

---------------- SERVICES ----------------
local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer

---------------- USER LOCK ----------------
local ALLOWED_USERID = 8076792445 -- <<< ĐỔI USERID CỦA BẠN
if player.UserId ~= ALLOWED_USERID then return end

---------------- CONFIG ----------------
_G.LIZLUK_CONFIG = {
    Core = {
        MaxHealth = 5000,
        Health = 5000,
        MaxStamina = 10000,
        Stamina = 10000,
        BaseDamage = 1,
        AttackSpeed = 1.5
    },
    AttributesGui = {
        Dexterity = 20,
        Regeneration = 20,
        Stamina = 10000,
        Strength = 500,
        Stride = 20,
        StrikeGuard = 20,
        Vitality = 5000
    },
    StatisticsGui = {
        Enabled = true,
        TotalBossDamageGiven = 585308,
        TotalDamageGiven = 1457890
    }
}
local DATA = _G.LIZLUK_CONFIG

---------------- APPLY CORE ----------------
local function applyCore(char)
    if not char then return end

    for k,v in pairs(DATA.Core) do
        pcall(function() char:SetAttribute(k,v) end)
    end

    for _,d in ipairs(char:GetDescendants()) do
        if (d:IsA("NumberValue") or d:IsA("IntValue")) and DATA.Core[d.Name] then
            d.Value = DATA.Core[d.Name]
        end
    end

    local hum = char:FindFirstChildOfClass("Humanoid")
    if hum then
        hum.MaxHealth = DATA.Core.MaxHealth
        hum.Health = math.clamp(DATA.Core.Health, 0, DATA.Core.MaxHealth)
    end
end

---------------- APPLY ATTRIBUTES ----------------
local function applyAttributes()
    pcall(function()
        local attr = player.PlayerGui.Gui.PlayerFolder.AccountStats.Attributes
        for k,v in pairs(DATA.AttributesGui) do
            local o = attr:FindFirstChild(k)
            if o then o.Value = v end
        end
    end)
end

---------------- RESPAWN ----------------
player.CharacterAdded:Connect(function(c)
    task.wait(1.2)
    applyCore(c)
    applyAttributes()
end)
if player.Character then
    applyCore(player.Character)
    applyAttributes()
end

---------------- STAT LOOP ----------------
task.spawn(function()
    while player and player.Parent do
        if DATA.StatisticsGui.Enabled then
            pcall(function()
                local stats = player.PlayerGui.Gui.PlayerFolder.AccountStats.Statistics
                for k,v in pairs(DATA.StatisticsGui) do
                    if k ~= "Enabled" then
                        local o = stats:FindFirstChild(k)
                        if o then o.Value = v end
                    end
                end
            end)
        end
        task.wait(1)
    end
end)

---------------- GUI ROOT ----------------
local gui = Instance.new("ScreenGui", gethui())
gui.ResetOnSpawn = false

---------------- MAIN SCROLL ----------------
local main = Instance.new("ScrollingFrame", gui)
main.Size = UDim2.fromScale(0.36,0.68)
main.Position = UDim2.fromScale(0.32,0.16)
main.CanvasSize = UDim2.fromScale(0,2)
main.ScrollBarThickness = 6
main.BackgroundColor3 = Color3.fromRGB(8,8,12)
main.BorderSizePixel = 0
Instance.new("UICorner",main).CornerRadius = UDim.new(0,28)

local stroke = Instance.new("UIStroke", main)
stroke.Color = Color3.fromRGB(200,170,255)
stroke.Transparency = 0.35

---------------- TITLE ----------------
local title = Instance.new("TextLabel", main)
title.Text = "lizluk"
title.Font = Enum.Font.GothamBold
title.TextSize = 36
title.TextColor3 = Color3.fromRGB(235,225,255)
title.Size = UDim2.fromScale(1,0.07)
title.BackgroundTransparency = 1

---------------- INPUT BOX ----------------
local function inputBox(label, value, cb, y)
    local box = Instance.new("TextBox", main)
    box.Text = label..": "..value
    box.Size = UDim2.fromScale(0.9,0.055)
    box.Position = UDim2.fromScale(0.05,y)
    box.BackgroundColor3 = Color3.fromRGB(20,20,30)
    box.TextColor3 = Color3.new(1,1,1)
    box.ClearTextOnFocus = true
    Instance.new("UICorner", box)

    box.FocusLost:Connect(function(enter)
        if enter then
            local n = tonumber(box.Text)
            if n then
                cb(n)
                box.Text = label..": "..n
            end
        end
    end)
end

---------------- CORE INPUT ----------------
local y = 0.08
inputBox("MaxHealth", DATA.Core.MaxHealth, function(n)
    DATA.Core.MaxHealth = n
    if player.Character then applyCore(player.Character) end
end, y)

y += 0.06
inputBox("Health", DATA.Core.Health, function(n)
    DATA.Core.Health = n
    if player.Character then applyCore(player.Character) end
end, y)

y += 0.06
inputBox("MaxStamina", DATA.Core.MaxStamina, function(n)
    DATA.Core.MaxStamina = n
    if player.Character then applyCore(player.Character) end
end, y)

y += 0.06
inputBox("Stamina", DATA.Core.Stamina, function(n)
    DATA.Core.Stamina = n
    if player.Character then applyCore(player.Character) end
end, y)

---------------- APPLY ATTRIBUTES BUTTON ----------------
y += 0.07
local applyBtn = Instance.new("TextButton", main)
applyBtn.Size = UDim2.fromScale(0.9,0.06)
applyBtn.Position = UDim2.fromScale(0.05,y)
applyBtn.Text = "Apply Attributes Now"
applyBtn.BackgroundColor3 = Color3.fromRGB(55,35,85)
applyBtn.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", applyBtn)

applyBtn.MouseButton1Click:Connect(applyAttributes)

---------------- ATTRIBUTES ----------------
y += 0.07
for k,v in pairs(DATA.AttributesGui) do
    inputBox(k, v, function(n)
        DATA.AttributesGui[k] = n
    end, y)
    y += 0.06
end

---------------- STAT TOGGLE ----------------
local toggle = Instance.new("TextButton", main)
toggle.Size = UDim2.fromScale(0.9,0.055)
toggle.Position = UDim2.fromScale(0.05,y)
toggle.Text = "Statistics: ON"
toggle.BackgroundColor3 = Color3.fromRGB(45,30,70)
toggle.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", toggle)

toggle.MouseButton1Click:Connect(function()
    DATA.StatisticsGui.Enabled = not DATA.StatisticsGui.Enabled
    toggle.Text = "Statistics: "..(DATA.StatisticsGui.Enabled and "ON" or "OFF")
end)

y += 0.06
inputBox("TotalDamageGiven", DATA.StatisticsGui.TotalDamageGiven,
    function(n) DATA.StatisticsGui.TotalDamageGiven = n end, y)

y += 0.06
inputBox("TotalBossDamageGiven", DATA.StatisticsGui.TotalBossDamageGiven,
    function(n) DATA.StatisticsGui.TotalBossDamageGiven = n end, y)

---------------- INSERT TOGGLE ----------------
UIS.InputBegan:Connect(function(i,g)
    if not g and i.KeyCode == Enum.KeyCode.Insert then
        gui.Enabled = not gui.Enabled
    end
end)

---------------- BUTTERFLY ORBIT ----------------
local BUTTERFLY_ID = "rbxassetid://11254757054"
local COUNT = 3
local RADIUS_X, RADIUS_Y = 0.22, 0.28
local SPEED = 0.8

local butterflies = {}
for i=1,COUNT do
    local b = Instance.new("ImageLabel", gui)
    b.Image = BUTTERFLY_ID
    b.Size = UDim2.fromScale(0.06,0.09)
    b.BackgroundTransparency = 1
    b.ZIndex = 999

    local glow = Instance.new("UIStroke", b)
    glow.Color = Color3.fromRGB(210,170,255)
    glow.Thickness = 2
    glow.Transparency = 0.35

    butterflies[i] = {ui=b, glow=glow, off=(math.pi*2/COUNT)*i}
end

local t = 0
RunService.RenderStepped:Connect(function(dt)
    if not gui.Enabled then return end
    t += dt*SPEED

    local center = main.AbsolutePosition + main.AbsoluteSize/2
    local vp = workspace.CurrentCamera.ViewportSize

    for _,d in ipairs(butterflies) do
        local a = t + d.off
        local x = center.X + math.cos(a)*(RADIUS_X*vp.X)
        local y = center.Y + math.sin(a)*(RADIUS_Y*vp.Y)

        d.ui.Position = UDim2.fromOffset(
            x - d.ui.AbsoluteSize.X/2,
            y - d.ui.AbsoluteSize.Y/2
        )
        d.ui.Rotation = math.sin(a*2)*10
        d.glow.Transparency = 0.25 + math.abs(math.sin(a))*0.4
    end
end)
