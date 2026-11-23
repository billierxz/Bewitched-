-- ═══════════════════════════════════════════════════════════════════════
-- BEWITCHED V35 - Stomp Safe (Hold Real) + BG+Wheel + Partículas + ESP
-- ═══════════════════════════════════════════════════════════════════════
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")
local StarterGui = game:GetService("StarterGui")
local VIM = game:GetService("VirtualInputManager")
local RunService = game:GetService("RunService")
local CollectionService = game:GetService("CollectionService")
local HttpService = game:GetService("HttpService")
local Debris = game:GetService("Debris")
local LP = Players.LocalPlayer
local PlayerGui = LP:WaitForChild("PlayerGui")

getgenv().AIOConfig = getgenv().AIOConfig or {
    lang = "es",
    regentNotify = false,
    treeStatus = false,
    stompAll = false,
    stompOnly = false,
    team = {},
    targets = {},
    esp = {All=false, Totem=false, DeathRose=false, Ancestor=false, Bones=false, WhiteTree=false, Sigil=false, Dagger=false},
    skillMode = "GREAT",
    skillAuto = true,
    partOn = true,
    partWithBg = false,
    partColor = 1,
    bgOpacity = 0.15,
    wheelOpacity = 0,
}

-- ── SONIDO CLICK ───────────────────────────────────────────────────────
local function playClick()
    pcall(function()
        local s = Instance.new("Sound")
        s.SoundId = "rbxassetid://139719503904449"
        s.Volume = 10
        s.PlaybackSpeed = 1
        s.RollOffMode = Enum.RollOffMode.Linear
        s.Parent = workspace.CurrentCamera or PlayerGui
        s:Play()
        Debris:AddItem(s, 2)
    end)
end

-- ── gethui SAFE ────────────────────────────────────────────────────────
local function getGuiParent()
    local ok, h = pcall(function() return gethui() end)
    if ok and h then return h end
    local ok2, cg = pcall(function() return game:GetService("CoreGui") end)
    if ok2 and cg then return cg end
    return PlayerGui
end

local GUI_RANDOM = "F"..tostring(math.random(1e6,9e6))
local function cleanOld()
    local parent = getGuiParent()
    for _,g in ipairs(parent:GetChildren()) do
        if g:IsA("ScreenGui") and g:GetAttribute("V34") then pcall(function() g:Destroy() end) end
    end
    for _,name in ipairs({"ForceGUI","BewitchedESP","BewitchedESP_V31","BewitchedESP_V32","BewitchedESP_V33"}) do
        if PlayerGui:FindFirstChild(name) then pcall(function() PlayerGui[name]:Destroy() end) end
    end
end
cleanOld()

local isMobile = UIS.TouchEnabled and not UIS.KeyboardEnabled
local baseSize = isMobile and Vector2.new(320, 480) or Vector2.new(400, 520)
local minSize = Vector2.new(300, 400)
local maxSize = Vector2.new(560, 700)

local Gui = Instance.new("ScreenGui")
Gui.Name = GUI_RANDOM
Gui.ResetOnSpawn = false
Gui.IgnoreGuiInset = true
Gui.Parent = getGuiParent()
Gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
Gui:SetAttribute("V34", true)
if syn and syn.protect_gui then pcall(function() syn.protect_gui(Gui) end) end

local function ensureGui()
    if not Gui or not Gui.Parent then pcall(function() Gui.Parent = getGuiParent() end) end
end
LP.CharacterAdded:Connect(function() task.wait(1) ensureGui() end)
task.spawn(function() while true do task.wait(2) pcall(ensureGui) end end)

-- ── HOLDERS ────────────────────────────────────────────────────────────
local NotifHolder = Instance.new("Frame", Gui)
NotifHolder.Size = UDim2.new(1,0,1,0)
NotifHolder.BackgroundTransparency = 1
NotifHolder.ZIndex = 100

local TrailHolder = Instance.new("Frame", Gui)
TrailHolder.Size = UDim2.new(1,0,1,0)
TrailHolder.BackgroundTransparency = 1
TrailHolder.ZIndex = 25

local ESPFolder = Instance.new("Folder")
ESPFolder.Name = "ESP_V34"
ESPFolder.Parent = Gui

-- ── COLORES DE PARTÍCULAS ──────────────────────────────────────────────
local PART_COLORS = {
    {name="Morado", color=Color3.fromRGB(200,160,255)},
    {name="Rosa", color=Color3.fromRGB(255,150,220)},
    {name="Azul", color=Color3.fromRGB(100,180,255)},
    {name="Verde", color=Color3.fromRGB(100,255,150)},
    {name="Amarillo", color=Color3.fromRGB(255,230,100)},
    {name="Rojo", color=Color3.fromRGB(255,120,120)},
    {name="Blanco", color=Color3.fromRGB(255,255,255)},
    {name="Cian", color=Color3.fromRGB(120,255,255)},
}

local function getPartColor()
    return (PART_COLORS[getgenv().AIOConfig.partColor] or PART_COLORS[1]).color
end

-- ── EFECTOS DEDO (AURA SUAVE, solo dentro del Main) ────────────────────
local function spawnTrail(x, y, color)
    color = color or getPartColor()
    local rand = math.random(1,8)
    if rand == 1 then
        local s = Instance.new("Frame", TrailHolder)
        s.Size = UDim2.new(0, 7, 0, 7)
        s.Position = UDim2.new(0,x,0,y)
        s.BackgroundColor3 = color
        s.BackgroundTransparency = 0.1
        s.ZIndex = 25
        s.Rotation = 45
        Instance.new("UICorner", s).CornerRadius = UDim.new(0,2)
        TweenService:Create(s, TweenInfo.new(0.9, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
            Size = UDim2.new(0,2,0,2),
            Position = UDim2.new(0, x+math.random(-30,30), 0, y-40-math.random(15,25)),
            BackgroundTransparency = 1, Rotation = 135
        }):Play()
        Debris:AddItem(s, 1)
    elseif rand == 2 then
        local s = Instance.new("TextLabel", TrailHolder)
        s.Size = UDim2.new(0,18,0,18)
        s.Position = UDim2.new(0,x-9,0,y-9)
        s.BackgroundTransparency = 1
        s.Text = "♥"
        s.TextColor3 = Color3.fromRGB(255,80,170)
        s.Font = Enum.Font.GothamBlack
        s.TextSize = 16
        s.TextStrokeTransparency = 0
        s.TextStrokeColor3 = Color3.fromRGB(0,0,0)
        s.ZIndex = 26
        TweenService:Create(s, TweenInfo.new(1, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {
            Position = UDim2.new(0, x+math.random(-20,20), 0, y-45),
            TextTransparency = 1, TextSize = 6
        }):Play()
        Debris:AddItem(s, 1)
    elseif rand == 3 then
        local holder = Instance.new("Frame", TrailHolder)
        holder.Size = UDim2.new(0,14,0,14)
        holder.Position = UDim2.new(0,x-7,0,y-7)
        holder.BackgroundTransparency = 1
        holder.ZIndex = 26
        local h = Instance.new("Frame", holder)
        h.Size = UDim2.new(0,14,0,4)
        h.Position = UDim2.new(0,0,0,5)
        h.BackgroundColor3 = Color3.fromRGB(255,255,255)
        h.BorderSizePixel = 0
        Instance.new("UICorner", h).CornerRadius = UDim.new(0,2)
        local hs = Instance.new("UIStroke", h)
        hs.Color = color
        hs.Thickness = 1
        local v = Instance.new("Frame", holder)
        v.Size = UDim2.new(0,4,0,14)
        v.Position = UDim2.new(0,5,0,0)
        v.BackgroundColor3 = Color3.fromRGB(255,255,255)
        v.BorderSizePixel = 0
        Instance.new("UICorner", v).CornerRadius = UDim.new(0,2)
        local vs = Instance.new("UIStroke", v)
        vs.Color = color
        vs.Thickness = 1
        TweenService:Create(holder, TweenInfo.new(0.9, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
            Position = UDim2.new(0, x-7+math.random(-25,25), 0, y-40), Rotation = 180
        }):Play()
        Debris:AddItem(holder, 0.9)
    elseif rand == 4 then
        local s = Instance.new("Frame", TrailHolder)
        s.Size = UDim2.new(0, 8, 0, 8)
        s.Position = UDim2.new(0,x,0,y)
        s.BackgroundColor3 = color
        s.ZIndex = 25
        Instance.new("UICorner", s).CornerRadius = UDim.new(0,10)
        TweenService:Create(s, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
            Size = UDim2.new(0,14,0,14),
            Position = UDim2.new(0, x+math.random(-15,15), 0, y-30),
            BackgroundTransparency = 1
        }):Play()
        Debris:AddItem(s, 1)
    elseif rand == 5 then
        local s = Instance.new("TextLabel", TrailHolder)
        s.Size = UDim2.new(0,20,0,20)
        s.Position = UDim2.new(0,x-10,0,y-10)
        s.BackgroundTransparency = 1
        s.Text = "✦"
        s.TextColor3 = Color3.fromRGB(255,230,120)
        s.Font = Enum.Font.GothamBlack
        s.TextSize = 18
        s.TextStrokeTransparency = 0
        s.TextStrokeColor3 = Color3.fromRGB(200,120,40)
        s.ZIndex = 26
        TweenService:Create(s, TweenInfo.new(1.1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
            Position = UDim2.new(0, x+math.random(-40,40), 0, y-60),
            TextTransparency = 1, TextSize = 4, Rotation = math.random(-180,180)
        }):Play()
        Debris:AddItem(s, 1.1)
    elseif rand == 6 then
        local s = Instance.new("Frame", TrailHolder)
        s.Size = UDim2.new(0, 4, 0, 4)
        s.Position = UDim2.new(0,x,0,y)
        s.BackgroundColor3 = Color3.fromRGB(255,255,200)
        s.ZIndex = 26
        Instance.new("UICorner", s).CornerRadius = UDim.new(0,2)
        local glow = Instance.new("UIStroke", s)
        glow.Color = color
        glow.Thickness = 2
        TweenService:Create(s, TweenInfo.new(0.7, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
            Size = UDim2.new(0,1,0,1),
            Position = UDim2.new(0, x+math.random(-50,50), 0, y-math.random(20,50)),
            BackgroundTransparency = 1
        }):Play()
        TweenService:Create(glow, TweenInfo.new(0.7), {Transparency = 1, Thickness = 0}):Play()
        Debris:AddItem(s, 0.7)
    elseif rand == 7 then
        local s = Instance.new("Frame", TrailHolder)
        s.Size = UDim2.new(0, 9, 0, 9)
        s.Position = UDim2.new(0,x,0,y)
        s.BackgroundColor3 = Color3.fromRGB(255,120,220)
        s.BackgroundTransparency = 0.3
        s.Rotation = 45
        s.ZIndex = 25
        Instance.new("UICorner", s).CornerRadius = UDim.new(0,2)
        local st = Instance.new("UIStroke", s)
        st.Color = color
        st.Thickness = 1
        TweenService:Create(s, TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
            Position = UDim2.new(0, x+math.random(-25,25), 0, y-35),
            Rotation = 225, BackgroundTransparency = 1, Size = UDim2.new(0,2,0,2)
        }):Play()
        TweenService:Create(st, TweenInfo.new(1), {Transparency = 1}):Play()
        Debris:AddItem(s, 1)
    else
        local s = Instance.new("TextLabel", TrailHolder)
        s.Size = UDim2.new(0,16,0,16)
        s.Position = UDim2.new(0,x-8,0,y-8)
        s.BackgroundTransparency = 1
        s.Text = "✧"
        s.TextColor3 = color
        s.Font = Enum.Font.GothamBlack
        s.TextSize = 14
        s.TextStrokeTransparency = 0
        s.TextStrokeColor3 = Color3.fromRGB(60,20,120)
        s.ZIndex = 26
        s.Rotation = 0
        TweenService:Create(s, TweenInfo.new(1.2, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {
            Position = UDim2.new(0, x+math.random(-30,30), 0, y-50),
            TextTransparency = 1, Rotation = 360
        }):Play()
        Debris:AddItem(s, 1.2)
    end
end

-- ── NOTIFICACIONES (solo cuando las llamo explícitamente) ──────────────
local function showNotification(title, msg, type)
    type = type or "magic"
    local colors = {
        magic = {bg = Color3.fromRGB(140,80,255), light = Color3.fromRGB(200,160,255), dark = Color3.fromRGB(80,40,160)},
        heart = {bg = Color3.fromRGB(255,90,180), light = Color3.fromRGB(255,160,210), dark = Color3.fromRGB(180,50,110)},
        error = {bg = Color3.fromRGB(255,80,80), light = Color3.fromRGB(255,150,150), dark = Color3.fromRGB(160,40,40)},
        success = {bg = Color3.fromRGB(0,200,120), light = Color3.fromRGB(120,255,180), dark = Color3.fromRGB(0,120,70)},
    }
    local col = colors[type] or colors.magic
    local Notif = Instance.new("Frame", NotifHolder)
    Notif.Size = UDim2.new(0, isMobile and 280 or 340, 0, 88)
    Notif.Position = UDim2.new(0.5, isMobile and -140 or -170, 0, -120)
    Notif.BackgroundColor3 = Color3.fromRGB(16,14,26)
    Notif.BackgroundTransparency = 0.02
    Notif.ZIndex = 100
    Notif.ClipsDescendants = true
    Instance.new("UICorner", Notif).CornerRadius = UDim.new(0,18)
    local NStroke = Instance.new("UIStroke", Notif)
    NStroke.Color = col.bg; NStroke.Thickness = 1.5; NStroke.Transparency = 0.15

    local TopMagic = Instance.new("Frame", Notif)
    TopMagic.Size = UDim2.new(1,-4,0,18)
    TopMagic.Position = UDim2.new(0,2,0,2)
    TopMagic.BackgroundColor3 = Color3.fromRGB(20,16,34)
    TopMagic.BackgroundTransparency = 0.35
    Instance.new("UICorner", TopMagic).CornerRadius = UDim.new(0,12)
    local CD = Instance.new("Frame", TopMagic)
    CD.Size = UDim2.new(0,12,0,12)
    CD.Position = UDim2.new(0.5,-6,0,3)
    CD.BackgroundColor3 = col.light
    CD.Rotation = 45
    Instance.new("UICorner", CD).CornerRadius = UDim.new(0,3)
    task.spawn(function()
        while TopMagic.Parent do
            TweenService:Create(CD, TweenInfo.new(0.6, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Size = UDim2.new(0,14,0,14), Position = UDim2.new(0.5,-7,0,2)}):Play()
            task.wait(0.6)
            TweenService:Create(CD, TweenInfo.new(0.8, Enum.EasingStyle.Sine), {Size = UDim2.new(0,12,0,12), Position = UDim2.new(0.5,-6,0,3)}):Play()
            task.wait(0.8)
        end
    end)

    local IconBox = Instance.new("Frame", Notif)
    IconBox.Size = UDim2.new(0,46,0,46)
    IconBox.Position = UDim2.new(0,12,0,26)
    IconBox.BackgroundColor3 = Color3.fromRGB(20,16,36)
    Instance.new("UICorner", IconBox).CornerRadius = UDim.new(0,12)
    local D1 = Instance.new("Frame", IconBox)
    D1.Size = UDim2.new(0,18,0,18)
    D1.Position = UDim2.new(0.5,-9,0,6)
    D1.BackgroundColor3 = col.light
    D1.Rotation = 45
    Instance.new("UICorner", D1).CornerRadius = UDim.new(0,3)
    local D2 = Instance.new("Frame", IconBox)
    D2.Size = UDim2.new(0,18,0,18)
    D2.Position = UDim2.new(0.5,-9,0,22)
    D2.BackgroundColor3 = col.dark
    D2.Rotation = 45
    Instance.new("UICorner", D2).CornerRadius = UDim.new(0,3)

    local TL = Instance.new("TextLabel", Notif)
    TL.Size = UDim2.new(1,-76,0,20)
    TL.Position = UDim2.new(0,68,0,24)
    TL.BackgroundTransparency = 1
    TL.Text = title
    TL.TextColor3 = Color3.fromRGB(255,255,255)
    TL.Font = Enum.Font.GothamBlack
    TL.TextSize = isMobile and 12 or 13
    TL.TextXAlignment = Enum.TextXAlignment.Left

    local ML = Instance.new("TextLabel", Notif)
    ML.Size = UDim2.new(1,-76,0,32)
    ML.Position = UDim2.new(0,68,0,42)
    ML.BackgroundTransparency = 1
    ML.Text = msg
    ML.TextColor3 = Color3.fromRGB(180,160,200)
    ML.Font = Enum.Font.Gotham
    ML.TextSize = isMobile and 9 or 10
    ML.TextXAlignment = Enum.TextXAlignment.Left
    ML.TextYAlignment = Enum.TextYAlignment.Top
    ML.TextWrapped = true

    local PBG = Instance.new("Frame", Notif)
    PBG.Size = UDim2.new(1,-20,0,3)
    PBG.Position = UDim2.new(0,10,0,80)
    PBG.BackgroundColor3 = Color3.fromRGB(30,25,45)
    PBG.BackgroundTransparency = 0.5
    Instance.new("UICorner", PBG).CornerRadius = UDim.new(0,2)
    local P = Instance.new("Frame", PBG)
    P.Size = UDim2.new(1,0,1,0)
    P.BackgroundColor3 = col.bg
    Instance.new("UICorner", P).CornerRadius = UDim.new(0,2)

    TweenService:Create(Notif, TweenInfo.new(0.6, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Position = UDim2.new(0.5, isMobile and -140 or -170, 0, 20)}):Play()
    TweenService:Create(P, TweenInfo.new(4, Enum.EasingStyle.Linear), {Size = UDim2.new(0,0,1,0)}):Play()
    task.delay(4, function()
        if Notif.Parent then
            TweenService:Create(Notif, TweenInfo.new(0.35, Enum.EasingStyle.Back, Enum.EasingDirection.In), {Position = UDim2.new(0.5, isMobile and -140 or -170, 0, -120), BackgroundTransparency = 1}):Play()
            task.wait(0.35)
            Notif:Destroy()
        end
    end)
end

-- ── MAIN FRAME ─────────────────────────────────────────────────────────
local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Size = UDim2.new(0, baseSize.X, 0, baseSize.Y)
Main.Position = isMobile and UDim2.new(0.5,-160,0.5,-240) or UDim2.new(0.5,-200,0.5,-260)
Main.BackgroundColor3 = Color3.fromRGB(10,10,16)
Main.BackgroundTransparency = 0.03
Main.BorderSizePixel = 0
Main.Active = true
Main.ClipsDescendants = true
Main.Parent = Gui
Main.ZIndex = 1
Instance.new("UICorner", Main).CornerRadius = UDim.new(0,20)
local MStroke = Instance.new("UIStroke", Main)
MStroke.Color = Color3.fromRGB(140,80,255)
MStroke.Thickness = 1.2
MStroke.Transparency = 0.5

-- ── HEADER ─────────────────────────────────────────────────────────────
local Header = Instance.new("Frame", Main)
Header.Size = UDim2.new(1,0,0, isMobile and 46 or 54)
Header.BackgroundTransparency = 1
Header.Active = true
Header.ZIndex = 5

local dragging, dragStart, startPos = false, nil, nil
Header.InputBegan:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = true dragStart = input.Position startPos = Main.Position end end)
UIS.InputChanged:Connect(function(input) if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then local d = input.Position - dragStart Main.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + d.X, startPos.Y.Scale, startPos.Y.Offset + d.Y) end end)
UIS.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end end)

