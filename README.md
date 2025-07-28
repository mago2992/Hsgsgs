--[[
    Mago Hub - Script para "Roube um Brainrot"
    Feito por mago2992

    Funções:
 --[ Painel com fundo RGB ]
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "MagoHub"
ScreenGui.Parent = game.CoreGui

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 350, 0, 380)
Frame.Position = UDim2.new(0.5, -175, 0.5, -190)
Frame.AnchorPoint = Vector2.new(0.5, 0.5)
Frame.BackgroundColor3 = Color3.new(1, 0, 0)
Frame.BorderSizePixel = 0
Frame.Parent = ScreenGui

-- RGB Animation
spawn(function()
    local t = 0
    while wait() do
        t = t + 0.01
        Frame.BackgroundColor3 = Color3.fromHSV((tick() % 5) / 5, 0.7, 1)
    end
end)

local Title = Instance.new("TextLabel")
Title.Parent = Frame
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Text = "MAGO HUB"
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 36
Title.TextColor3 = Color3.new(1,1,1)

-- Funções úteis
local function Notify(text)
    game.StarterGui:SetCore("SendNotification", {Title="Mago Hub", Text=text, Duration=3})
end

-- ESP
local function EnableESP()
    for _,v in pairs(game.Players:GetPlayers()) do
        if v ~= game.Players.LocalPlayer then
            if v.Character and v.Character:FindFirstChild("Head") then
                if not v.Character.Head:FindFirstChild("MagoESP") then
                    local bill = Instance.new("BillboardGui", v.Character.Head)
                    bill.Name = "MagoESP"
                    bill.Size = UDim2.new(0,100,0,40)
                    bill.AlwaysOnTop = true
                    local txt = Instance.new("TextLabel", bill)
                    txt.Size = UDim2.new(1,0,1,0)
                    txt.Text = v.Name
                    txt.BackgroundTransparency = 1
                    txt.TextColor3 = Color3.fromHSV(math.random(), 1, 1)
                    txt.TextStrokeTransparency = 0.5
                    txt.Font = Enum.Font.GothamBold
                    txt.TextScaled = true
                end
            end
        end
    end
    Notify("ESP ativado!")
end

-- Speed Boost
local SpeedEnabled = false
local SpeedValue = 80
local function ToggleSpeed()
    SpeedEnabled = not SpeedEnabled
    local char = game.Players.LocalPlayer.Character
    if char and char:FindFirstChildOfClass("Humanoid") then
        char.Humanoid.WalkSpeed = SpeedEnabled and SpeedValue or 16
    end
    Notify(SpeedEnabled and "Speed Boost ativado!" or "Speed Boost desativado!")
end

-- Super Jump
local JumpEnabled = false
local JumpValue = 125
local function ToggleJump()
    JumpEnabled = not JumpEnabled
    local char = game.Players.LocalPlayer.Character
    if char and char:FindFirstChildOfClass("Humanoid") then
        char.Humanoid.JumpPower = JumpEnabled and JumpValue or 50
    end
    Notify(JumpEnabled and "Super Jump ativado!" or "Super Jump desativado!")
end

-- Anti Ragdoll
local function AntiRagdoll()
    local char = game.Players.LocalPlayer.Character
    if not char then return end
    for _,v in pairs(char:GetDescendants()) do
        if v:IsA("BodyAngularVelocity") or v:IsA("BodyVelocity") then
            v:Destroy()
        end
    end
    Notify("Anti Ragdoll ativado!")
end

-- Mostrar tempo da base inimiga (exemplo genérico)
local function ShowBaseTimer()
    local found = false
    for _,v in pairs(workspace:GetDescendants()) do
        if v:IsA("TextLabel") and (v.Text:find("Tempo") or v.Text:find("Cooldown")) then
            found = true
            v.TextColor3 = Color3.new(1,0,0)
            v.TextStrokeTransparency = 0.5
            Notify("Tempo da base do inimigo: " .. v.Text)
        end
    end
    if not found then
        Notify("Tempo/Cooldown da base não encontrado.")
    end
end

-- [Botões no painel]
local ButtonConfig = {
    {Name="ESP",Func=EnableESP,Y=60},
    {Name="Speed Boost",Func=ToggleSpeed,Y=110},
    {Name="Super Jump",Func=ToggleJump,Y=160},
    {Name="Anti Ragdoll",Func=AntiRagdoll,Y=210},
    {Name="Mostrar Tempo Base",Func=ShowBaseTimer,Y=260},
    {Name="Fechar",Func=function() ScreenGui:Destroy() end,Y=320}
}

for _,btn in ipairs(ButtonConfig) do
    local b = Instance.new("TextButton", Frame)
    b.Position = UDim2.new(0.5, -120, 0, btn.Y)
    b.Size = UDim2.new(0, 240, 0, 40)
    b.BackgroundColor3 = Color3.new(0,0,0)
    b.BackgroundTransparency = 0.45
    b.Text = btn.Name
    b.TextColor3 = Color3.new(1,1,1)
    b.TextScaled = true
    b.Font = Enum.Font.GothamBold
    b.MouseButton1Click:Connect(btn.Func)
end

-- [Arrastar o painel]
local UIS = game:GetService("UserInputService")
local dragging, dragInput, dragStart, startPos

Frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = Frame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

Frame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

UIS.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = input.Position - dragStart
        Frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

Notify("Mago Hub carregado! Aproveite.")# Hsgsgs
