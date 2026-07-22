--[[
    BLITZ CURLER OPTIMIZER — VERSÃO COM 1 FUNÇÃO REAL
    ----------------------------------------------------
    Bolinha arrastável (com imagem customizada) + painel
    roxo/azul, com APENAS UMA opção funcional: Otimização
    Gráfica. Também tem uma faixa "MINIMIZAR MENU" no
    rodapé, igual ao estilo do menu original.

    O que a opção "OTIMIZAÇÃO GRÁFICA" faz quando ativada:
    - Remove Texture/Decal do workspace
    - Deixa todas as BaseParts com material SmoothPlastic
      e Reflectance 0
    - Desativa ParticleEmitter e Trail
    - Continua aplicando isso a objetos novos que entrarem
      no workspace, enquanto a opção estiver ligada

    Como usar:
    - Cole este script em um LocalScript dentro do StarterGui.
]]

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

----------------------------------------------------------------
-- CONFIGURAÇÃO VISUAL
----------------------------------------------------------------

local ICON_ID = "rbxassetid://93373199521665"

local THEME = {
    Background   = Color3.fromRGB(28, 18, 48),
    HeaderBar    = Color3.fromRGB(18, 10, 34),
    Accent       = Color3.fromRGB(90, 60, 200),
    AccentLight  = Color3.fromRGB(130, 100, 255),
    RowOff       = Color3.fromRGB(60, 20, 20),
    RowOn        = Color3.fromRGB(20, 60, 30),
    TextMain     = Color3.fromRGB(255, 255, 255),
    TextOn       = Color3.fromRGB(150, 255, 150),
    TextOff      = Color3.fromRGB(255, 210, 90),
}

----------------------------------------------------------------
-- LÓGICA REAL: OTIMIZAÇÃO GRÁFICA
----------------------------------------------------------------

local OPTIMIZE_ENABLED = false
local descendantConn

local function optimize(obj)
    if obj:IsA("Texture") or obj:IsA("Decal") then
        obj:Destroy()
    elseif obj:IsA("BasePart") then
        obj.Material = Enum.Material.SmoothPlastic
        obj.Reflectance = 0
    elseif obj:IsA("ParticleEmitter") or obj:IsA("Trail") then
        obj.Enabled = false
    end
end

local function enableOptimization()
    for _, v in pairs(workspace:GetDescendants()) do
        optimize(v)
    end
    if not descendantConn then
        descendantConn = workspace.DescendantAdded:Connect(function(obj)
            if not OPTIMIZE_ENABLED then return end
            task.wait(0.1)
            optimize(obj)
        end)
    end
end

local function disableOptimization()
    if descendantConn then
        descendantConn:Disconnect()
        descendantConn = nil
    end
    -- Observação: desligar não desfaz o que já foi otimizado
    -- (texturas removidas não podem ser restauradas), apenas
    -- para de aplicar a otimização em objetos novos.
end

----------------------------------------------------------------
-- CONSTRUÇÃO DA GUI
----------------------------------------------------------------

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "BlitzCurlerOptimizer"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.Parent = PlayerGui

-- ==== BOLINHA FLUTUANTE ====
local Bubble = Instance.new("ImageButton")
Bubble.Name = "Bubble"
Bubble.Size = UDim2.new(0, 60, 0, 60)
Bubble.Position = UDim2.new(0, 20, 0.5, -30)
Bubble.BackgroundTransparency = 1
Bubble.Image = ICON_ID
Bubble.ScaleType = Enum.ScaleType.Crop
Bubble.AutoButtonColor = false
Bubble.Parent = ScreenGui

local BubbleCorner = Instance.new("UICorner")
BubbleCorner.CornerRadius = UDim.new(1, 0)
BubbleCorner.Parent = Bubble

-- ==== PAINEL PRINCIPAL ====
local Panel = Instance.new("Frame")
Panel.Name = "Panel"
Panel.Size = UDim2.new(0, 420, 0, 310)
Panel.Position = UDim2.new(0, 100, 0.5, -155)
Panel.BackgroundColor3 = THEME.Background
Panel.Visible = false
Panel.ClipsDescendants = true
Panel.Parent = ScreenGui

local PanelCorner = Instance.new("UICorner")
PanelCorner.CornerRadius = UDim.new(0, 12)
PanelCorner.Parent = Panel

-- Header
local Header = Instance.new("Frame")
Header.Size = UDim2.new(1, 0, 0, 50)
Header.BackgroundColor3 = THEME.HeaderBar
Header.Parent = Panel

local HeaderCorner = Instance.new("UICorner")
HeaderCorner.CornerRadius = UDim.new(0, 12)
HeaderCorner.Parent = Header

local HeaderFix = Instance.new("Frame")
HeaderFix.Size = UDim2.new(1, 0, 0, 12)
HeaderFix.Position = UDim2.new(0, 0, 1, -12)
HeaderFix.BackgroundColor3 = THEME.HeaderBar
HeaderFix.BorderSizePixel = 0
HeaderFix.Parent = Header

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -100, 1, 0)
Title.Position = UDim2.new(0, 15, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "⚡ BLITZ CURLER OPTIMIZER ⚡"
Title.Font = Enum.Font.GothamBold
Title.TextSize = 20
Title.TextColor3 = THEME.TextMain
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Header

local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 34, 0, 34)
CloseButton.Position = UDim2.new(1, -44, 0.5, -17)
CloseButton.BackgroundColor3 = THEME.RowOff
CloseButton.Text = "X"
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 16
CloseButton.TextColor3 = THEME.TextMain
CloseButton.Parent = Header

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 8)
CloseCorner.Parent = CloseButton