local IconHolder = Instance.new("Frame", Header)
IconHolder.Size = UDim2.new(0, isMobile and 36 or 44, 0, isMobile and 36 or 44)
IconHolder.Position = UDim2.new(0,10,0,4)
IconHolder.BackgroundTransparency = 1
local Glow = Instance.new("Frame", IconHolder)
Glow.Size = UDim2.new(1,0,1,0)
Glow.BackgroundColor3 = Color3.fromRGB(140,80,255)
Glow.BackgroundTransparency = 0.78
Instance.new("UICorner", Glow).CornerRadius = UDim.new(0,14)
task.spawn(function()
    while Glow and Glow.Parent do
        pcall(function() TweenService:Create(Glow, TweenInfo.new(1.4, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {BackgroundTransparency = 0.92}):Play() end)
        task.wait(1.4)
        pcall(function() TweenService:Create(Glow, TweenInfo.new(1.4), {BackgroundTransparency = 0.68}):Play() end)
        task.wait(1.4)
    end
end)
local Icon = Instance.new("Frame", IconHolder)
Icon.Size = UDim2.new(0, isMobile and 30 or 36, 0, isMobile and 30 or 36)
Icon.Position = UDim2.new(0,3,0,3)
Icon.BackgroundColor3 = Color3.fromRGB(14,12,28)
Instance.new("UICorner", Icon).CornerRadius = UDim.new(0,12)
local DC = Instance.new("Frame", Icon)
DC.Size = UDim2.new(1,0,1,0)
DC.BackgroundTransparency = 1
local TF = Instance.new("Frame", DC)
TF.Size = UDim2.new(0, isMobile and 14 or 18, 0, isMobile and 14 or 18)
TF.Position = UDim2.new(0.5, isMobile and -7 or -9, 0, isMobile and 2 or 4)
TF.BackgroundColor3 = Color3.fromRGB(200,150,255)
TF.Rotation = 45
Instance.new("UICorner", TF).CornerRadius = UDim.new(0,3)
local BF = Instance.new("Frame", DC)
BF.Size = UDim2.new(0, isMobile and 14 or 18, 0, isMobile and 14 or 18)
BF.Position = UDim2.new(0.5, isMobile and -7 or -9, 0, isMobile and 12 or 16)
BF.BackgroundColor3 = Color3.fromRGB(80,40,160)
BF.Rotation = 45
Instance.new("UICorner", BF).CornerRadius = UDim.new(0,3)
task.spawn(function() while DC and DC.Parent do TweenService:Create(DC, TweenInfo.new(1.2, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {Rotation = 8}):Play() task.wait(1.2) TweenService:Create(DC, TweenInfo.new(1.2), {Rotation = -8}):Play() task.wait(1.2) end end)

local TitleH = Instance.new("Frame", Header)
TitleH.Size = UDim2.new(0,220,0,34)
TitleH.Position = UDim2.new(0, isMobile and 52 or 64,0,6)
TitleH.BackgroundTransparency = 1
local TitleM = Instance.new("TextLabel", TitleH)
TitleM.Size = UDim2.new(1,0,0, isMobile and 16 or 18)
TitleM.BackgroundTransparency = 1
TitleM.Text = "BEWITCHED"
TitleM.Font = Enum.Font.GothamBlack
TitleM.TextSize = isMobile and 13 or 16
TitleM.TextXAlignment = Enum.TextXAlignment.Left
TitleM.TextColor3 = Color3.fromRGB(255,255,255)
local TS = Instance.new("UIStroke", TitleM); TS.Color = Color3.fromRGB(180,110,255); TS.Thickness = 1; TS.Transparency = 0.25
local TG = Instance.new("UIGradient", TitleM)
TG.Color = ColorSequence.new(Color3.fromRGB(255,255,255), Color3.fromRGB(170,90,255), Color3.fromRGB(100,30,190))
TG.Rotation = 90
task.spawn(function() while TG and TG.Parent do TweenService:Create(TG, TweenInfo.new(2.8, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {Offset = Vector2.new(0.6,0)}):Play() task.wait(2.8) TweenService:Create(TG, TweenInfo.new(2.8), {Offset = Vector2.new(-0.6,0)}):Play() task.wait(2.8) end end)

local Sub = Instance.new("TextLabel", TitleH)
Sub.Size = UDim2.new(1,0,0,10)
Sub.Position = UDim2.new(0,0,0, isMobile and 16 or 20)
Sub.BackgroundTransparency = 1
Sub.Text = "BY BILLIERXZ"
Sub.TextColor3 = Color3.fromRGB(180,140,240)
Sub.Font = Enum.Font.GothamBlack
Sub.TextSize = isMobile and 7 or 8
Sub.TextXAlignment = Enum.TextXAlignment.Left

local MinBtn = Instance.new("TextButton", Header)
MinBtn.Size = UDim2.new(0,22,0,22)
MinBtn.Position = UDim2.new(1,-52,0,12)
MinBtn.BackgroundColor3 = Color3.fromRGB(28,22,40)
MinBtn.Text = "-"
MinBtn.TextColor3 = Color3.fromRGB(180,160,210)
MinBtn.Font = Enum.Font.GothamBold
MinBtn.TextSize = 14
Instance.new("UICorner", MinBtn).CornerRadius = UDim.new(0,6)

local Close = Instance.new("TextButton", Header)
Close.Size = UDim2.new(0,22,0,22)
Close.Position = UDim2.new(1,-24,0,12)
Close.BackgroundColor3 = Color3.fromRGB(45,22,30)
Close.Text = "X"
Close.TextColor3 = Color3.fromRGB(200,120,130)
Close.Font = Enum.Font.GothamBold
Close.TextSize = 9
Instance.new("UICorner", Close).CornerRadius = UDim.new(0,6)
Close.MouseButton1Click:Connect(function() playClick() pcall(function() ESPFolder:ClearAllChildren() end) Gui:Destroy() end)

-- ── TAB BAR ────────────────────────────────────────────────────────────
local TabBar = Instance.new("Frame", Main)
TabBar.Size = UDim2.new(1,-20,0, isMobile and 32 or 38)
TabBar.Position = UDim2.new(0,10,0, isMobile and 46 or 54)
TabBar.BackgroundColor3 = Color3.fromRGB(22,20,32)
TabBar.BackgroundTransparency = 0.15
Instance.new("UICorner", TabBar).CornerRadius = UDim.new(0,12)

local tabNames = {"MAIN","SKILL","TRACKERS","ESP","STOMP","OTHER"}
local tabBtns = {}
local tabWidth = 1/#tabNames

local Indicator = Instance.new("Frame", TabBar)
Indicator.Size = UDim2.new(tabWidth,-4,0, isMobile and 24 or 30)
Indicator.Position = UDim2.new(0,4,0,4)
Indicator.BackgroundColor3 = Color3.fromRGB(140,80,255)
Indicator.BackgroundTransparency = 0.12
Instance.new("UICorner", Indicator).CornerRadius = UDim.new(0,10)

for i, name in ipairs(tabNames) do
    local b = Instance.new("TextButton", TabBar)
    b.Size = UDim2.new(tabWidth,-4,0, isMobile and 24 or 30)
    b.Position = UDim2.new((i-1)*tabWidth,2,0,4)
    b.BackgroundTransparency = 1
    b.Text = name
    b.TextColor3 = i==1 and Color3.new(1,1,1) or Color3.fromRGB(100,100,110)
    b.Font = Enum.Font.GothamBlack
    b.TextSize = isMobile and 7 or 8
    b.ZIndex = 3
    tabBtns[name] = b
end

-- ── CONTENT AREAS ──────────────────────────────────────────────────────
local ContentHolder = Instance.new("Frame", Main)
ContentHolder.Size = UDim2.new(1,-20,0, isMobile and 375 or 405)
ContentHolder.Position = UDim2.new(0,10,0, isMobile and 84 or 96)
ContentHolder.BackgroundTransparency = 1
ContentHolder.ClipsDescendants = true

local function makeScrollTab()
    local sf = Instance.new("ScrollingFrame", ContentHolder)
    sf.Size = UDim2.new(1,0,1,0)
    sf.BackgroundTransparency = 1
    sf.Visible = false
    sf.ScrollBarThickness = 4
    sf.ScrollBarImageColor3 = Color3.fromRGB(140,80,255)
    sf.CanvasSize = UDim2.new(0,0,0, 1200)
    sf.BorderSizePixel = 0
    local list = Instance.new("Frame", sf)
    list.Size = UDim2.new(1,0,0,1200)
    list.BackgroundTransparency = 1
    local lay = Instance.new("UIListLayout", list)
    lay.Padding = UDim.new(0,8)
    lay.SortOrder = Enum.SortOrder.LayoutOrder
    lay:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        sf.CanvasSize = UDim2.new(0,0,0, lay.AbsoluteContentSize.Y + 20)
    end)
    return sf, list
end

local MainTab, MainList = makeScrollTab()
local SkillTab, SkillList = makeScrollTab()
local TrackersTab, TrackersList = makeScrollTab()
local EspTab, EspList = makeScrollTab()
local StompTab, StompList = makeScrollTab()
local OtherTab, OtherList = makeScrollTab()
MainTab.Visible = true

-- ── BgImage (fondo del área de contenido, opción B) ───────────────────
local BgImage = Instance.new("ImageLabel", ContentHolder)
BgImage.Name = "BgImage"
BgImage.Size = UDim2.new(1, 0, 1, 0)
BgImage.Position = UDim2.new(0, 0, 0, 0)
BgImage.BackgroundTransparency = 1
BgImage.Image = ""
BgImage.ScaleType = Enum.ScaleType.Crop
BgImage.ZIndex = 0
BgImage.ClipsDescendants = true
Instance.new("UICorner", BgImage).CornerRadius = UDim.new(0, 12)
local BgImageStroke = Instance.new("UIStroke", BgImage)
BgImageStroke.Color = Color3.fromRGB(200, 160, 255)
BgImageStroke.Thickness = 2
BgImageStroke.Transparency = 1
BgImageStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

-- ── PARTÍCULAS FONDO (contenidas en Main) ─────────────────────────────
local PartHolder = Instance.new("Frame", Main)
PartHolder.Name = "PartHolder"
PartHolder.Size = UDim2.new(1,0,1,0)
PartHolder.BackgroundTransparency = 1
PartHolder.ClipsDescendants = true
PartHolder.ZIndex = 1
Instance.new("UICorner", PartHolder).CornerRadius = UDim.new(0,20)

local particleFrames = {}
for i=1, isMobile and 14 or 22 do
    local p = Instance.new("Frame", PartHolder)
    p.Size = UDim2.new(0, math.random(2,4), 0, math.random(2,4))
    p.Position = UDim2.new(math.random(),0,math.random(),0)
    p.BackgroundColor3 = getPartColor()
    p.BackgroundTransparency = 0.2
    p.BorderSizePixel = 0
    p.ZIndex = 1
    Instance.new("UICorner", p).CornerRadius = UDim.new(0,10)
    table.insert(particleFrames, p)
    task.spawn(function()
        while p and p.Parent do
            pcall(function() TweenService:Create(p, TweenInfo.new(math.random(5,8), Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {Position = UDim2.new(math.random(),0,math.random(),0)}):Play() end)
            task.wait(math.random(5,8))
        end
    end)
end

local function updateParticles()
    local visible = getgenv().AIOConfig.partOn
    if visible and (not getgenv().AIOConfig.partWithBg) and BgImage.Image ~= "" then
        visible = false
    end
    PartHolder.Visible = visible
    local col = getPartColor()
    for _, p in ipairs(particleFrames) do
        p.BackgroundColor3 = col
    end
end
updateParticles()

-- ── TRAILS SOLO DENTRO DE MAIN ─────────────────────────────────────────
local lastTrail = 0
local function isInsideMain(x, y)
    local mp = Main.AbsolutePosition
    local ms = Main.AbsoluteSize
    return x >= mp.X and x <= mp.X + ms.X and y >= mp.Y and y <= mp.Y + ms.Y
end

Main.MouseMoved:Connect(function(x, y)
    if tick() - lastTrail > 0.03 then
        spawnTrail(x, y); lastTrail = tick()
    end
end)
UIS.TouchMoved:Connect(function(touch, gp)
    if gp then return end
    local pos = touch.Position
    if isInsideMain(pos.X, pos.Y) then
        if tick() - lastTrail > 0.02 then spawnTrail(pos.X, pos.Y) lastTrail = tick() end
    end
end)

-- ── HELPERS UI ─────────────────────────────────────────────────────────
local activeModules = {}

local function makeSection(parent, titleText)
    local S = Instance.new("Frame", parent)
    S.Size = UDim2.new(1,0,0,22)
    S.BackgroundTransparency = 1
    local L = Instance.new("TextLabel", S)
    L.Size = UDim2.new(1,0,1,0)
    L.BackgroundTransparency = 1
    L.Text = titleText
    L.TextColor3 = Color3.fromRGB(140,80,255)
    L.Font = Enum.Font.GothamBlack
    L.TextSize = isMobile and 9 or 10
    L.TextXAlignment = Enum.TextXAlignment.Left
    L.Position = UDim2.new(0,4,0,0)
    return S
end

local function createModule(parent, name, desc, key, isOn, callback, silent)
    local state = isOn
    activeModules[key] = state
    local F = Instance.new("Frame", parent)
    F.Size = UDim2.new(1,-16,0, 54)
    F.Position = UDim2.new(0,8,0,0)
    F.BackgroundColor3 = state and Color3.fromRGB(34,26,54) or Color3.fromRGB(20,20,32)
    Instance.new("UICorner", F).CornerRadius = UDim.new(0,14)
    local Stroke = Instance.new("UIStroke", F)
    Stroke.Color = state and Color3.fromRGB(140,80,255) or Color3.fromRGB(40,35,60)
    Stroke.Thickness = 1
    Stroke.Transparency = state and 0.1 or 0.4
    local Grad = Instance.new("UIGradient", F)
    Grad.Color = ColorSequence.new(Color3.fromRGB(28,24,44), Color3.fromRGB(18,16,28))
    Grad.Rotation = 90

    local T1 = Instance.new("TextLabel", F)
    T1.Size = UDim2.new(1,-100,0,16)
    T1.Position = UDim2.new(0,14,0,10)
    T1.BackgroundTransparency = 1
    T1.Text = name
    T1.TextColor3 = Color3.new(1,1,1)
    T1.Font = Enum.Font.GothamBlack
    T1.TextSize = isMobile and 10 or 11
    T1.TextXAlignment = Enum.TextXAlignment.Left

    local T2 = Instance.new("TextLabel", F)
    T2.Size = UDim2.new(1,-100,0,12)
    T2.Position = UDim2.new(0,14,0,28)
    T2.BackgroundTransparency = 1
    T2.Text = desc
    T2.TextColor3 = Color3.fromRGB(110,100,130)
    T2.Font = Enum.Font.Gotham
    T2.TextSize = isMobile and 8 or 9
    T2.TextXAlignment = Enum.TextXAlignment.Left

    local TBG = Instance.new("Frame", F)
    TBG.Size = UDim2.new(0,44,0,24)
    TBG.Position = UDim2.new(1,-54,0,15)
    TBG.BackgroundColor3 = state and Color3.fromRGB(140,80,255) or Color3.fromRGB(34,34,48)
    Instance.new("UICorner", TBG).CornerRadius = UDim.new(0,12)
    local Dot = Instance.new("Frame", TBG)
    Dot.Size = UDim2.new(0,20,0,20)
    Dot.Position = state and UDim2.new(1,-22,0,2) or UDim2.new(0,2,0,2)
    Dot.BackgroundColor3 = Color3.fromRGB(10,10,16)
    Instance.new("UICorner", Dot).CornerRadius = UDim.new(0,10)

    local btn = Instance.new("TextButton", F)
    btn.Size = UDim2.new(1,0,1,0)
    btn.BackgroundTransparency = 1
    btn.Text = ""
    btn.MouseButton1Click:Connect(function()
        playClick()
        state = not state
        activeModules[key] = state
        TweenService:Create(F, TweenInfo.new(0.3, Enum.EasingStyle.Back), {BackgroundColor3 = state and Color3.fromRGB(34,26,54) or Color3.fromRGB(20,20,32)}):Play()
        TweenService:Create(Stroke, TweenInfo.new(0.3), {Color = state and Color3.fromRGB(140,80,255) or Color3.fromRGB(40,35,60), Transparency = state and 0.1 or 0.4}):Play()
        TweenService:Create(TBG, TweenInfo.new(0.3, Enum.EasingStyle.Back), {BackgroundColor3 = state and Color3.fromRGB(140,80,255) or Color3.fromRGB(34,34,48)}):Play()
        TweenService:Create(Dot, TweenInfo.new(0.3, Enum.EasingStyle.Back), {Position = state and UDim2.new(1,-22,0,2) or UDim2.new(0,2,0,2)}):Play()
        for i=1,8 do spawnTrail(F.AbsolutePosition.X + F.AbsoluteSize.X/2 + math.random(-40,40), F.AbsolutePosition.Y + F.AbsoluteSize.Y/2 + math.random(-10,10), getPartColor()) end
        if callback then callback(state) end
    end)
    return F
end

-- ── FUNCIONES BG/WHEEL (integradas) ───────────────────────────────────
local userId = tostring(LP.UserId)
local BG_LIST_FILE = "bewitched_bg_list_"..userId..".json"
local BG_CURRENT_FILE = "bewitched_bg_current_"..userId..".txt"
local WHEEL_LIST_FILE = "bewitched_wheel_list_"..userId..".json"
local WHEEL_CURRENT_FILE = "bewitched_wheel_current_"..userId..".txt"
local BG_OPACITY_FILE = "bewitched_bg_opacity_"..userId..".txt"
local WHEEL_OPACITY_FILE = "bewitched_wheel_opacity_"..userId..".txt"
local PART_SETTINGS_FILE = "bewitched_part_"..userId..".json"

local bgList = {}
local wheelList = {}

pcall(function()
    if isfile and isfile(BG_LIST_FILE) then
        local d = HttpService:JSONDecode(readfile(BG_LIST_FILE))
        if d and type(d) == "table" then bgList = d end
    end
end)
pcall(function()
    if isfile and isfile(WHEEL_LIST_FILE) then
        local d = HttpService:JSONDecode(readfile(WHEEL_LIST_FILE))
        if d and type(d) == "table" then wheelList = d end
    end
end)
pcall(function()
    if isfile and isfile(PART_SETTINGS_FILE) then
        local d = HttpService:JSONDecode(readfile(PART_SETTINGS_FILE))
        if d and type(d) == "table" then
            for k, v in pairs(d) do getgenv().AIOConfig[k] = v end
        end
    end
end)
pcall(function()
    if isfile and isfile(BG_OPACITY_FILE) then
        getgenv().AIOConfig.bgOpacity = tonumber(readfile(BG_OPACITY_FILE)) or 0.15
    end
end)
pcall(function()
    if isfile and isfile(WHEEL_OPACITY_FILE) then
        getgenv().AIOConfig.wheelOpacity = tonumber(readfile(WHEEL_OPACITY_FILE)) or 0
    end
end)

local function saveBgList() pcall(function() if writefile then writefile(BG_LIST_FILE, HttpService:JSONEncode(bgList)) end end) end
local function saveWheelList() pcall(function() if writefile then writefile(WHEEL_LIST_FILE, HttpService:JSONEncode(wheelList)) end end) end
local function saveBgCurrent(a) pcall(function() if writefile then writefile(BG_CURRENT_FILE, a) end end) end
local function saveWheelCurrent(a) pcall(function() if writefile then writefile(WHEEL_CURRENT_FILE, a) end end) end
local function savePartSettings()
    pcall(function()
        if writefile then
            writefile(PART_SETTINGS_FILE, HttpService:JSONEncode({
                partOn = getgenv().AIOConfig.partOn,
                partWithBg = getgenv().AIOConfig.partWithBg,
                partColor = getgenv().AIOConfig.partColor,
            }))
        end
    end)
end
local function saveBgOpacity()
    pcall(function() if writefile then writefile(BG_OPACITY_FILE, tostring(getgenv().AIOConfig.bgOpacity)) end end)
end
local function saveWheelOpacity()
    pcall(function() if writefile then writefile(WHEEL_OPACITY_FILE, tostring(getgenv().AIOConfig.wheelOpacity)) end end)
end

local function downloadAndGetAsset(url)
    if not url or url == "" then return "" end
    url = url:gsub("%s+",""):gsub("\n",""):gsub("\r","")
    if url:find("postimg%.cc") and not url:find("i%.postimg%.cc") then
        local req = (syn and syn.request) or request or http_request
        if req then
            local ok,res = pcall(function() return req({Url=url,Method="GET"}) end)
            if ok and res and res.Body then
                local d = res.Body:match('og:image["\']?%s*content%s*=%s*["\'](https?://i%.postimg%.cc/[^"\']+)')
                if not d then d = res.Body:match('content%s*=%s*["\'](https?://i%.postimg%.cc/[^"\']+)["\']%s*property%s*=%s*["\']og:image') end
                if not d then d = res.Body:match('(https?://i%.postimg%.cc/[^"\'%s<>]+)') end
                if d then url = d end
            end
        end
    end
    if url:find("rbxasset") then return url end
    if tonumber(url) then return "rbxassetid://"..url end
    if not url:find("http") then return url end
    if not writefile or not getcustomasset then return url end
    local fileName = "bewitched_"..url:gsub("%W",""):sub(1,30)..".png"
    local req = (syn and syn.request) or request or http_request
    if req and (not isfile or not isfile(fileName)) then
        local ok = false
        pcall(function()
            local res = req({Url=url, Method="GET"})
            if res and res.Body and #res.Body > 100 then
                writefile(fileName, res.Body)
                ok = true
            end
        end)
        if not ok then return url end
    end
    if getcustomasset and isfile and isfile(fileName) then
        local a = nil
        pcall(function() a = getcustomasset(fileName) end)
        if a and a ~= "" then return a end
    end
    return url
end

-- ── WHEEL: poner detrás del wheel del juego ────────────────────────────
local createdBack = nil
local configW = {size=80, posX=0, posY=0, transp=0, round=100}
local WHEEL_CONFIG_FILE = "bewitched_wheel_config_"..userId..".json"
pcall(function()
    if isfile and isfile(WHEEL_CONFIG_FILE) then
        local d = HttpService:JSONDecode(readfile(WHEEL_CONFIG_FILE))
        if d and type(d) == "table" then
            for k, v in pairs(d) do configW[k] = v end
        end
    end
end)
local function saveWheelConfig()
    pcall(function() if writefile then writefile(WHEEL_CONFIG_FILE, HttpService:JSONEncode(configW)) end end)
end

local function findWheel()
    local pg = LP:FindFirstChild("PlayerGui")
    if not pg then return nil end
    local itf = pg:FindFirstChild("Interface")
    if itf then
        local w = itf:FindFirstChild("Wheel")
        if w then return w end
    end
    for _, g in ipairs(pg:GetDescendants()) do
        if g.Name == "Wheel" and g:IsA("GuiObject") then return g end
    end
    return nil
end

local function putWheelBehind(asset)
    if not asset or asset == "" then return end
    local Wheel = findWheel()
    if not Wheel then saveWheelCurrent(asset) return end
    if createdBack and createdBack.Parent then createdBack:Destroy() end
    local b = Instance.new("ImageLabel")
    b.Name = "MyCustomBack"
    b.Size = UDim2.new(configW.size/100, 0, configW.size/100, 0)
    b.Position = UDim2.new((1-configW.size/100)/2 + configW.posX/500, 0, (1-configW.size/100)/2 + configW.posY/500, 0)
    b.BackgroundTransparency = 1
    b.Image = asset
    b.ScaleType = Enum.ScaleType.Crop
    b.ZIndex = 0
    b.ImageTransparency = getgenv().AIOConfig.wheelOpacity
    b.ClipsDescendants = true
    Instance.new("UICorner", b).CornerRadius = UDim.new(configW.round/100, 0)
    b.Parent = Wheel
    createdBack = b
    for _, ch in ipairs(Wheel:GetChildren()) do
        if ch:IsA("ImageLabel") and ch ~= b then ch.ZIndex = 2 end
        if ch:IsA("Frame") then ch.ZIndex = 3 end
    end
    saveWheelCurrent(asset)
end

-- ── SLIDER helper ──────────────────────────────────────────────────────
local function createSlider(parent, label, initialValue, minVal, maxVal, callback)
    local holder = Instance.new("Frame", parent)
    holder.Size = UDim2.new(1,-16,0, 40)
    holder.Position = UDim2.new(0,8,0,0)
    holder.BackgroundColor3 = Color3.fromRGB(20,20,32)
    holder.BackgroundTransparency = 0.2
    Instance.new("UICorner", holder).CornerRadius = UDim.new(0,10)
    local lbl = Instance.new("TextLabel", holder)
    lbl.Size = UDim2.new(0, 110, 0, 20)
    lbl.Position = UDim2.new(0, 8, 0, 4)
    lbl.BackgroundTransparency = 1
    lbl.Text = label
    lbl.TextColor3 = Color3.fromRGB(200,160,255)
    lbl.Font = Enum.Font.GothamBold
    lbl.TextSize = isMobile and 9 or 10
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    local valLbl = Instance.new("TextLabel", holder)
    valLbl.Size = UDim2.new(0, 50, 0, 20)
    valLbl.Position = UDim2.new(1, -60, 0, 4)
    valLbl.BackgroundTransparency = 1
    valLbl.Text = string.format("%.0f%%", initialValue * 100)
    valLbl.TextColor3 = Color3.fromRGB(0,255,150)
    valLbl.Font = Enum.Font.GothamBlack
    valLbl.TextSize = isMobile and 9 or 10
    valLbl.TextXAlignment = Enum.TextXAlignment.Right
    local barBg = Instance.new("Frame", holder)
    barBg.Size = UDim2.new(1,-20,0, 8)
    barBg.Position = UDim2.new(0, 10, 0, 28)
    barBg.BackgroundColor3 = Color3.fromRGB(30,28,42)
    Instance.new("UICorner", barBg).CornerRadius = UDim.new(0,4)
    local barFill = Instance.new("Frame", barBg)
    barFill.Size = UDim2.new(initialValue, 0, 1, 0)
    barFill.BackgroundColor3 = Color3.fromRGB(140,80,255)
    Instance.new("UICorner", barFill).CornerRadius = UDim.new(0,4)
    local dragger = Instance.new("TextButton", barBg)
    dragger.Size = UDim2.new(0, 14, 0, 14)
    dragger.Position = UDim2.new(initialValue, -7, 0.5, -7)
    dragger.BackgroundColor3 = Color3.fromRGB(200,160,255)
    dragger.Text = ""
    dragger.AutoButtonColor = false
    Instance.new("UICorner", dragger).CornerRadius = UDim.new(1,0)
    local dragging = false
    dragger.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
        end
    end)
    UIS.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    UIS.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local rel = (input.Position.X - barBg.AbsolutePosition.X) / barBg.AbsoluteSize.X
            rel = math.clamp(rel, 0, 1)
            local val = minVal + (maxVal - minVal) * rel
            barFill.Size = UDim2.new(rel, 0, 1, 0)
            dragger.Position = UDim2.new(rel, -7, 0.5, -7)
            valLbl.Text = string.format("%.0f%%", val * 100)
            if callback then callback(val) end
        end
    end)
    barBg.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            local rel = (input.Position.X - barBg.AbsolutePosition.X) / barBg.AbsoluteSize.X
            rel = math.clamp(rel, 0, 1)
            local val = minVal + (maxVal - minVal) * rel
            barFill.Size = UDim2.new(rel, 0, 1, 0)
            dragger.Position = UDim2.new(rel, -7, 0.5, -7)
            valLbl.Text = string.format("%.0f%%", val * 100)
            if callback then callback(val) end
        end
    end)
    return holder
end

-- ═══════════════════════════════════════════════════════════════════════
-- TAB 1: MAIN (BG + Wheel + Opciones de partículas, sin bienvenida)
-- ═══════════════════════════════════════════════════════════════════════

-- ── SECCIÓN BACKGROUND ─────────────────────────────────────────────────
makeSection(MainList, "🖼️ BACKGROUND")

local bgSection = Instance.new("Frame", MainList)
bgSection.Size = UDim2.new(1,-16,0, 70)
bgSection.Position = UDim2.new(0,8,0,0)
bgSection.BackgroundTransparency = 1

local bgInput = Instance.new("TextBox", bgSection)
bgInput.Size = UDim2.new(0.72, 0, 0, 32)
bgInput.Position = UDim2.new(0, 0, 0, 0)
bgInput.BackgroundColor3 = Color3.fromRGB(30, 26, 40)
bgInput.BackgroundTransparency = 0.15
bgInput.Text = ""
bgInput.PlaceholderText = "https://i.postimg.cc/..."
bgInput.PlaceholderColor3 = Color3.fromRGB(120, 110, 150)
bgInput.TextColor3 = Color3.new(1, 1, 1)
bgInput.Font = Enum.Font.Gotham
bgInput.TextSize = isMobile and 9 or 10
bgInput.ClearTextOnFocus = false
Instance.new("UICorner", bgInput).CornerRadius = UDim.new(0, 8)
local bgInputStroke = Instance.new("UIStroke", bgInput)
bgInputStroke.Color = Color3.fromRGB(140, 80, 255)
bgInputStroke.Thickness = 1
bgInputStroke.Transparency = 0.6