-- Subtítulo
local SubBar = Instance.new("TextLabel")
SubBar.Size = UDim2.new(1, 0, 0, 30)
SubBar.Position = UDim2.new(0, 0, 0, 50)
SubBar.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
SubBar.Text = "OTIMIZAÇÃO GRÁFICA"
SubBar.Font = Enum.Font.GothamBold
SubBar.TextSize = 14
SubBar.TextColor3 = THEME.AccentLight
SubBar.Parent = Panel

-- ==== BOTÃO ÚNICO: OTIMIZAÇÃO ====
local Row = Instance.new("TextButton")
Row.Size = UDim2.new(1, -40, 0, 50)
Row.Position = UDim2.new(0, 20, 0, 100)
Row.BackgroundColor3 = THEME.RowOff
Row.AutoButtonColor = false
Row.Text = ""
Row.Parent = Panel

local RowCorner = Instance.new("UICorner")
RowCorner.CornerRadius = UDim.new(0, 8)
RowCorner.Parent = Row

local RowLabel = Instance.new("TextLabel")
RowLabel.Size = UDim2.new(1, -20, 1, 0)
RowLabel.Position = UDim2.new(0, 10, 0, 0)
RowLabel.BackgroundTransparency = 1
RowLabel.Text = "OTIMIZAÇÃO GRÁFICA: OFF"
RowLabel.Font = Enum.Font.GothamBold
RowLabel.TextSize = 16
RowLabel.TextColor3 = THEME.TextOff
RowLabel.TextXAlignment = Enum.TextXAlignment.Center
RowLabel.Parent = Row

local Desc = Instance.new("TextLabel")
Desc.Size = UDim2.new(1, -40, 0, 60)
Desc.Position = UDim2.new(0, 20, 0, 165)
Desc.BackgroundTransparency = 1
Desc.Text = "Remove texturas/decais, desativa partículas e trails, e simplifica os materiais para melhorar o FPS."
Desc.Font = Enum.Font.Gotham
Desc.TextSize = 13
Desc.TextWrapped = true
Desc.TextColor3 = Color3.fromRGB(200, 200, 200)
Desc.TextXAlignment = Enum.TextXAlignment.Left
Desc.TextYAlignment = Enum.TextYAlignment.Top
Desc.Parent = Panel

Row.MouseButton1Click:Connect(function()
    OPTIMIZE_ENABLED = not OPTIMIZE_ENABLED
    if OPTIMIZE_ENABLED then
        Row.BackgroundColor3 = THEME.RowOn
        RowLabel.Text = "OTIMIZAÇÃO GRÁFICA: ON"
        RowLabel.TextColor3 = THEME.TextOn
        enableOptimization()
    else
        Row.BackgroundColor3 = THEME.RowOff
        RowLabel.Text = "OTIMIZAÇÃO GRÁFICA: OFF"
        RowLabel.TextColor3 = THEME.TextOff
        disableOptimization()
    end
end)

-- ==== RODAPÉ: MINIMIZAR MENU ====
local Footer = Instance.new("TextButton")
Footer.Size = UDim2.new(1, 0, 0, 46)
Footer.Position = UDim2.new(0, 0, 1, -46)
Footer.BackgroundColor3 = THEME.Accent
Footer.AutoButtonColor = false
Footer.Text = "MINIMIZAR MENU"
Footer.Font = Enum.Font.GothamBold
Footer.TextSize = 16
Footer.TextColor3 = THEME.TextMain
Footer.Parent = Panel

----------------------------------------------------------------
-- INTERAÇÃO: ABRIR/FECHAR PELO CLIQUE NA BOLINHA
----------------------------------------------------------------

local function togglePanel()
    Panel.Visible = not Panel.Visible
end

local function closePanel()
    Panel.Visible = false
end

CloseButton.MouseButton1Click:Connect(closePanel)
Footer.MouseButton1Click:Connect(closePanel)

----------------------------------------------------------------
-- PAINEL ARRASTÁVEL PELA BARRA SUPERIOR (Header)
----------------------------------------------------------------

local panelDragging = false
local panelDragStart, panelStartPos

Header.Active = true

Header.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
        panelDragging = true
        panelDragStart = input.Position
        panelStartPos = Panel.Position

        local conn
        conn = input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                panelDragging = false
                conn:Disconnect()
            end
        end)
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if panelDragging and (input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - panelDragStart
        Panel.Position = UDim2.new(
            panelStartPos.X.Scale, panelStartPos.X.Offset + delta.X,
            panelStartPos.Y.Scale, panelStartPos.Y.Offset + delta.Y
        )
    end
end)

----------------------------------------------------------------
-- BOLINHA ARRASTÁVEL (drag manual, funciona em PC e mobile)
----------------------------------------------------------------

local dragging = false
local dragStart, startPos
local didDrag = false

Bubble.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        didDrag = false
        dragStart = input.Position
        startPos = Bubble.Position

        local conn
        conn = input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
                conn:Disconnect()
                if not didDrag then
                    togglePanel()
                end
            end
        end)
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        if delta.Magnitude > 4 then
            didDrag = true
        end
        Bubble.Position = UDim2.new(
            startPos.X.Scale, startPos.X.Offset + delta.X,
            startPos.Y.Scale, startPos.Y.Offset + delta.Y
        )
    end
end)