local bgApplyBtn = Instance.new("TextButton", bgSection)
bgApplyBtn.Size = UDim2.new(0.24, 0, 0, 32)
bgApplyBtn.Position = UDim2.new(0.74, 0, 0, 0)
bgApplyBtn.BackgroundColor3 = Color3.fromRGB(140, 80, 255)
bgApplyBtn.BackgroundTransparency = 0.15
bgApplyBtn.Text = "APLICAR"
bgApplyBtn.TextColor3 = Color3.new(1, 1, 1)
bgApplyBtn.Font = Enum.Font.GothamBlack
bgApplyBtn.TextSize = isMobile and 9 or 10
Instance.new("UICorner", bgApplyBtn).CornerRadius = UDim.new(0, 8)

local bgListFrame = Instance.new("ScrollingFrame", bgSection)
bgListFrame.Size = UDim2.new(1, 0, 0, 0)
bgListFrame.Position = UDim2.new(0, 0, 0, 38)
bgListFrame.BackgroundTransparency = 1
bgListFrame.ScrollBarThickness = 3
bgListFrame.ScrollBarImageColor3 = Color3.fromRGB(140, 80, 255)
bgListFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
local bgLayout = Instance.new("UIListLayout", bgListFrame)
bgLayout.Padding = UDim.new(0, 6)
bgLayout.SortOrder = Enum.SortOrder.LayoutOrder

local function animateRow(row)
    row.BackgroundTransparency = 1
    row.Position = UDim2.new(0, 0, 0, -20)
    TweenService:Create(row, TweenInfo.new(0.35, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {
        BackgroundTransparency = 0.15,
        Position = UDim2.new(0, 0, 0, 0)
    }):Play()
end

local function setBgApplied(asset)
    if asset and asset ~= "" then
        BgImage.Image = asset
        BgImage.ImageTransparency = getgenv().AIOConfig.bgOpacity
        BgImageStroke.Transparency = 0.5
        updateParticles()
    else
        BgImage.Image = ""
        BgImageStroke.Transparency = 1
        updateParticles()
    end
end

local function applyBgWithAnim(asset, fromButton)
    if fromButton then
        local origSize = bgApplyBtn.Size
        TweenService:Create(bgApplyBtn, TweenInfo.new(0.15, Enum.EasingStyle.Back), {
            Size = UDim2.new(0.26, 0, 0, 34),
        }):Play()
        task.delay(0.15, function()
            TweenService:Create(bgApplyBtn, TweenInfo.new(0.2, Enum.EasingStyle.Back), {
                Size = origSize,
            }):Play()
        end)
    end
    setBgApplied(asset)
end

local function refreshBgList()
    for _, c in ipairs(bgListFrame:GetChildren()) do
        if c:IsA("Frame") then c:Destroy() end
    end
    local anchored = {}
    local normal = {}
    for i, item in ipairs(bgList) do
        if item.anchored then table.insert(anchored, {item=item, idx=i}) else table.insert(normal, {item=item, idx=i}) end
    end
    local ordered = {}
    for _, e in ipairs(anchored) do table.insert(ordered, e) end
    for _, e in ipairs(normal) do table.insert(ordered, e) end

    for order, entry in ipairs(ordered) do
        local i = entry.idx
        local item = entry.item
        local row = Instance.new("Frame", bgListFrame)
        row.Size = UDim2.new(1, -8, 0, 50)
        row.BackgroundColor3 = item.anchored and Color3.fromRGB(40, 32, 55) or Color3.fromRGB(28, 24, 38)
        row.LayoutOrder = order
        row.Name = "BGItem_"..i
        Instance.new("UICorner", row).CornerRadius = UDim.new(0, 10)

        local thumb = Instance.new("ImageLabel", row)
        thumb.Size = UDim2.new(0, 42, 0, 42)
        thumb.Position = UDim2.new(0, 4, 0, 4)
        thumb.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
        thumb.BackgroundTransparency = 0.3
        thumb.ScaleType = Enum.ScaleType.Crop
        Instance.new("UICorner", thumb).CornerRadius = UDim.new(0, 8)
        task.spawn(function()
            local a = downloadAndGetAsset(item.url)
            if a and a ~= "" then thumb.Image = a end
        end)

        local nameBox = Instance.new("TextBox", row)
        nameBox.Size = UDim2.new(1, -180, 0, 26)
        nameBox.Position = UDim2.new(0, 52, 0, 12)
        nameBox.BackgroundColor3 = Color3.fromRGB(20, 18, 28)
        nameBox.BackgroundTransparency = 0.3
        nameBox.Text = item.name or ("Foto "..i)
        nameBox.TextColor3 = Color3.fromRGB(255, 255, 255)
        nameBox.Font = Enum.Font.GothamBold
        nameBox.TextSize = isMobile and 9 or 10
        nameBox.TextXAlignment = Enum.TextXAlignment.Left
        nameBox.ClearTextOnFocus = false
        nameBox.ZIndex = 2
        Instance.new("UICorner", nameBox).CornerRadius = UDim.new(0, 6)
        local nameStroke = Instance.new("UIStroke", nameBox)
        nameStroke.Color = Color3.fromRGB(60, 50, 80)
        nameStroke.Thickness = 1
        nameStroke.Transparency = 0.5
        nameBox.Focused:Connect(function()
            nameStroke.Color = Color3.fromRGB(200, 160, 255)
            nameStroke.Transparency = 0
        end)
        nameBox.FocusLost:Connect(function()
            nameStroke.Color = Color3.fromRGB(60, 50, 80)
            nameStroke.Transparency = 0.5
            if nameBox.Text ~= "" then
                bgList[i].name = nameBox.Text
                saveBgList()
            end
        end)

        local anchorBtn = Instance.new("TextButton", row)
        anchorBtn.Size = UDim2.new(0, 30, 0, 30)
        anchorBtn.Position = UDim2.new(1, -98, 0, 10)
        anchorBtn.BackgroundColor3 = item.anchored and Color3.fromRGB(255, 180, 40) or Color3.fromRGB(50, 45, 65)
        anchorBtn.Text = item.anchored and "📌" or "📍"
        anchorBtn.TextSize = 14
        anchorBtn.Font = Enum.Font.GothamBlack
        Instance.new("UICorner", anchorBtn).CornerRadius = UDim.new(0, 8)
        anchorBtn.MouseButton1Click:Connect(function()
            playClick()
            bgList[i].anchored = not bgList[i].anchored
            saveBgList()
            refreshBgList()
        end)

        local applyBtn = Instance.new("TextButton", row)
        applyBtn.Size = UDim2.new(0, 30, 0, 30)
        applyBtn.Position = UDim2.new(1, -64, 0, 10)
        applyBtn.BackgroundColor3 = Color3.fromRGB(0, 160, 100)
        applyBtn.Text = "✓"
        applyBtn.TextColor3 = Color3.new(1, 1, 1)
        applyBtn.Font = Enum.Font.GothamBlack
        applyBtn.TextSize = 14
        Instance.new("UICorner", applyBtn).CornerRadius = UDim.new(0, 8)
        applyBtn.MouseButton1Click:Connect(function()
            playClick()
            local asset = downloadAndGetAsset(item.url)
            applyBgWithAnim(asset, false)
            saveBgCurrent(asset)
        end)

        local delBtn = Instance.new("TextButton", row)
        delBtn.Size = UDim2.new(0, 30, 0, 30)
        delBtn.Position = UDim2.new(1, -30, 0, 10)
        delBtn.BackgroundColor3 = Color3.fromRGB(200, 40, 60)
        delBtn.Text = "X"
        delBtn.TextColor3 = Color3.new(1, 1, 1)
        delBtn.Font = Enum.Font.GothamBlack
        delBtn.TextSize = 12
        Instance.new("UICorner", delBtn).CornerRadius = UDim.new(0, 8)
        delBtn.MouseButton1Click:Connect(function()
            playClick()
            table.remove(bgList, i)
            saveBgList()
            refreshBgList()
        end)

        animateRow(row)
    end
    task.wait(0.1)
    bgListFrame.Size = UDim2.new(1, 0, 0, math.min(bgLayout.AbsoluteContentSize.Y, 250))
    bgSection.Size = UDim2.new(1,-16,0, 38 + bgListFrame.Size.Y.Offset + 8)
end

bgApplyBtn.MouseButton1Click:Connect(function()
    playClick()
    local url = bgInput.Text:gsub("%s+", "")
    if url == "" then return end
    bgInput.Text = ""
    bgInput.PlaceholderText = "⏳ Descargando..."
    task.spawn(function()
        local asset = downloadAndGetAsset(url)
        if asset:find("^http") and not asset:find("rbxasset") then
            bgInput.PlaceholderText = "❌ no se pudo descargar"
            task.wait(2.5)
            bgInput.PlaceholderText = "https://i.postimg.cc/..."
            return
        end
        table.insert(bgList, {url = url, name = "Foto "..(#bgList + 1), anchored = false})
        saveBgList()
        refreshBgList()
        applyBgWithAnim(asset, true)
        saveBgCurrent(asset)
        bgInput.PlaceholderText = "https://i.postimg.cc/..."
    end)
end)

-- Slider opacidad BG
local bgOpacitySlider = createSlider(MainList, "OPACIDAD BG", getgenv().AIOConfig.bgOpacity, 0, 0.9, function(v)
    getgenv().AIOConfig.bgOpacity = v
    if BgImage.Image ~= "" then BgImage.ImageTransparency = v end
    saveBgOpacity()
end)

pcall(function()
    if isfile and isfile(BG_CURRENT_FILE) then
        local a = readfile(BG_CURRENT_FILE)
        if a and a ~= "" then
            BgImage.Image = a
            BgImage.ImageTransparency = getgenv().AIOConfig.bgOpacity
            BgImageStroke.Transparency = 0.5
        end
    end
end)
refreshBgList()
updateParticles()

-- ── DIVISIÓN ───────────────────────────────────────────────────────────
local divider = Instance.new("Frame", MainList)
divider.Size = UDim2.new(1, -16, 0, 2)
divider.Position = UDim2.new(0,8,0,0)
divider.BackgroundColor3 = Color3.fromRGB(140, 80, 255)
divider.BackgroundTransparency = 0.5
divider.BorderSizePixel = 0
Instance.new("UICorner", divider).CornerRadius = UDim.new(0, 1)

-- ── SECCIÓN WHEEL ──────────────────────────────────────────────────────
makeSection(MainList, "🎡 WHEEL")

local wheelSection = Instance.new("Frame", MainList)
wheelSection.Size = UDim2.new(1,-16,0, 70)
wheelSection.Position = UDim2.new(0,8,0,0)
wheelSection.BackgroundTransparency = 1

local wheelInput = Instance.new("TextBox", wheelSection)
wheelInput.Size = UDim2.new(0.72, 0, 0, 32)
wheelInput.Position = UDim2.new(0, 0, 0, 0)
wheelInput.BackgroundColor3 = Color3.fromRGB(30, 26, 40)
wheelInput.BackgroundTransparency = 0.15
wheelInput.Text = ""
wheelInput.PlaceholderText = "https://i.postimg.cc/..."
wheelInput.PlaceholderColor3 = Color3.fromRGB(120, 110, 150)
wheelInput.TextColor3 = Color3.new(1, 1, 1)
wheelInput.Font = Enum.Font.Gotham
wheelInput.TextSize = isMobile and 9 or 10
wheelInput.ClearTextOnFocus = false
Instance.new("UICorner", wheelInput).CornerRadius = UDim.new(0, 8)
local wheelInputStroke = Instance.new("UIStroke", wheelInput)
wheelInputStroke.Color = Color3.fromRGB(140, 80, 255)
wheelInputStroke.Thickness = 1
wheelInputStroke.Transparency = 0.6

local wheelApplyBtn = Instance.new("TextButton", wheelSection)
wheelApplyBtn.Size = UDim2.new(0.24, 0, 0, 32)
wheelApplyBtn.Position = UDim2.new(0.74, 0, 0, 0)
wheelApplyBtn.BackgroundColor3 = Color3.fromRGB(140, 80, 255)
wheelApplyBtn.BackgroundTransparency = 0.15
wheelApplyBtn.Text = "APLICAR"
wheelApplyBtn.TextColor3 = Color3.new(1, 1, 1)
wheelApplyBtn.Font = Enum.Font.GothamBlack
wheelApplyBtn.TextSize = isMobile and 9 or 10
Instance.new("UICorner", wheelApplyBtn).CornerRadius = UDim.new(0, 8)

local wheelListFrame = Instance.new("ScrollingFrame", wheelSection)
wheelListFrame.Size = UDim2.new(1, 0, 0, 0)
wheelListFrame.Position = UDim2.new(0, 0, 0, 38)
wheelListFrame.BackgroundTransparency = 1
wheelListFrame.ScrollBarThickness = 3
wheelListFrame.ScrollBarImageColor3 = Color3.fromRGB(140, 80, 255)
wheelListFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
local wheelLayout = Instance.new("UIListLayout", wheelListFrame)
wheelLayout.Padding = UDim.new(0, 6)
wheelLayout.SortOrder = Enum.SortOrder.LayoutOrder

local function refreshWheelList()
    for _, c in ipairs(wheelListFrame:GetChildren()) do
        if c:IsA("Frame") then c:Destroy() end
    end
    local anchored = {}
    local normal = {}
    for i, item in ipairs(wheelList) do
        if item.anchored then table.insert(anchored, {item=item, idx=i}) else table.insert(normal, {item=item, idx=i}) end
    end
    local ordered = {}
    for _, e in ipairs(anchored) do table.insert(ordered, e) end
    for _, e in ipairs(normal) do table.insert(ordered, e) end

    for order, entry in ipairs(ordered) do
        local i = entry.idx
        local item = entry.item
        local row = Instance.new("Frame", wheelListFrame)
        row.Size = UDim2.new(1, -8, 0, 50)
        row.BackgroundColor3 = item.anchored and Color3.fromRGB(40, 32, 55) or Color3.fromRGB(28, 24, 38)
        row.LayoutOrder = order
        row.Name = "WheelItem_"..i
        Instance.new("UICorner", row).CornerRadius = UDim.new(0, 10)

        local thumb = Instance.new("ImageLabel", row)
        thumb.Size = UDim2.new(0, 42, 0, 42)
        thumb.Position = UDim2.new(0, 4, 0, 4)
        thumb.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
        thumb.BackgroundTransparency = 0.3
        thumb.ScaleType = Enum.ScaleType.Crop
        Instance.new("UICorner", thumb).CornerRadius = UDim.new(0, 8)
        task.spawn(function()
            local a = downloadAndGetAsset(item.url)
            if a and a ~= "" then thumb.Image = a end
        end)

        local nameBox = Instance.new("TextBox", row)
        nameBox.Size = UDim2.new(1, -180, 0, 26)
        nameBox.Position = UDim2.new(0, 52, 0, 12)
        nameBox.BackgroundColor3 = Color3.fromRGB(20, 18, 28)
        nameBox.BackgroundTransparency = 0.3
        nameBox.Text = item.name or ("Wheel "..i)
        nameBox.TextColor3 = Color3.fromRGB(255, 255, 255)
        nameBox.Font = Enum.Font.GothamBold
        nameBox.TextSize = isMobile and 9 or 10
        nameBox.TextXAlignment = Enum.TextXAlignment.Left
        nameBox.ClearTextOnFocus = false
        nameBox.ZIndex = 2
        Instance.new("UICorner", nameBox).CornerRadius = UDim.new(0, 6)
        local nameStroke = Instance.new("UIStroke", nameBox)
        nameStroke.Color = Color3.fromRGB(60, 50, 80)
        nameStroke.Thickness = 1
        nameStroke.Transparency = 0.5
        nameBox.Focused:Connect(function()
            nameStroke.Color = Color3.fromRGB(200, 160, 255)
            nameStroke.Transparency = 0
        end)
        nameBox.FocusLost:Connect(function()
            nameStroke.Color = Color3.fromRGB(60, 50, 80)
            nameStroke.Transparency = 0.5
            if nameBox.Text ~= "" then
                wheelList[i].name = nameBox.Text
                saveWheelList()
            end
        end)

        local anchorBtn = Instance.new("TextButton", row)
        anchorBtn.Size = UDim2.new(0, 30, 0, 30)
        anchorBtn.Position = UDim2.new(1, -98, 0, 10)
        anchorBtn.BackgroundColor3 = item.anchored and Color3.fromRGB(255, 180, 40) or Color3.fromRGB(50, 45, 65)
        anchorBtn.Text = item.anchored and "📌" or "📍"
        anchorBtn.TextSize = 14
        anchorBtn.Font = Enum.Font.GothamBlack
        Instance.new("UICorner", anchorBtn).CornerRadius = UDim.new(0, 8)
        anchorBtn.MouseButton1Click:Connect(function()
            playClick()
            wheelList[i].anchored = not wheelList[i].anchored
            saveWheelList()
            refreshWheelList()
        end)

        local applyBtn = Instance.new("TextButton", row)
        applyBtn.Size = UDim2.new(0, 30, 0, 30)
        applyBtn.Position = UDim2.new(1, -64, 0, 10)
        applyBtn.BackgroundColor3 = Color3.fromRGB(0, 160, 100)
        applyBtn.Text = "✓"
        applyBtn.TextColor3 = Color3.new(1, 1, 1)
        applyBtn.Font = Enum.Font.GothamBlack
        applyBtn.TextSize = 14
        Instance.new("UICorner", applyBtn).CornerRadius = UDim.new(0, 8)
        applyBtn.MouseButton1Click:Connect(function()
            playClick()
            local asset = downloadAndGetAsset(item.url)
            putWheelBehind(asset)
        end)

        local delBtn = Instance.new("TextButton", row)
        delBtn.Size = UDim2.new(0, 30, 0, 30)
        delBtn.Position = UDim2.new(1, -30, 0, 10)
        delBtn.BackgroundColor3 = Color3.fromRGB(200, 40, 60)
        delBtn.Text = "X"
        delBtn.TextColor3 = Color3.new(1, 1, 1)
        delBtn.Font = Enum.Font.GothamBlack
        delBtn.TextSize = 12
        Instance.new("UICorner", delBtn).CornerRadius = UDim.new(0, 8)
        delBtn.MouseButton1Click:Connect(function()
            playClick()
            table.remove(wheelList, i)
            saveWheelList()
            refreshWheelList()
        end)

        animateRow(row)
    end
    task.wait(0.1)
    wheelListFrame.Size = UDim2.new(1, 0, 0, math.min(wheelLayout.AbsoluteContentSize.Y, 250))
    wheelSection.Size = UDim2.new(1,-16,0, 38 + wheelListFrame.Size.Y.Offset + 8)
end

wheelApplyBtn.MouseButton1Click:Connect(function()
    playClick()
    local url = wheelInput.Text:gsub("%s+", "")
    if url == "" then return end
    wheelInput.Text = ""
    wheelInput.PlaceholderText = "⏳ Descargando..."
    task.spawn(function()
        local asset = downloadAndGetAsset(url)
        if asset:find("^http") and not asset:find("rbxasset") then
            wheelInput.PlaceholderText = "❌ no se pudo descargar"
            task.wait(2.5)
            wheelInput.PlaceholderText = "https://i.postimg.cc/..."
            return
        end
        table.insert(wheelList, {url = url, name = "Wheel "..(#wheelList + 1), anchored = false})
        saveWheelList()
        refreshWheelList()
        putWheelBehind(asset)
        wheelInput.PlaceholderText = "https://i.postimg.cc/..."
    end)
end)

local wheelOpacitySlider = createSlider(MainList, "OPACIDAD WHEEL", getgenv().AIOConfig.wheelOpacity, 0, 0.9, function(v)
    getgenv().AIOConfig.wheelOpacity = v
    if createdBack then createdBack.ImageTransparency = v end
    saveWheelOpacity()
end)

pcall(function()
    if isfile and isfile(WHEEL_CURRENT_FILE) then
        local a = readfile(WHEEL_CURRENT_FILE)
        if a and a ~= "" then
            task.spawn(function()
                while true do
                    task.wait(1)
                    local w = findWheel()
                    if w and (not createdBack or not createdBack.Parent) then
                        putWheelBehind(a)
                    end
                end
            end)
        end
    end
end)
refreshWheelList()

-- ── DIVISIÓN ───────────────────────────────────────────────────────────
local divider2 = Instance.new("Frame", MainList)
divider2.Size = UDim2.new(1, -16, 0, 2)
divider2.Position = UDim2.new(0,8,0,0)
divider2.BackgroundColor3 = Color3.fromRGB(140, 80, 255)
divider2.BackgroundTransparency = 0.5
divider2.BorderSizePixel = 0
Instance.new("UICorner", divider2).CornerRadius = UDim.new(0, 1)

-- ── SECCIÓN PARTÍCULAS (opciones) ──────────────────────────────────────
makeSection(MainList, "✨ PARTÍCULAS")

createModule(MainList, "PARTÍCULAS ON", "Muestra las partículas del fondo", "PART_ON", getgenv().AIOConfig.partOn, function(s)
    getgenv().AIOConfig.partOn = s
    savePartSettings()
    updateParticles()
end)

createModule(MainList, "PARTÍCULAS CON FONDO", "Se ven aunque haya foto aplicada", "PART_BG", getgenv().AIOConfig.partWithBg, function(s)
    getgenv().AIOConfig.partWithBg = s
    savePartSettings()
    updateParticles()
end)

-- Selector color partículas
local colorCard = Instance.new("Frame", MainList)
colorCard.Size = UDim2.new(1,-16,0, 80)
colorCard.Position = UDim2.new(0,8,0,0)
colorCard.BackgroundColor3 = Color3.fromRGB(20,20,32)
Instance.new("UICorner", colorCard).CornerRadius = UDim.new(0,14)
local colorLbl = Instance.new("TextLabel", colorCard)
colorLbl.Size = UDim2.new(1,-16,0,18)
colorLbl.Position = UDim2.new(0,8,0,4)
colorLbl.BackgroundTransparency = 1
colorLbl.Text = "COLOR PARTÍCULAS"
colorLbl.TextColor3 = Color3.fromRGB(200,160,255)
colorLbl.Font = Enum.Font.GothamBlack
colorLbl.TextSize = isMobile and 9 or 10
colorLbl.TextXAlignment = Enum.TextXAlignment.Left
for i, pc in ipairs(PART_COLORS) do
    local b = Instance.new("TextButton", colorCard)
    b.Size = UDim2.new(0.105, 0, 0, 28)
    b.Position = UDim2.new(0.02 + ((i-1) % 8) * 0.115, 0, 0, 26 + math.floor((i-1)/8) * 32)
    b.BackgroundColor3 = pc.color
    b.Text = (getgenv().AIOConfig.partColor == i) and "OK" or ""
    b.TextColor3 = Color3.new(0,0,0)
    b.Font = Enum.Font.GothamBlack
    b.TextSize = isMobile and 7 or 8
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 6)
    b.MouseButton1Click:Connect(function()
        playClick()
        getgenv().AIOConfig.partColor = i
        savePartSettings()
        updateParticles()
        for j, btn in ipairs(colorCard:GetChildren()) do
            if btn:IsA("TextButton") and btn ~= b then btn.Text = "" end
        end
        b.Text = "OK"
    end)
end

-- ═══════════════════════════════════════════════════════════════════════
-- TAB 2: SKILL
-- ═══════════════════════════════════════════════════════════════════════
makeSection(SkillList, "SKILL CHECK")

local skillTitle = Instance.new("Frame", SkillList)
skillTitle.Size = UDim2.new(1,-16,0,26)
skillTitle.Position = UDim2.new(0,8,0,0)
skillTitle.BackgroundColor3 = Color3.fromRGB(16,14,26)
Instance.new("UICorner", skillTitle).CornerRadius = UDim.new(0,10)
local skillLbl = Instance.new("TextLabel", skillTitle)
skillLbl.Size = UDim2.new(1,-16,1,0)
skillLbl.Position = UDim2.new(0,8,0,0)
skillLbl.BackgroundTransparency = 1
skillLbl.Text = "Modo: "..getgenv().AIOConfig.skillMode
skillLbl.TextColor3 = Color3.fromRGB(255,215,0)
skillLbl.Font = Enum.Font.GothamBlack
skillLbl.TextSize = isMobile and 9 or 10
skillLbl.TextXAlignment = Enum.TextXAlignment.Left

local skillFrame = Instance.new("Frame", SkillList)
skillFrame.Size = UDim2.new(1,-16,0,44)
skillFrame.Position = UDim2.new(0,8,0,0)
skillFrame.BackgroundColor3 = Color3.fromRGB(16,14,22)
Instance.new("UICorner", skillFrame).CornerRadius = UDim.new(0,12)
for i,m in ipairs({"GREAT","PERFECT","OFF"}) do
    local b = Instance.new("TextButton", skillFrame)
    b.Size = UDim2.new(0.33,-4,1,-8)
    b.Position = UDim2.new((i-1)*0.33+0.01,0,0,4)
    b.Text = m
    b.BackgroundColor3 = Color3.fromRGB(35,35,50)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBlack
    b.TextSize = isMobile and 9 or 10
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,8)
    if getgenv().AIOConfig.skillMode==m then
        b.BackgroundColor3 = m=="GREAT" and Color3.fromRGB(0,220,120) or m=="PERFECT" and Color3.fromRGB(255,215,0) or Color3.fromRGB(255,70,70)
        b.TextColor3 = Color3.fromRGB(0,0,0)
    end
    b.MouseButton1Click:Connect(function()
        playClick()
        getgenv().AIOConfig.skillMode = m
        skillLbl.Text = "Modo: "..m
        for _,c in pairs(skillFrame:GetChildren()) do if c:IsA("TextButton") then c.BackgroundColor3 = Color3.fromRGB(35,35,50) c.TextColor3 = Color3.new(1,1,1) end end
        local col = m=="GREAT" and Color3.fromRGB(0,220,120) or m=="PERFECT" and Color3.fromRGB(255,215,0) or Color3.fromRGB(255,70,70)
        TweenService:Create(b, TweenInfo.new(0.25, Enum.EasingStyle.Back), {BackgroundColor3 = col}):Play()
        b.TextColor3 = Color3.fromRGB(0,0,0)
    end)
end

-- ═══════════════════════════════════════════════════════════════════════
-- TAB 3: TRACKERS
-- ═══════════════════════════════════════════════════════════════════════
makeSection(TrackersList, "REGENT")

local regentCard = Instance.new("Frame", TrackersList)
regentCard.Size = UDim2.new(1,-16,0,95)
regentCard.Position = UDim2.new(0,8,0,0)
regentCard.BackgroundColor3 = Color3.fromRGB(16,14,22)
Instance.new("UICorner", regentCard).CornerRadius = UDim.new(0,14)
local rName = Instance.new("TextLabel", regentCard)
rName.Size = UDim2.new(1,-16,0,20)
rName.Position = UDim2.new(0,8,0,8)
rName.BackgroundTransparency = 1
rName.Text = "Regent Tracker"
rName.TextColor3 = Color3.new(1,1,1)
rName.Font = Enum.Font.GothamBlack
rName.TextSize = isMobile and 10 or 11
rName.TextXAlignment = Enum.TextXAlignment.Left
local rStatus = Instance.new("TextLabel", regentCard)
rStatus.Size = UDim2.new(1,-16,0,16)
rStatus.Position = UDim2.new(0,8,0,28)
rStatus.BackgroundTransparency = 1
rStatus.Text = "Buscando..."
rStatus.TextColor3 = Color3.fromRGB(255,215,0)
rStatus.Font = Enum.Font.Gotham
rStatus.TextSize = isMobile and 8 or 9
rStatus.TextXAlignment = Enum.TextXAlignment.Left
local rPBG = Instance.new("Frame", regentCard)
rPBG.Size = UDim2.new(1,-16,0,8)
rPBG.Position = UDim2.new(0,8,0,50)
rPBG.BackgroundColor3 = Color3.fromRGB(30,30,40)
Instance.new("UICorner", rPBG).CornerRadius = UDim.new(0,4)
local rPF = Instance.new("Frame", rPBG)
rPF.Size = UDim2.new(0.5,0,1,0)
rPF.BackgroundColor3 = Color3.fromRGB(255,215,0)
Instance.new("UICorner", rPF).CornerRadius = UDim.new(0,4)
local rTimer = Instance.new("TextLabel", regentCard)
rTimer.Size = UDim2.new(1,-16,0,16)
rTimer.Position = UDim2.new(0,8,0,64)
rTimer.BackgroundTransparency = 1
rTimer.Text = "Estado: Libre o ocupado"
rTimer.TextColor3 = Color3.fromRGB(150,150,150)
rTimer.Font = Enum.Font.Gotham
rTimer.TextSize = isMobile and 8 or 9
rTimer.TextXAlignment = Enum.TextXAlignment.Left

createModule(TrackersList, "REGENT NOTIFY", "Notifica cuando cambia regente", "RN", getgenv().AIOConfig.regentNotify, function(s)
    getgenv().AIOConfig.regentNotify = s
    if s then showNotification("REGENT NOTIFY", "Activado", "success") else showNotification("REGENT NOTIFY", "Desactivado", "error") end
end)

makeSection(TrackersList, "WHITE TREE")

local treeCard = Instance.new("Frame", TrackersList)
treeCard.Size = UDim2.new(1,-16,0,120)
treeCard.Position = UDim2.new(0,8,0,0)
treeCard.BackgroundColor3 = Color3.fromRGB(16,14,22)
Instance.new("UICorner", treeCard).CornerRadius = UDim.new(0,14)
local tName = Instance.new("TextLabel", treeCard)
tName.Size = UDim2.new(1,-16,0,20)
tName.Position = UDim2.new(0,8,0,8)
tName.BackgroundTransparency = 1
tName.Text = "White Oak Tree"
tName.TextColor3 = Color3.new(1,1,1)
tName.Font = Enum.Font.GothamBlack
tName.TextSize = isMobile and 10 or 11
tName.TextXAlignment = Enum.TextXAlignment.Left
local tStatus = Instance.new("TextLabel", treeCard)
tStatus.Size = UDim2.new(1,-16,0,24)
tStatus.Position = UDim2.new(0,8,0,28)
tStatus.BackgroundTransparency = 1
tStatus.Text = "Esperando datos..."
tStatus.TextColor3 = Color3.fromRGB(120,255,150)
tStatus.Font = Enum.Font.Gotham
tStatus.TextSize = isMobile and 8 or 9
tStatus.TextXAlignment = Enum.TextXAlignment.Left
tStatus.TextWrapped = true
local tPBG = Instance.new("Frame", treeCard)
tPBG.Size = UDim2.new(1,-16,0,8)
tPBG.Position = UDim2.new(0,8,0,56)
tPBG.BackgroundColor3 = Color3.fromRGB(30,30,40)
Instance.new("UICorner", tPBG).CornerRadius = UDim.new(0,4)
local tPF = Instance.new("Frame", tPBG)
tPF.Size = UDim2.new(0,0,1,0)
tPF.BackgroundColor3 = Color3.fromRGB(0,220,120)
Instance.new("UICorner", tPF).CornerRadius = UDim.new(0,4)
local tTimer = Instance.new("TextLabel", treeCard)
tTimer.Size = UDim2.new(1,-16,0,40)
tTimer.Position = UDim2.new(0,8,0,70)
tTimer.BackgroundTransparency = 1
tTimer.Text = "Cooldown: --\nReaparece en: --"
tTimer.TextColor3 = Color3.fromRGB(180,180,180)
tTimer.Font = Enum.Font.Gotham
tTimer.TextSize = isMobile and 8 or 9
tTimer.TextXAlignment = Enum.TextXAlignment.Left
tTimer.TextWrapped = true

createModule(TrackersList, "TREE STATUS", "Muestra cooldown White Tree", "TS", getgenv().AIOConfig.treeStatus, function(s)
    getgenv().AIOConfig.treeStatus = s
    if s then showNotification("TREE STATUS", "Activado", "success") else showNotification("TREE STATUS", "Desactivado", "error") end
end)

-- ═══════════════════════════════════════════════════════════════════════
-- TAB 4: ESP
-- ═══════════════════════════════════════════════════════════════════════
local espConfigs = {
    {key="Totem", name="TOTEM", desc="Boundary", color=Color3.fromRGB(0,120,255)},
    {key="DeathRose", name="DEATH ROSE", desc="Handle", color=Color3.fromRGB(255,50,50)},
    {key="Ancestor", name="ANCESTOR", desc="HRP", color=Color3.fromRGB(255,215,0)},
    {key="Bones", name="BONES", desc="Main", color=Color3.fromRGB(230,230,230)},
    {key="WhiteTree", name="WHITE TREE", desc="Trunk", color=Color3.fromRGB(0,200,100)},
    {key="Sigil", name="SIGIL", desc="Crafting", color=Color3.fromRGB(160,80,255)},
    {key="Dagger", name="DAGGER", desc="Dagger", color=Color3.fromRGB(255,140,0)},
}
local espBillboards = {}
local function destroyESP(key)
    if espBillboards[key] then
        for _,bb in pairs(espBillboards[key]) do pcall(function() bb:Destroy() end) end
        espBillboards[key]=nil
    end
    for _,v in ipairs(CollectionService:GetTagged("ESP34_"..key)) do pcall(function() v:Destroy() end) end
end

makeSection(EspList, "ESP OBJETOS")

local function createEyeButton()
    local state = getgenv().AIOConfig.esp.All or false
    local F = Instance.new("Frame", EspList)
    F.Size = UDim2.new(1,-16,0, 58)
    F.Position = UDim2.new(0,8,0,0)
    F.BackgroundColor3 = state and Color3.fromRGB(34,26,54) or Color3.fromRGB(20,20,32)
    Instance.new("UICorner", F).CornerRadius = UDim.new(0,14)
    local Stroke = Instance.new("UIStroke", F)
    Stroke.Color = state and Color3.fromRGB(140,80,255) or Color3.fromRGB(40,35,60)
    Stroke.Thickness = 1
    Stroke.Transparency = state and 0.1 or 0.4
    local Grad = Instance.new("UIGradient", F)
    Grad.Color = ColorSequence.new(Color3.fromRGB(28,24,44), Color3.fromRGB(18,16,28))
    Grad.Rotation = 90

    local Ico = Instance.new("Frame", F)
    Ico.Size = UDim2.new(0, 38, 0, 38)
    Ico.Position = UDim2.new(0,8,0,10)
    Ico.BackgroundColor3 = state and Color3.fromRGB(50,32,80) or Color3.fromRGB(30,22,50)
    Instance.new("UICorner", Ico).CornerRadius = UDim.new(0,11)
    local IcoStroke = Instance.new("UIStroke", Ico)
    IcoStroke.Color = Color3.fromRGB(140,80,255)
    IcoStroke.Thickness = 1
    IcoStroke.Transparency = 0.5

    local Eye = Instance.new("Frame", Ico)
    Eye.Size = UDim2.new(0,22,0,14)
    Eye.Position = UDim2.new(0.5,-11,0.5,-7)
    Eye.BackgroundColor3 = Color3.fromRGB(20,16,32)
    Instance.new("UICorner", Eye).CornerRadius = UDim.new(0,7)
    local EyeStroke = Instance.new("UIStroke", Eye)
    EyeStroke.Color = Color3.fromRGB(200,160,255)
    EyeStroke.Thickness = 1

    local Pupil = Instance.new("Frame", Eye)
    Pupil.Size = UDim2.new(0, state and 10 or 8, 0, state and 10 or 8)
    Pupil.Position = UDim2.new(0.5, state and -5 or -4, 0.5, state and -5 or -4)
    Pupil.BackgroundColor3 = state and Color3.fromRGB(255,255,255) or Color3.fromRGB(140,80,255)
    Pupil.Rotation = state and 135 or 45
    Instance.new("UICorner", Pupil).CornerRadius = UDim.new(0,2)

    local T1 = Instance.new("TextLabel", F)
    T1.Size = UDim2.new(0,140,0,16)
    T1.Position = UDim2.new(0, 54,0,10)
    T1.BackgroundTransparency = 1
    T1.Text = "BEWITCHED ESP ALL"
    T1.TextColor3 = Color3.new(1,1,1)
    T1.Font = Enum.Font.GothamBlack
    T1.TextSize = isMobile and 10 or 11
    T1.TextXAlignment = Enum.TextXAlignment.Left

    local T2 = Instance.new("TextLabel", F)
    T2.Size = UDim2.new(0,150,0,12)
    T2.Position = UDim2.new(0, 54,0,26)
    T2.BackgroundTransparency = 1
    T2.Text = "Todos los objetos"
    T2.TextColor3 = Color3.fromRGB(110,100,130)
    T2.Font = Enum.Font.Gotham
    T2.TextSize = isMobile and 8 or 9
    T2.TextXAlignment = Enum.TextXAlignment.Left

    local TBG = Instance.new("Frame", F)
    TBG.Size = UDim2.new(0, 44,0, 24)
    TBG.Position = UDim2.new(1, -52,0, 17)
    TBG.BackgroundColor3 = state and Color3.fromRGB(140,80,255) or Color3.fromRGB(34,34,48)
    Instance.new("UICorner", TBG).CornerRadius = UDim.new(0,12)

    local Dot = Instance.new("Frame", TBG)
    Dot.Size = UDim2.new(0, 20,0, 20)
    Dot.Position = state and UDim2.new(1,-22,0,2) or UDim2.new(0,2,0,2)
    Dot.BackgroundColor3 = Color3.fromRGB(10,10,16)
    Instance.new("UICorner", Dot).CornerRadius = UDim.new(0,10)

    task.spawn(function()
        while F.Parent do
            task.wait(math.random(2,4))
            if not F.Parent then break end
            TweenService:Create(Eye, TweenInfo.new(0.1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = UDim2.new(0,22,0,2), Position = UDim2.new(0.5,-11,0.5,-1)}):Play()
            task.wait(0.1)
            TweenService:Create(Eye, TweenInfo.new(0.15, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Size = UDim2.new(0,22,0,14), Position = UDim2.new(0.5,-11,0.5,-7)}):Play()
        end
    end)

    local btn = Instance.new("TextButton", F)
    btn.Size = UDim2.new(1,0,1,0)
    btn.BackgroundTransparency = 1
    btn.Text = ""
    btn.MouseButton1Click:Connect(function()
        playClick()
        state = not state
        getgenv().AIOConfig.esp.All = state
        for _,cfg in pairs(espConfigs) do
            getgenv().AIOConfig.esp[cfg.key] = state
            if not state then destroyESP(cfg.key) end
        end
        TweenService:Create(F, TweenInfo.new(0.3, Enum.EasingStyle.Back), {BackgroundColor3 = state and Color3.fromRGB(34,26,54) or Color3.fromRGB(20,20,32)}):Play()
        TweenService:Create(Stroke, TweenInfo.new(0.3), {Color = state and Color3.fromRGB(140,80,255) or Color3.fromRGB(40,35,60), Transparency = state and 0.1 or 0.4}):Play()
        TweenService:Create(Ico, TweenInfo.new(0.3, Enum.EasingStyle.Back), {BackgroundColor3 = state and Color3.fromRGB(50,32,80) or Color3.fromRGB(30,22,50)}):Play()
        TweenService:Create(Pupil, TweenInfo.new(0.3, Enum.EasingStyle.Back), {BackgroundColor3 = state and Color3.fromRGB(255,255,255) or Color3.fromRGB(140,80,255), Size = state and UDim2.new(0,10,0,10) or UDim2.new(0,8,0,8), Rotation = state and 135 or 45}):Play()
        TweenService:Create(TBG, TweenInfo.new(0.3, Enum.EasingStyle.Back), {BackgroundColor3 = state and Color3.fromRGB(140,80,255) or Color3.fromRGB(34,34,48)}):Play()
        TweenService:Create(Dot, TweenInfo.new(0.3, Enum.EasingStyle.Back), {Position = state and UDim2.new(1,-22,0,2) or UDim2.new(0,2,0,2)}):Play()
        for i=1,15 do spawnTrail(F.AbsolutePosition.X + F.AbsoluteSize.X/2 + math.random(-40,40), F.AbsolutePosition.Y + F.AbsoluteSize.Y/2 + math.random(-10,10), getPartColor()) end
    end)
end
createEyeButton()

for _,cfg in ipairs(espConfigs) do
    createModule(EspList, cfg.name, cfg.desc, "ESP_"..cfg.key, getgenv().AIOConfig.esp[cfg.key], function(s)
        getgenv().AIOConfig.esp[cfg.key]=s
        if not s then destroyESP(cfg.key) end
    end, true) -- silent: no notificación
end

-- ═══════════════════════════════════════════════════════════════════════
-- TAB 5: STOMP
-- ═══════════════════════════════════════════════════════════════════════

-- ⚠️ FUNCIONES SAFE DEL STOMP
local stompBusy = false
local function isSelfStunned()
    local char = LP.Character
    if not char then return true end
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not hum then return true end
    local cv = char:FindFirstChild("CharacterValues")
    if cv then
        local st = cv:FindFirstChild("Stun") or cv:FindFirstChild("Stunned")
        if st and st.Value == true then return true end
    end
    if hum.PlatformStand == true then return true end
    if hum.WalkSpeed <= 1 then return true end
    return false
end

local function isSelfDowned()
    local char = LP.Character
    if not char then return true end
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not hum then return true end
    local cv = char:FindFirstChild("CharacterValues")
    if cv then
        local d = cv:FindFirstChild("Downed") or cv:FindFirstChild("Knocked")
        if d and d.Value == true then return true end
    end
    if char:GetAttribute("Downed") == true or char:GetAttribute("Knocked") == true then return true end
    if hum.Health <= hum.MaxHealth * 0.3 then return true end
    return false
end

local function isEnemyDowned(char)
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not hum or hum.Health <= 0 then return false end
    local cv = char:FindFirstChild("CharacterValues")
    if cv then
        local d = cv:FindFirstChild("Downed") or cv:FindFirstChild("Knocked")
        if d and d.Value == true then return true end
    end
    if char:GetAttribute("Downed") == true or char:GetAttribute("Knocked") == true then return true end
    if hum.Health <= hum.MaxHealth * 0.3 then return true end
    return false
end

local function findExecutePrompt(char)
    for _, d in ipairs(char:GetDescendants()) do
        if d:IsA("ProximityPrompt") and d.Enabled then
            local hrp = char:FindFirstChild("HumanoidRootPart")
            if hrp and d.Parent and d.Parent:IsA("BasePart") then
                if (d.Parent.Position - hrp.Position).Magnitude < 10 then return d end
            end
        end
    end
    return nil
end

local function processSafeStomp(targetMode, listOnly)
    if stompBusy then return end
    if isSelfStunned() or isSelfDowned() then return end
    
    stompBusy = true
    task.spawn(function()
        local char = LP.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        local g = workspace:FindFirstChild("Game")
        local chars = g and g:FindFirstChild("Characters")
        
        if hrp and chars then
            local bestTarget = nil
            local bestDist = 15
            
            for _, c in ipairs(chars:GetChildren()) do
                if c ~= char and c:IsA("Model") and isEnemyDowned(c) then
                    local eHrp = c:FindFirstChild("HumanoidRootPart")
                    if eHrp then
                        local dist = (hrp.Position - eHrp.Position).Magnitude
                        if dist < bestDist then
                            -- ⚠️ CHECK DE TEAM / TARGET
                            local isTarget = false
                            local isTeam = false
                            for _, tName in ipairs(getgenv().AIOConfig.targets) do
                                if tName == c.Name then isTarget = true break end
                            end
                            for _, tmName in ipairs(getgenv().AIOConfig.team) do
                                if tmName == c.Name then isTeam = true break end
                            end
                            
                            local shouldStomp = false
                            if listOnly then
                                if isTarget then shouldStomp = true end -- STOMP ONLY
                            else
                                if not isTeam then shouldStomp = true end -- STOMP ALL (menos team)
                            end
                            
                            if shouldStomp then
                                local prompt = findExecutePrompt(c)
                                if prompt then
                                    bestTarget = {char = c, prompt = prompt}
                                    bestDist = dist
                                end
                            end
                        end
                    end
                end
            end
            
            if bestTarget then
                local prompt = bestTarget.prompt
                local targetChar = bestTarget.char
                local holdTime = prompt.HoldDuration > 0 and prompt.HoldDuration or 0.5
                
                local canceled = false
                pcall(function() prompt:InputHoldBegin() end)
                
                local t0 = tick()
                while tick() - t0 < holdTime do
                    if isSelfStunned() or isSelfDowned() or not isEnemyDowned(targetChar) or not prompt.Enabled then
                        canceled = true
                        break
                    end
                    task.wait(0.05)
                end
                
                pcall(function() prompt:InputHoldEnd() end)
            end
        end
        task.wait(0.5)
        stompBusy = false
    end)
end

makeSection(StompList, "STOMP")

createModule(StompList, "STOMP ALL", "Todos menos team (Safe)", "SA", getgenv().AIOConfig.stompAll, function(s)
    getgenv().AIOConfig.stompAll = s
    if s then getgenv().AIOConfig.stompOnly = false end
end, true)

createModule(StompList, "STOMP ONLY", "Solo targets (Safe)", "SO", getgenv().AIOConfig.stompOnly, function(s)
    getgenv().AIOConfig.stompOnly = s
    if s then getgenv().AIOConfig.stompAll = false end
end, true)

-- ── LOOP DEL STOMP AUTO (Safe) ──
task.spawn(function()
    while Gui and Gui.Parent do
        task.wait(0.3) -- Revisa cada 0.3 segundos
        if getgenv().AIOConfig.stompAll then
            processSafeStomp("ALL", false)
        elseif getgenv().AIOConfig.stompOnly then
            processSafeStomp("ONLY", true)
        end
    end
end)

-- LISTAS DE TEAM Y TARGET
local teamListFrame, targetListFrame
local function createStompList(titleTxt, listKey, otherKey, color, arrow)
    local holder = Instance.new("Frame", StompList)
    holder.Size = UDim2.new(1,-16,0, 90)
    holder.Position = UDim2.new(0,8,0,0)
    holder.BackgroundColor3 = Color3.fromRGB(16,14,22)
    Instance.new("UICorner", holder).CornerRadius = UDim.new(0,14)
    local Stroke = Instance.new("UIStroke", holder)
    Stroke.Color = color
    Stroke.Thickness = 1
    Stroke.Transparency = 0.6

    local title = Instance.new("TextLabel", holder)
    title.Size = UDim2.new(1,-16,0,18)
    title.Position = UDim2.new(0,8,0,8)
    title.BackgroundTransparency = 1
    title.Text = titleTxt
    title.TextColor3 = color
    title.Font = Enum.Font.GothamBlack
    title.TextSize = isMobile and 9 or 10
    title.TextXAlignment = Enum.TextXAlignment.Left

    local inputRow = Instance.new("Frame", holder)
    inputRow.Size = UDim2.new(1,-16,0,30)
    inputRow.Position = UDim2.new(0,8,0,28)
    inputRow.BackgroundTransparency = 1

    local box = Instance.new("TextBox", inputRow)
    box.Size = UDim2.new(0.62,0,1,0)
    box.BackgroundColor3 = Color3.fromRGB(22,20,32)
    box.Text = ""
    box.PlaceholderText = "Nombre..."
    box.PlaceholderColor3 = Color3.fromRGB(90,85,110)
    box.TextColor3 = Color3.new(1,1,1)
    box.Font = Enum.Font.GothamBold
    box.TextSize = isMobile and 9 or 10
    box.ClearTextOnFocus = false
    Instance.new("UICorner", box).CornerRadius = UDim.new(0,8)
    local bS = Instance.new("UIStroke", box)
    bS.Color = color
    bS.Thickness = 1
    bS.Transparency = 0.6

    local add = Instance.new("TextButton", inputRow)
    add.Size = UDim2.new(0.36,0,1,0)
    add.Position = UDim2.new(0.64,0,0,0)
    add.Text = "ADD"
    add.BackgroundColor3 = color
    add.TextColor3 = Color3.new(1,1,1)
    add.Font = Enum.Font.GothamBlack
    add.TextSize = isMobile and 9 or 10
    Instance.new("UICorner", add).CornerRadius = UDim.new(0,8)

    local listHolder = Instance.new("Frame", holder)
    listHolder.Size = UDim2.new(1,-16,0,0)
    listHolder.Position = UDim2.new(0,8,0,62)
    listHolder.BackgroundTransparency = 1
    local lay = Instance.new("UIListLayout", listHolder)
    lay.Padding = UDim.new(0,4)
    lay.SortOrder = Enum.SortOrder.LayoutOrder

    local function refresh()
        for _,c in pairs(listHolder:GetChildren()) do if c:IsA("Frame") then c:Destroy() end end
        local items = getgenv().AIOConfig[listKey]
        local newH = 62 + (#items * 30) + 8
        TweenService:Create(holder, TweenInfo.new(0.3, Enum.EasingStyle.Back), {Size = UDim2.new(1,-16,0,newH)}):Play()
        TweenService:Create(listHolder, TweenInfo.new(0.3, Enum.EasingStyle.Back), {Size = UDim2.new(1,-16,0,#items*30)}):Play()
        for i,name in ipairs(items) do
            local row = Instance.new("Frame", listHolder)
            row.Size = UDim2.new(1,0,0,26)
            row.BackgroundColor3 = Color3.fromRGB(28,24,42)
            Instance.new("UICorner", row).CornerRadius = UDim.new(0,8)
            local nl = Instance.new("TextLabel", row)
            nl.Size = UDim2.new(0.5,0,1,0)
            nl.Position = UDim2.new(0,8,0,0)
            nl.BackgroundTransparency = 1
            nl.Text = name
            nl.TextColor3 = Color3.new(1,1,1)
            nl.Font = Enum.Font.GothamBlack
            nl.TextSize = isMobile and 9 or 10
            nl.TextXAlignment = Enum.TextXAlignment.Left
            local move = Instance.new("TextButton", row)
            move.Size = UDim2.new(0,28,0,18)
            move.Position = UDim2.new(0.58,0,0.5,-9)
            move.Text = arrow
            move.BackgroundColor3 = Color3.fromRGB(60,50,90)
            move.TextColor3 = Color3.new(1,1,1)
            move.Font = Enum.Font.GothamBlack
            move.TextSize = 12
            Instance.new("UICorner", move).CornerRadius = UDim.new(0,4)
            move.MouseButton1Click:Connect(function()
                playClick()
                table.remove(getgenv().AIOConfig[listKey], i)
                table.insert(getgenv().AIOConfig[otherKey], name)
                refresh()
                if teamListFrame and targetListFrame then teamListFrame(); targetListFrame() end
            end)
            local del = Instance.new("TextButton", row)
            del.Size = UDim2.new(0,24,0,18)
            del.Position = UDim2.new(0.86,0,0.5,-9)
            del.Text = "X"
            del.BackgroundColor3 = Color3.fromRGB(200,40,50)
            del.TextColor3 = Color3.new(1,1,1)
            del.Font = Enum.Font.GothamBlack
            Instance.new("UICorner", del).CornerRadius = UDim.new(0,4)
            del.MouseButton1Click:Connect(function() playClick() table.remove(getgenv().AIOConfig[listKey], i) refresh() end)
        end
    end

    local function addPlayer()
        if box.Text=="" then return end
        for _,plr in pairs(Players:GetPlayers()) do
            if plr.Name:lower():find(box.Text:lower(),1,true) then box.Text = plr.Name break end
        end
        table.insert(getgenv().AIOConfig[listKey], box.Text)
        box.Text = ""
        refresh()
        if teamListFrame and targetListFrame then teamListFrame(); targetListFrame() end
    end
    box.FocusLost:Connect(function(enter) if enter then playClick() addPlayer() end end)
    add.MouseButton1Click:Connect(function() playClick() addPlayer() end)
    refresh()
    return refresh
end

teamListFrame = createStompList("TEAM (no pisar) ↓", "team", "targets", Color3.fromRGB(0,140,255), "↓")
targetListFrame = createStompList("TARGET (pisar) ↑", "targets", "team", Color3.fromRGB(255,60,60), "↑")

-- ═══════════════════════════════════════════════════════════════════════
-- TAB 6: OTHER (idioma)
-- ═══════════════════════════════════════════════════════════════════════
makeSection(OtherList, "IDIOMA / LANGUAGE")

for _,lg in ipairs({"es","en"}) do
    local b = Instance.new("TextButton", OtherList)
    b.Size = UDim2.new(1,-16,0,32)
    b.Position = UDim2.new(0,8,0,0)
    b.Text = lg=="es" and "ESPAÑOL" or "ENGLISH"
    b.BackgroundColor3 = getgenv().AIOConfig.lang==lg and Color3.fromRGB(0,220,120) or Color3.fromRGB(35,35,50)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBlack
    b.TextSize = isMobile and 9 or 10
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,10)
    b.MouseButton1Click:Connect(function()
        playClick()
        getgenv().AIOConfig.lang = lg
        for _,c in pairs(OtherList:GetChildren()) do
            if c:IsA("TextButton") and (c.Text=="ESPAÑOL" or c.Text=="ENGLISH") then
                c.BackgroundColor3 = Color3.fromRGB(35,35,50)
            end
        end
        b.BackgroundColor3 = Color3.fromRGB(0,220,120)
    end)
end

-- ── TAB SWITCH ─────────────────────────────────────────────────────────
local currentTab = "MAIN"
local function switchTab(name)
    if name == currentTab then return end
    playClick()
    currentTab = name
    local idx = 1
    for i, n in ipairs(tabNames) do if n == name then idx = i break end end
    TweenService:Create(Indicator, TweenInfo.new(0.35, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Position = UDim2.new((idx-1)*tabWidth, 2, 0, 4)}):Play()
    for n, b in pairs(tabBtns) do b.TextColor3 = n==name and Color3.new(1,1,1) or Color3.fromRGB(100,100,110) end
    MainTab.Visible = name=="MAIN"
    SkillTab.Visible = name=="SKILL"
    TrackersTab.Visible = name=="TRACKERS"
    EspTab.Visible = name=="ESP"
    StompTab.Visible = name=="STOMP"
    OtherTab.Visible = name=="OTHER"
end
for name, b in pairs(tabBtns) do b.MouseButton1Click:Connect(function() switchTab(name) end) end

-- ── RESIZE ─────────────────────────────────────────────────────────────
local ResizeBtn = Instance.new("TextButton", Main)
ResizeBtn.Size = UDim2.new(0,28,0,28)
ResizeBtn.Position = UDim2.new(1,-30,1,-30)
ResizeBtn.BackgroundColor3 = Color3.fromRGB(140,80,255)
ResizeBtn.BackgroundTransparency = 0.15
ResizeBtn.Text = "↘"
ResizeBtn.TextColor3 = Color3.fromRGB(255,255,255)
ResizeBtn.Font = Enum.Font.GothamBold
ResizeBtn.TextSize = 14
ResizeBtn.ZIndex = 20
ResizeBtn.AutoButtonColor = false
Instance.new("UICorner", ResizeBtn).CornerRadius = UDim.new(0,8)
local ResizeStroke = Instance.new("UIStroke", ResizeBtn)
ResizeStroke.Color = Color3.fromRGB(180,130,255)
ResizeStroke.Thickness = 1.2
task.spawn(function()
    while ResizeBtn.Parent do
        TweenService:Create(ResizeBtn, TweenInfo.new(1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {BackgroundTransparency = 0.4, Rotation = 3}):Play()
        task.wait(1)
        TweenService:Create(ResizeBtn, TweenInfo.new(1), {BackgroundTransparency = 0.1, Rotation = -3}):Play()
        task.wait(1)
    end
end)
local resizing = false
local resizeStart, startSize
ResizeBtn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        resizing = true
        resizeStart = input.Position
        startSize = Main.AbsoluteSize
    end
end)
UIS.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then resizing = false end end)
UIS.InputChanged:Connect(function(input)
    if resizing and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local d = input.Position - resizeStart
        local nW = math.clamp(startSize.X + d.X, minSize.X, maxSize.X)
        local nH = math.clamp(startSize.Y + d.Y, minSize.Y, maxSize.Y)
        Main.Size = UDim2.new(0, nW, 0, nH)
        ContentHolder.Size = UDim2.new(1,-20,0, nH - (isMobile and 96 or 108))
    end
end)

-- ── MINIMIZE ───────────────────────────────────────────────────────────
local minimized = false
MinBtn.MouseButton1Click:Connect(function()
    playClick()
    minimized = not minimized
    if minimized then
        TweenService:Create(Main, TweenInfo.new(0.35, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Size = UDim2.new(0, Main.AbsoluteSize.X, 0, isMobile and 80 or 92)}):Play()
        TabBar.Visible = false
        ContentHolder.Visible = false
        ResizeBtn.Visible = false
        MinBtn.Text = "+"
    else
        TweenService:Create(Main, TweenInfo.new(0.35, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Size = UDim2.new(0, Main.AbsoluteSize.X, 0, baseSize.Y)}):Play()
        task.wait(0.15)
        TabBar.Visible = true
        ContentHolder.Visible = true
        ResizeBtn.Visible = true
        MinBtn.Text = "-"
    end
end)

-- ── SKILLCHECK LOOP ────────────────────────────────────────────────────
if getgenv().SkillConn then pcall(function() getgenv().SkillConn:Disconnect() end) end
if getgenv().ClashEndConn then pcall(function() getgenv().ClashEndConn:Disconnect() end) end
local lastPress, canPress = 0, true
getgenv().SkillConn = RunService.Heartbeat:Connect(function()
    if not getgenv().AIOConfig.skillAuto or getgenv().AIOConfig.skillMode=="OFF" then return end
    if not canPress then return end
    local sc = LP.PlayerGui:FindFirstChild("SkillCheck")
    local outline = sc and sc:FindFirstChild("Frame") and sc.Frame:FindFirstChild("Outline")
    if not outline or not outline.Visible then return end
    local cz, needle = outline:FindFirstChild("CheckZone"), outline:FindFirstChild("Needle")
    if not cz or not needle then return end
    local thr = getgenv().AIOConfig.skillMode=="PERFECT" and 30 or 5
    local diff = math.abs((needle.Rotation%360)-(cz.Rotation%360))
    if diff>180 then diff=360-diff end
    if diff < thr then
        if tick()-lastPress < 0.35 then return end
        lastPress = tick(); canPress = false
        task.spawn(function()
            pcall(function() VIM:SendKeyEvent(true, Enum.KeyCode.Space, false, game) end)
            task.wait(0.05)
            pcall(function() VIM:SendKeyEvent(false, Enum.KeyCode.Space, false, game) end)
            task.wait(0.2); canPress = true
        end)
    end
end)
getgenv().ClashEndConn = LP.PlayerGui.ChildRemoved:Connect(function(c)
    if c.Name=="SkillCheck" or c.Name=="Clasher" then
        canPress = true
        pcall(function() VIM:SendKeyEvent(false, Enum.KeyCode.Space, false, game) end)
    end
end)

-- ── LOOP PRINCIPAL (Regent, Tree, ESP) ─────────────────────────────────
task.spawn(function()
    local lastRegent = ""
    local burnTime = nil
    local RESPAWN = 446
    while Gui and Gui.Parent do
        task.wait(0.5)
        pcall(function()
            local g = workspace:FindFirstChild("Game")
            local chars = g and g:FindFirstChild("Characters")
            if chars then
                local owner = nil
                for _,cf in pairs(chars:GetChildren()) do
                    local v = cf:FindFirstChild("CharacterValues")
                    if v and v:FindFirstChild("Regent") then owner = cf.Name break end
                end
                if owner then
                    rStatus.Text = owner==LP.Name and "TU ERES REGENTE" or "Ocupado: "..owner
                    rStatus.TextColor3 = owner==LP.Name and Color3.fromRGB(0,255,150) or Color3.fromRGB(255,80,80)
                    rTimer.Text = owner==LP.Name and "Eres el regente actual" or "Dueño: "..owner
                    TweenService:Create(rPF, TweenInfo.new(0.3), {Size = UDim2.new(1,0,1,0), BackgroundColor3 = owner==LP.Name and Color3.fromRGB(0,255,150) or Color3.fromRGB(255,80,80)}):Play()
                    if getgenv().AIOConfig.regentNotify and lastRegent ~= owner then
                        showNotification("REGENT", owner.." agarró regencia", "heart")
                    end
                    lastRegent = owner
                else
                    rStatus.Text = "LIBRE - Sin dueño"
                    rStatus.TextColor3 = Color3.fromRGB(0,220,120)
                    rTimer.Text = "Puedes agarrar regencia en Debris"
                    TweenService:Create(rPF, TweenInfo.new(0.3), {Size = UDim2.new(1,0,1,0), BackgroundColor3 = Color3.fromRGB(0,220,120)}):Play()
                    if getgenv().AIOConfig.regentNotify and lastRegent ~= "" then
                        showNotification("REGENT", "Regencia libre", "heart")
                    end
                    lastRegent = ""
                end
            end
        end)
        if getgenv().AIOConfig.treeStatus then
            pcall(function()
                local tree = workspace:FindFirstChild("WhiteOakTree", true)
                if tree then
                    local burning = false
                    local trunk = nil
                    for _,d in pairs(tree:GetDescendants()) do
                        if d:IsA("ParticleEmitter") and d.Enabled then burning = true end
                        if d.Name=="Trunk" and d:IsA("BasePart") then trunk = d end
                    end
                    if burning then
                        tStatus.Text = "Estado: Quemándose\nEl árbol está en llamas"
                        tStatus.TextColor3 = Color3.fromRGB(255,100,50)
                        TweenService:Create(tPF, TweenInfo.new(0.3), {Size = UDim2.new(0.3,0,1,0), BackgroundColor3 = Color3.fromRGB(255,100,50)}):Play()
                        tTimer.Text = "Esperando que termine\nNo se puede usar"
                    elseif trunk then
                        if trunk.Transparency>=0.99 then
                            if not burnTime then burnTime = os.clock() end
                            local el = os.clock()-burnTime
                            local prog = math.min(el/RESPAWN,1)
                            local rem = RESPAWN-el
                            tStatus.Text = "Estado: Quemado\nProgreso: "..math.floor(prog*100).."%"
                            TweenService:Create(tPF, TweenInfo.new(0.3), {Size = UDim2.new(prog,0,1,0), BackgroundColor3 = Color3.fromRGB(100,100,100)}):Play()
                            tTimer.Text = "Restante: "..(rem>0 and string.format("%02d:%02d", math.floor(rem/60), rem%60) or "YA").."\nCooldown: "..math.floor(el).."s / 446s"
                        elseif trunk.Transparency<=0.01 then
                            tStatus.Text = "Estado: Vivo\n100% Visible y sólido"
                            TweenService:Create(tPF, TweenInfo.new(0.3), {Size = UDim2.new(1,0,1,0), BackgroundColor3 = Color3.fromRGB(0,220,120)}):Play()
                            tTimer.Text = "Listo para quemar\nPuedes interactuar"
                            burnTime = nil
                        else
                            local pct = math.floor((1-trunk.Transparency)*100)
                            tStatus.Text = "Estado: Reapareciendo\nVisibilidad: "..pct.."%"
                            TweenService:Create(tPF, TweenInfo.new(0.3), {Size = UDim2.new(pct/100,0,1,0), BackgroundColor3 = Color3.fromRGB(0,150,255)}):Play()
                            tTimer.Text = "El árbol regresa\nColisión: "..(trunk.CanCollide and "Sí" or "No")
                            burnTime = nil
                        end
                    end
                end
            end)
        end
        for _,cfg in pairs(espConfigs) do
            if getgenv().AIOConfig.esp[cfg.key] or getgenv().AIOConfig.esp.All then
                local obj = nil
                if cfg.key=="Totem" then
                    local g = workspace:FindFirstChild("Game")
                    local r = g and g:FindFirstChild("Rituals")
                    local t = r and r:FindFirstChild("Totem")
                    obj = t and t:FindFirstChild("Boundary")
                elseif cfg.key=="DeathRose" then
                    local g = workspace:FindFirstChild("Game")
                    local m = g and g:FindFirstChild("Map")
                    local p = m and m:FindFirstChild("Puddles")
                    local d = p and p:FindFirstChild("DeathRose")
                    obj = d and d:FindFirstChild("Handle")
                elseif cfg.key=="Ancestor" then
                    local g = workspace:FindFirstChild("Game")
                    local d = g and g:FindFirstChild("Debris")
                    local a = d and d:FindFirstChild("Ancestor")
                    obj = a and a:FindFirstChild("HumanoidRootPart")
                elseif cfg.key=="Bones" then
                    local g = workspace:FindFirstChild("Game")
                    local d = g and g:FindFirstChild("Debris")
                    local b = d and d:FindFirstChild("Bones")
                    obj = b and b:FindFirstChild("Main")
                elseif cfg.key=="WhiteTree" then
                    local g = workspace:FindFirstChild("Game")
                    local r = g and g:FindFirstChild("Rituals")
                    local w = r and r:FindFirstChild("WhiteOakTree")
                    obj = w and w:FindFirstChild("Trunk")
                else
                    local bp = LP:FindFirstChild("Backpack")
                    if bp then obj = bp:FindFirstChild(cfg.key=="Sigil" and "Crafting Sigil" or "Bone Dagger") end
                    if not obj and LP.Character then obj = LP.Character:FindFirstChild(cfg.key=="Sigil" and "Crafting Sigil" or "Bone Dagger") end
                end
                if obj and obj:IsA("BasePart") then
                    local bb = espBillboards[cfg.key] and espBillboards[cfg.key][1]
                    if not bb or not bb.Parent then
                        bb = Instance.new("BillboardGui")
                        bb.Name = "ESP_"..cfg.key.."_"..math.random(1e4,9e4)
                        bb.Size = UDim2.new(0,140,0,42)
                        bb.StudsOffset = Vector3.new(0,3,0)
                        bb.AlwaysOnTop = true
                        bb.Adornee = obj
                        bb.Parent = workspace.CurrentCamera or PlayerGui
                        CollectionService:AddTag(bb, "ESP34_"..cfg.key)
                        local f = Instance.new("Frame", bb)
                        f.Name = "ESPFrame"
                        f.Size = UDim2.new(1,0,1,0)
                        f.BackgroundColor3 = Color3.fromRGB(15,15,20)
                        f.BackgroundTransparency = 0.2
                        Instance.new("UICorner", f).CornerRadius = UDim.new(0,8)
                        local st = Instance.new("UIStroke", f)
                        st.Color = cfg.color
                        st.Thickness = 1.5
                        st.Parent = f
                        local l = Instance.new("TextLabel", f)
                        l.Name = "DistLabel"
                        l.Size = UDim2.new(1,0,1,0)
                        l.BackgroundTransparency = 1
                        l.Text = cfg.name.."\n-- studs"
                        l.TextColor3 = Color3.new(1,1,1)
                        l.Font = Enum.Font.GothamBlack
                        l.TextSize = 9
                        l.Parent = f
                        espBillboards[cfg.key] = {bb}
                    end
                    if bb.Adornee ~= obj then bb.Adornee = obj end
                    local lbl = bb:FindFirstChild("DistLabel", true)
                    if lbl then
                        local hrp = LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
                        if hrp then
                            local dist = math.floor((hrp.Position - obj.Position).Magnitude)
                            lbl.Text = cfg.name.."\n"..dist.." studs"
                            if dist < 20 then lbl.TextColor3 = Color3.fromRGB(0,255,100)
                            elseif dist < 60 then lbl.TextColor3 = Color3.fromRGB(255,215,0)
                            else lbl.TextColor3 = Color3.fromRGB(255,80,80) end
                        end
                    end
                end
            end
        end
    end
end)

print("V35 cargado - Stomp Safe + BG+Wheel en MAIN + partículas opciones + sliders")
