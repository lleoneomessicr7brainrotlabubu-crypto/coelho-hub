-- [[ COELHO HUB V18 - EDIÇÃO ESPECIAL COMPLETA ]]

local CoreGui = game:GetService("CoreGui")
local TS = game:GetService("TweenService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- [[ CONFIGURAÇÃO GLOBAL ]]
_G.Config = {
    FarmAtivo = false,
    StatsAtivo = false
}

-- [[ LIMPEZA ]]
if CoreGui:FindFirstChild("CoelhoHubBeta") then CoreGui.CoelhoHubBeta:Destroy() end

local UI = Instance.new("ScreenGui", CoreGui)
UI.Name = "CoelhoHubBeta"

-- [[ FRAME PRINCIPAL ]]
local Main = Instance.new("Frame", UI)
Main.Name = "MainFrame"
Main.Size = UDim2.new(0, 450, 0, 680)
Main.Position = UDim2.new(0.5, -225, 0.5, -340)
Main.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
Main.BorderSizePixel = 0
Main.Active = true
Main.Draggable = true
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 15)

-- IMAGEM DE FUNDO (BACKGROUND)
local BgImage = Instance.new("ImageLabel", Main)
BgImage.Name = "Background"
BgImage.Size = UDim2.new(1, 0, 1, 0)
BgImage.Image = "rbxassetid://74321670735720"
BgImage.ImageTransparency = 0.4
BgImage.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
BgImage.ScaleType = Enum.ScaleType.Stretch
Instance.new("UICorner", BgImage).CornerRadius = UDim.new(0, 15)

-- BORDA NEON VERMELHA
local Stroke = Instance.new("UIStroke", Main)
Stroke.Color = Color3.fromRGB(255, 0, 0)
Stroke.Thickness = 5

-- [[ BOTÕES DE NAVEGAÇÃO ]]
local CreditsBtn = Instance.new("TextButton", Main)
CreditsBtn.Size = UDim2.new(0, 85, 0, 30)
CreditsBtn.Position = UDim2.new(1, -100, 0, 15)
CreditsBtn.Text = "CREDITS"
CreditsBtn.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
CreditsBtn.TextColor3 = Color3.new(1, 1, 1)
CreditsBtn.Font = Enum.Font.GothamBold
CreditsBtn.TextSize = 12
Instance.new("UICorner", CreditsBtn)
Instance.new("UIStroke", CreditsBtn).Color = Color3.fromRGB(255, 0, 0)

local ExitBtn = Instance.new("TextButton", Main)
ExitBtn.Size = UDim2.new(0, 85, 0, 30)
ExitBtn.Position = UDim2.new(0, 15, 0, 15)
ExitBtn.Text = "EXIT"
ExitBtn.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
ExitBtn.TextColor3 = Color3.new(1, 1, 1)
ExitBtn.Font = Enum.Font.GothamBold
ExitBtn.TextSize = 12
ExitBtn.Visible = false
Instance.new("UICorner", ExitBtn)
Instance.new("UIStroke", ExitBtn).Color = Color3.fromRGB(255, 0, 0)

-- [[ TELA PRINCIPAL (HOME) ]]
local HomeContent = Instance.new("Frame", Main)
HomeContent.Size = UDim2.new(1, 0, 1, 0)
HomeContent.BackgroundTransparency = 1

local Title = Instance.new("TextLabel", HomeContent)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.Position = UDim2.new(0, 0, 0, 50)
Title.Text = "COELHO HUB beta"
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 24
Title.BackgroundTransparency = 1

-- BARRA DE PESQUISA 🔍
local SearchBar = Instance.new("TextBox", HomeContent)
SearchBar.Size = UDim2.new(1, -60, 0, 35)
SearchBar.Position = UDim2.new(0, 30, 0, 100)
SearchBar.PlaceholderText = "Pesquisar funções... 🔍"
SearchBar.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
SearchBar.TextColor3 = Color3.new(1, 1, 1)
SearchBar.Font = Enum.Font.GothamBold
SearchBar.Text = ""
Instance.new("UICorner", SearchBar)
Instance.new("UIStroke", SearchBar).Color = Color3.fromRGB(200, 0, 0)

-- CONTAINER DE BOTÕES (SCROLL)
local Holder = Instance.new("ScrollingFrame", HomeContent)
Holder.Size = UDim2.new(1, -30, 1, -210)
Holder.Position = UDim2.new(0, 15, 0, 150)
Holder.BackgroundTransparency = 1
Holder.CanvasSize = UDim2.new(0, 0, 0, 1200)
Holder.ScrollBarThickness = 2
Instance.new("UIListLayout", Holder).Padding = UDim.new(0, 10)

-- [[ TELA DE CRÉDITOS ]]
local CreditsPage = Instance.new("Frame", Main)
CreditsPage.Size = UDim2.new(1, 0, 1, 0)
CreditsPage.BackgroundTransparency = 1
CreditsPage.Visible = false

local CreditText = Instance.new("TextLabel", CreditsPage)
CreditText.Size = UDim2.new(1, 0, 0, 200)
CreditText.Position = UDim2.new(0, 0, 0, 80)
CreditText.Text = "creator @café-k3y dev helper =eclipse hub STATUS: desenvolvimento"
CreditText.TextColor3 = Color3.new(1, 1, 1)
CreditText.Font = Enum.Font.GothamBold
CreditText.TextSize = 15
CreditText.BackgroundTransparency = 1

-- [[ FUNÇÃO PARA CRIAR BOTÕES ]]
local function CreateToggle(text, configKey)
    local B = Instance.new("TextButton", Holder)
    B.Size = UDim2.new(1, -10, 0, 45)
    B.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    B.Text = text
    B.TextColor3 = Color3.new(1, 1, 1)
    B.Font = Enum.Font.GothamBold
    Instance.new("UICorner", B)
    local bStroke = Instance.new("UIStroke", B)
    bStroke.Color = Color3.fromRGB(50, 50, 50)

    B.MouseButton1Click:Connect(function()
        _G.Config[configKey] = not _G.Config[configKey]
        B.BackgroundColor3 = _G.Config[configKey] and Color3.fromRGB(200, 0, 0) or Color3.fromRGB(20, 20, 20)
        bStroke.Color = _G.Config[configKey] and Color3.fromRGB(255, 0, 0) or Color3.fromRGB(50, 50, 50)
    end)
end

-- [[ ADICIONANDO SEUS BOTÕES ]]
CreateToggle("AUTO FARM LEVEL", "FarmAtivo")
CreateToggle("AUTO STATS (MELEE)", "StatsAtivo")
CreateToggle("AUTO RANDOM FRUIT", "RandomAtivo")
CreateToggle("AUTO STORE FRUIT", "StoreAtivo")

-- [[ LÓGICA DE NAVEGAÇÃO ]]
CreditsBtn.MouseButton1Click:Connect(function()
    HomeContent.Visible = false
    CreditsBtn.Visible = false
    CreditsPage.Visible = true
    ExitBtn.Visible = true
end)

ExitBtn.MouseButton1Click:Connect(function()
    HomeContent.Visible = true
    CreditsBtn.Visible = true
    CreditsPage.Visible = false
    ExitBtn.Visible = false
end)

-- [[ FILTRO DE BUSCA ]]
SearchBar:GetPropertyChangedSignal("Text"):Connect(function()
    local input = SearchBar.Text:lower()
    for _, btn in pairs(Holder:GetChildren()) do
        if btn:IsA("TextButton") then
            btn.Visible = btn.Text:lower():find(input) and true or false
        end
    end
end)
-- [[ COELHO HUB V18 - MÓDULO DE EXPANSÃO LATERAL (MANTENDO CONTEÚDO) ]]

local CoreGui = game:GetService("CoreGui")
local TS = game:GetService("TweenService")

-- Localiza sua UI e o Frame Principal
local UI = CoreGui:FindFirstChild("CoelhoHubBeta")
if not UI then 
    warn("Execute o Coelho Hub V18 primeiro!")
    return 
end

local Main = UI:FindFirstChild("MainFrame")
local Home = Main and Main:FindFirstChild("HomeContent")

if Main and Home then
    -- [[ CONFIGURAÇÃO DA NOVA LARGURA ]]
    local LarguraExpandida = 650 -- Ela vai de 450 para 650
    local Tempo = 0.8
    local Info = TweenInfo.new(Tempo, Enum.EasingStyle.Quart, Enum.EasingStyle.Out)

    -- 1. Expande o Frame Principal mantendo o centro da tela
    TS:Create(Main, Info, {
        Size = UDim2.new(0, LarguraExpandida, 0, 680),
        Position = UDim2.new(0.5, -(LarguraExpandida/2), 0.5, -340)
    }):Play()

    -- 2. Ajusta a Imagem de Fundo para cobrir o novo tamanho
    local Bg = Main:FindFirstChild("Background")
    if Bg then
        TS:Create(Bg, Info, {Size = UDim2.new(1, 0, 1, 0)}):Play()
    end

    -- 3. AJUSTA A BARRA DE PESQUISA (Para não ficar curta)
    local Search = Home:FindFirstChild("SearchBox") or Home:FindFirstChild("SearchBar")
    if Search then
        TS:Create(Search, Info, {Size = UDim2.new(1, -60, 0, 35)}):Play()
    end

    -- 4. AJUSTA O HOLDER DOS BOTÕES (Mantendo todos os botões visíveis e largos)
    local Holder = Home:FindFirstChild("ScrollingFrame") or Home:FindFirstChild("Holder")
    if Holder then
        TS:Create(Holder, Info, {
            Size = UDim2.new(1, -30, 1, -210) -- Mantém a proporção mas ganha largura
        }):Play()
        
        -- Garante que os botões dentro do Holder se adaptem ao novo tamanho
        for _, btn in pairs(Holder:GetChildren()) do
            if btn:IsA("TextButton") then
                TS:Create(btn, Info, {Size = UDim2.new(1, -10, 0, 45)}):Play()
            end
        end
    end

    -- 5. Reposiciona o botão de Credits para o novo canto direito
    local CreditsBtn = Main:FindFirstChild("CreditsBtn") or Main:FindFirstChild("TextButton")
    if CreditsBtn and CreditsBtn.Text == "CREDITS" then
        TS:Create(CreditsBtn, Info, {Position = UDim2.new(1, -100, 0, 15)}):Play()
    end
-- [[ COELHO HUB V18 - EDIÇÃO WIDE (CRÉDITOS COMPRIMIDOS) ]]

local CoreGui = game:GetService("CoreGui")
local TS = game:GetService("TweenService")

-- [[ LIMPEZA ]]
if CoreGui:FindFirstChild("CoelhoLoading") then CoreGui.CoelhoLoading:Destroy() end
if CoreGui:FindFirstChild("CoelhoHubBeta") then CoreGui.CoelhoHubBeta:Destroy() end

-- [[ 1. LOADING BAR ]]
local LoadUI = Instance.new("ScreenGui", CoreGui); LoadUI.Name = "CoelhoLoading"
local LoadMain = Instance.new("Frame", LoadUI); LoadMain.Size = UDim2.new(0, 400, 0, 100); LoadMain.Position = UDim2.new(0.5, -200, 0.5, -50)
LoadMain.BackgroundColor3 = Color3.fromRGB(5, 5, 5); Instance.new("UICorner", LoadMain); Instance.new("UIStroke", LoadMain).Color = Color3.fromRGB(255, 0, 0)
local BarBack = Instance.new("Frame", LoadMain); BarBack.Size = UDim2.new(0, 350, 0, 10); BarBack.Position = UDim2.new(0.5, -175, 0.7, 0); BarBack.BackgroundColor3 = Color3.fromRGB(20, 20, 20); Instance.new("UICorner", BarBack)
local BarFill = Instance.new("Frame", BarBack); BarFill.Size = UDim2.new(0, 0, 1, 0); BarFill.BackgroundColor3 = Color3.fromRGB(255, 0, 0); Instance.new("UICorner", BarFill)

-- [[ 2. UI PRINCIPAL WIDE ]]
local function BuildWideUI()
    local UI = Instance.new("ScreenGui", CoreGui); UI.Name = "CoelhoHubBeta"
    local Main = Instance.new("Frame", UI); Main.Name = "MainFrame"; Main.Size = UDim2.new(0, 650, 0, 600); Main.Position = UDim2.new(0.5, -325, 0.5, -300)
    Main.BackgroundColor3 = Color3.fromRGB(5, 5, 5); Main.Active = true; Main.Draggable = true; Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 15)

    local BgImage = Instance.new("ImageLabel", Main); BgImage.Size = UDim2.new(1, 0, 1, 0); BgImage.Image = "rbxassetid://74321670735720"; BgImage.ImageTransparency = 0.4; BgImage.ScaleType = Enum.ScaleType.Stretch; Instance.new("UICorner", BgImage).CornerRadius = UDim.new(0, 15)
    local Stroke = Instance.new("UIStroke", Main); Stroke.Color = Color3.fromRGB(255, 0, 0); Stroke.Thickness = 4

    -- [[ CRÉDITOS COMPRIMIDOS (POP-UP) ]]
    local CreditsPopup = Instance.new("Frame", Main); CreditsPopup.Size = UDim2.new(0, 250, 0, 120); CreditsPopup.Position = UDim2.new(0.5, -125, 0.5, -60)
    CreditsPopup.BackgroundColor3 = Color3.fromRGB(10, 10, 10); CreditsPopup.Visible = false; Instance.new("UICorner", CreditsPopup)
    Instance.new("UIStroke", CreditsPopup).Color = Color3.fromRGB(255, 0, 0)
    
    local CreditText = Instance.new("TextLabel", CreditsPopup); CreditText.Size = UDim2.new(1, 0, 1, 0); CreditText.Text = "OWNER: COELHO\nDEV: MISTER DEV\nV18 INTANKÁVEL"
    CreditText.TextColor3 = Color3.new(1,1,1); CreditText.Font = Enum.Font.GothamBold; CreditText.BackgroundTransparency = 1
    
    local CloseCredits = Instance.new("TextButton", CreditsPopup); CloseCredits.Size = UDim2.new(0, 20, 0, 20); CloseCredits.Position = UDim2.new(1, -25, 0, 5); CloseCredits.Text = "X"; CloseCredits.BackgroundColor3 = Color3.fromRGB(200,0,0); Instance.new("UICorner", CloseCredits)

    -- [[ BOTÕES ]]
    local CreditsBtn = Instance.new("TextButton", Main); CreditsBtn.Size = UDim2.new(0, 100, 0, 35); CreditsBtn.Position = UDim2.new(1, -115, 0, 15); CreditsBtn.Text = "CREDITS"
    CreditsBtn.BackgroundColor3 = Color3.fromRGB(35, 0, 0); CreditsBtn.TextColor3 = Color3.new(1,1,1); Instance.new("UICorner", CreditsBtn); Instance.new("UIStroke", CreditsBtn).Color = Color3.fromRGB(255, 0, 0)

    -- Lógica dos Créditos
    CreditsBtn.MouseButton1Click:Connect(function() CreditsPopup.Visible = true end)
    CloseCredits.MouseButton1Click:Connect(function() CreditsPopup.Visible = false end)

    print("Coelho Hub: Créditos Comprimidos Carregados! 🕶️🔥")
end

-- [[ 3. LÓGICA DE CARREGAMENTO ]]
local anim = TS:Create(BarFill, TweenInfo.new(3, Enum.EasingStyle.Sine), {Size = UDim2.new(1, 0, 1, 0)})
anim:Play()
anim.Completed:Connect(function() task.wait(0.3); LoadUI:Destroy(); BuildWideUI() end)

-- [[ COELHO HUB V18 - UNIVERSAL COMPATIBILITY MODULE ]]
-- Este script deve ser executado JUNTO ou ANTES do principal.

local UserInputService = game:GetService("UserInputService")
local GuiService = game:GetService("GuiService")
local RunService = game:GetService("RunService")

-- [[ DETECTOR DE DISPOSITIVO ]]
local isMobile = UserInputService.TouchEnabled
local isPC = UserInputService.KeyboardEnabled

print("Coelho Hub: Detectando Hardware...")
if isMobile then
    print("Modo Celular Ativado 📱")
else
    print("Modo PC Ativado 💻")
end

-- [[ FUNÇÃO DE SUPORTE PARA TODOS OS EXECUTORES ]]
-- Garante que funções como 'getgenv', 'writefile' ou 'protect_gui' não crashquem o script
getgenv().CheckExecutor = function()
    local executor = (identifyexecutor or getexecutorname or function() return "Unknown" end)()
    return executor
end

-- [[ SISTEMA DE ARRASTE UNIVERSAL (ANTI-BUG) ]]
-- Melhora o Draggable para que funcione liso no Touch e no Mouse
getgenv().MakeDraggable = function(Frame)
    local Dragging = nil
    local DragInput = nil
    local DragStart = nil
    local StartPos = nil

    local function Update(input)
        local Delta = input.Position - DragStart
        Frame.Position = UDim2.new(StartPos.X.Scale, StartPos.X.Offset + Delta.X, StartPos.Y.Scale, StartPos.Y.Offset + Delta.Y)
    end

    Frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            Dragging = true
            DragStart = input.Position
            StartPos = Frame.Position

            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    Dragging = false
                end
            end)
        end
    end)

    Frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            DragInput = input
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if input == DragInput and Dragging then
            Update(input)
        end
    end)
end

-- [[ AJUSTE DE SENSIBILIDADE DA UI ]]
-- Se for mobile, a gente aumenta um pouco o tamanho dos hits dos botões (invisivelmente)
local function OptimizeUI()
    local UI = game:GetService("CoreGui"):FindFirstChild("CoelhoHubBeta")
    if UI then
        local Main = UI:FindFirstChild("MainFrame")
        if Main then
            if isMobile then
                -- Ajustes específicos para Celular (Executor Delta, Fluxus, etc)
                Main.Size = UDim2.new(0, 500, 0, 450) -- UI um pouco menor e mais gordinha pra tela do celular
            end
            getgenv().MakeDraggable(Main)
        end
    end
end

-- Roda a otimização assim que a UI carregar
task.spawn(function()
    while task.wait(1) do
        if game:GetService("CoreGui"):FindFirstChild("CoelhoHubBeta") then
            OptimizeUI()
            break
        end
    end
end)

loadstring(game:HttpGet("https://raw.githubusercontent.com/AnhDzaiScript/Setting/refs/heads/main/FastMax.lua"))() local function GetBladeHits() local targets = {} local function GetDistance(v) return (v.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude end

for _, part in pairs({game.Workspace.Enemies, game.Workspace.Characters}) do
    for _, v in pairs(part:GetChildren()) do
        if v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Head") and v:FindFirstChild("Humanoid") then
            if GetDistance(v.HumanoidRootPart) < 60 then
                table.insert(targets, v)
            end
        end
    end
end

return targets
end

local function AttackAll() local player = game.Players.LocalPlayer local character = player.Character if not character then return end

local equippedWeapon = character:FindFirstChild("EquippedWeapon")
if not equippedWeapon then return end


local enemies = GetBladeHits()
if #enemies > 0 then
    local netModule = game:GetService("ReplicatedStorage"):WaitForChild("Modules"):WaitForChild("Net")
    netModule:WaitForChild("RE/RegisterAttack"):FireServer(-math.huge)
    
    local args = {nil, {}}
    for i, v in pairs(enemies) do
        if not args[1] then
            args[1] = v.Head
        end
        args[2][i] = {v, v.HumanoidRootPart}
    end
    
    netModule:WaitForChild("RE/RegisterHit"):FireServer(unpack(args))
end
end

spawn(function() while task.wait() do AttackAll() end end)

local Players = game:GetService("Players") local RunService = game:GetService("RunService") local ReplicatedStorage = game:GetService("ReplicatedStorage") local Workspace = game:GetService("Workspace") local VirtualInputManager = game:GetService("VirtualInputManager")

local Player = Players.LocalPlayer local Modules = ReplicatedStorage:WaitForChild("Modules") local Net = Modules:WaitForChild("Net") local RegisterAttack = Net:WaitForChild("RE/RegisterAttack") local RegisterHit = Net:WaitForChild("RE/RegisterHit") local ShootGunEvent = Net:WaitForChild("RE/ShootGunEvent") local GunValidator = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("Validator2")

local Config = { AttackDistance = 95, AttackMobs = true, AttackPlayers = true, AttackCooldown = 0.2, ComboResetTime = 0.6, MaxCombo = 18, HitboxLimbs = {"RightLowerArm", "RightUpperArm", "LeftLowerArm", "LeftUpperArm", "RightHand", "LeftHand"}, AutoClickEnabled = true }

local FastAttack = {} FastAttack.__index = FastAttack

function FastAttack.new() local self = setmetatable({ Debounce = 0, ComboDebounce = 0, ShootDebounce = 0, M1Combo = 1, EnemyRootPart = nil, Connections = {}, Overheat = {Dragonstorm = {MaxOverheat = 3, Cooldown = 0, TotalOverheat = 0, Distance = 350, Shooting = false}}, ShootsPerTarget = {["Dual Flintlock"] = 2}, SpecialShoots = {["Skull Guitar"] = "TAP", ["Bazooka"] = "Position", ["Cannon"] = "Position", ["Dragonstorm"] = "Overheat"} }, FastAttack)

pcall(function()
    self.CombatFlags = require(Modules.Flags).COMBAT_REMOTE_THREAD
    self.ShootFunction = getupvalue(require(ReplicatedStorage.Controllers.CombatController).Attack, 9)
    local LocalScript = Player:WaitForChild("PlayerScripts"):FindFirstChildOfClass("LocalScript")
    if LocalScript and getsenv then
        self.HitFunction = getsenv(LocalScript)._G.SendHitsToServer
    end
end)

return self
end

function FastAttack:IsEntityAlive(entity) local humanoid = entity and entity:FindFirstChild("Humanoid") return humanoid and humanoid.Health > 0 end

function FastAttack:CheckStun(Character, Humanoid, ToolTip) local Stun = Character:FindFirstChild("Stun") local Busy = Character:FindFirstChild("Busy") if Humanoid.Sit and (ToolTip == "Sword" or ToolTip == "Melee" or ToolTip == "Blox Fruit") then return false elseif Stun and Stun.Value > 0 or Busy and Busy.Value then return false end return true end

function FastAttack:GetBladeHits(Character, Distance) local Position = Character:GetPivot().Position local BladeHits = {} Distance = Distance or Config.AttackDistance

local function ProcessTargets(Folder, CanAttack)
    for _, Enemy in ipairs(Folder:GetChildren()) do
        if Enemy ~= Character and self:IsEntityAlive(Enemy) then
            local BasePart = Enemy:FindFirstChild(Config.HitboxLimbs[math.random(#Config.HitboxLimbs)]) or Enemy:FindFirstChild("HumanoidRootPart")
            if BasePart and (Position - BasePart.Position).Magnitude <= Distance then
                if not self.EnemyRootPart then
                    self.EnemyRootPart = BasePart
                else
                    table.insert(BladeHits, {Enemy, BasePart})
                end
            end
        end
    end
end

if Config.AttackMobs then ProcessTargets(Workspace.Enemies) end
if Config.AttackPlayers then ProcessTargets(Workspace.Characters, true) end

return BladeHits
end

function FastAttack:GetClosestEnemy(Character, Distance) local BladeHits = self:GetBladeHits(Character, Distance) local Closest, MinDistance = nil, math.huge

for _, Hit in ipairs(BladeHits) do
    local Magnitude = (Character:GetPivot().Position - Hit[2].Position).Magnitude
    if Magnitude < MinDistance then
        MinDistance = Magnitude
        Closest = Hit[2]
    end
end
return Closest
end

function FastAttack:GetCombo() local Combo = (tick() - self.ComboDebounce) <= Config.ComboResetTime and self.M1Combo or 0 Combo = Combo >= Config.MaxCombo and 1 or Combo + 1 self.ComboDebounce = tick() self.M1Combo = Combo return Combo end

function FastAttack:ShootInTarget(TargetPosition) local Character = Player.Character if not self:IsEntityAlive(Character) then return end

local Equipped = Character:FindFirstChildOfClass("Tool")
if not Equipped or Equipped.ToolTip ~= "Gun" then return end

local Cooldown = Equipped:FindFirstChild("Cooldown") and Equipped.Cooldown.Value or 0.3
if (tick() - self.ShootDebounce) < Cooldown then return end

local ShootType = self.SpecialShoots[Equipped.Name] or "Normal"
if ShootType == "Position" or (ShootType == "TAP" and Equipped:FindFirstChild("RemoteEvent")) then
    Equipped:SetAttribute("LocalTotalShots", (Equipped:GetAttribute("LocalTotalShots") or 0) + 1)
    GunValidator:FireServer(self:GetValidator2())
    
    if ShootType == "TAP" then
        Equipped.RemoteEvent:FireServer("TAP", TargetPosition)
    else
        ShootGunEvent:FireServer(TargetPosition)
    end
    self.ShootDebounce = tick()
else
    VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 1)
    task.wait(0.05)
    VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 1)
    self.ShootDebounce = tick()
end
end

function FastAttack:GetValidator2() local v1 = getupvalue(self.ShootFunction, 15) local v2 = getupvalue(self.ShootFunction, 13) local v3 = getupvalue(self.ShootFunction, 16) local v4 = getupvalue(self.ShootFunction, 17) local v5 = getupvalue(self.ShootFunction, 14) local v6 = getupvalue(self.ShootFunction, 12) local v7 = getupvalue(self.ShootFunction, 18)

local v8 = v6 * v2
local v9 = (v5 * v2 + v6 * v1) % v3
v9 = (v9 * v3 + v8) % v4
v5 = math.floor(v9 / v3)
v6 = v9 - v5 * v3
v7 = v7 + 1

setupvalue(self.ShootFunction, 15, v1)
setupvalue(self.ShootFunction, 13, v2)
setupvalue(self.ShootFunction, 16, v3)
setupvalue(self.ShootFunction, 17, v4)
setupvalue(self.ShootFunction, 14, v5)
setupvalue(self.ShootFunction, 12, v6)
setupvalue(self.ShootFunction, 18, v7)

return math.floor(v9 / v4 * 16777215), v7
end

function FastAttack:UseNormalClick(Character, Humanoid, Cooldown) self.EnemyRootPart = nil local BladeHits = self:GetBladeHits(Character)

if self.EnemyRootPart then
    RegisterAttack:FireServer(Cooldown)
    if self.CombatFlags and self.HitFunction then
        self.HitFunction(self.EnemyRootPart, BladeHits)
    else
        RegisterHit:FireServer(self.EnemyRootPart, BladeHits)
    end
end
end

function FastAttack:UseFruitM1(Character, Equipped, Combo) local Targets = self:GetBladeHits(Character) if not Targets[1] then return end

local Direction = (Targets[1][2].Position - Character:GetPivot().Position).Unit
Equipped.LeftClickRemote:FireServer(Direction, Combo)
end

function FastAttack:Attack() if not Config.AutoClickEnabled or (tick() - self.Debounce) < Config.AttackCooldown then return end local Character = Player.Character if not Character or not self:IsEntityAlive(Character) then return end

local Humanoid = Character.Humanoid
local Equipped = Character:FindFirstChildOfClass("Tool")
if not Equipped then return end

local ToolTip = Equipped.ToolTip
if not table.find({"Melee", "Blox Fruit", "Sword", "Gun"}, ToolTip) then return end

local Cooldown = Equipped:FindFirstChild("Cooldown") and Equipped.Cooldown.Value or Config.AttackCooldown
if not self:CheckStun(Character, Humanoid, ToolTip) then return end

local Combo = self:GetCombo()
Cooldown = Cooldown + (Combo >= Config.MaxCombo and 0.05 or 0)
self.Debounce = Combo >= Config.MaxCombo and ToolTip ~= "Gun" and (tick() + 0.05) or tick()

if ToolTip == "Blox Fruit" and Equipped:FindFirstChild("LeftClickRemote") then
    self:UseFruitM1(Character, Equipped, Combo)
elseif ToolTip == "Gun" then
    local Target = self:GetClosestEnemy(Character, 120)
    if Target then
        self:ShootInTarget(Target.Position)
    end
else
    self:UseNormalClick(Character, Humanoid, Cooldown)
end
end

local AttackInstance = FastAttack.new() table.insert(AttackInstance.Connections, RunService.Stepped:Connect(function() AttackInstance:Attack() end))

for _, v in pairs(getgc(true)) do if typeof(v) == "function" and iscclosure(v) then local name = debug.getinfo(v).name if name == "Attack" or name == "attack" or name == "RegisterHit" then hookfunction(v, function(...) AttackInstance:Attack() return v(...) end) end end end ---Fast 2 --- local Modules = game.ReplicatedStorage.Modules local Net = Modules.Net local Register_Hit, Register_Attack = Net:WaitForChild("RE/RegisterHit"), Net:WaitForChild("RE/RegisterAttack") local Funcs = {} function GetAllBladeHits() bladehits = {} for _, v in pairs(workspace.Enemies:GetChildren()) do if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 and (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 65 then table.insert(bladehits, v) end end return bladehits end function Getplayerhit() bladehits = {} for _, v in pairs(workspace.Characters:GetChildren()) do if v.Name ~= game.Players.LocalPlayer.Name and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 and (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 65 then table.insert(bladehits, v) end end return bladehits end function Funcs:Attack() local bladehits = {} for r,v in pairs(GetAllBladeHits()) do table.insert(bladehits, v) end for r,v in pairs(Getplayerhit()) do table.insert(bladehits, v) end if #bladehits == 0 then return end local args = { [1] = nil; [2] = {}, [4] = "078da341" } for r, v in pairs(bladehits) do Register_Attack:FireServer(0) if not args[1] then args[1] = v.Head end args[2][r] = { [1] = v, [2] = v.HumanoidRootPart } end Register_Hit:FireServer(unpack(args)) end

-- [[ LOGICA DE SELEÇÃO DE MISSÃO (COLE AQUI) ]]
pcall(function()
    local MyLevel = game:GetService("Players").LocalPlayer.Data.Level.Value
    local Mon, LevelQuest, NameQuest, CFrameQuest, CFrameMon

    -- SUA TABELA DE MISSÕES
    if MyLevel <= 9 then
        Mon = "Bandit"; LevelQuest = 1; NameQuest = "BanditQuest1"; CFrameQuest = CFrame.new(1059, 17, 1546); CFrameMon = CFrame.new(943, 45, 1562)
    elseif MyLevel <= 14 then
        Mon = "Monkey"; LevelQuest = 1; NameQuest = "JungleQuest"; CFrameQuest = CFrame.new(-1598, 37, 153); CFrameMon = CFrame.new(-1524, 50, 37)
    elseif MyLevel <= 29 then
        Mon = "Gorilla"; LevelQuest = 2; NameQuest = "JungleQuest"; CFrameQuest = CFrame.new(-1598, 37, 153); CFrameMon = CFrame.new(-1128, 40, -451)
    elseif MyLevel <= 39 then
        Mon = "Pirate"; LevelQuest = 1; NameQuest = "BuggyQuest1"; CFrameQuest = CFrame.new(-1140, 4, 3829); CFrameMon = CFrame.new(-1262, 40, 3905)
    elseif MyLevel <= 59 then
        Mon = "Brute"; LevelQuest = 2; NameQuest = "BuggyQuest1"; CFrameQuest = CFrame.new(-1140, 4, 3829); CFrameMon = CFrame.new(-976, 55, 4304)
    elseif MyLevel <= 74 then
        Mon = "Desert Bandit"; LevelQuest = 1; NameQuest = "DesertQuest"; CFrameQuest = CFrame.new(897, 6, 4389); CFrameMon = CFrame.new(924, 7, 4482)
    elseif MyLevel <= 89 then
        Mon = "Desert Officer"; LevelQuest = 2; NameQuest = "DesertQuest"; CFrameQuest = CFrame.new(897, 6, 4389); CFrameMon = CFrame.new(1608, 9, 4371)
    elseif MyLevel <= 99 then
        Mon = "Snow Bandit"; LevelQuest = 1; NameQuest = "SnowQuest"; CFrameQuest = CFrame.new(1385, 87, -1298); CFrameMon = CFrame.new(1362, 120, -1531)
    elseif MyLevel <= 119 then
        Mon = "Snowman"; LevelQuest = 2; NameQuest = "SnowQuest"; CFrameQuest = CFrame.new(1385, 87, -1298); CFrameMon = CFrame.new(1243, 140, -1437)
    elseif MyLevel <= 149 then
        Mon = "Chief Petty Officer"; LevelQuest = 1; NameQuest = "MarineQuest2"; CFrameQuest = CFrame.new(-5035, 29, 4326); CFrameMon = CFrame.new(-4881, 23, 4274)
    elseif MyLevel <= 174 then
        Mon = "Sky Bandit"; LevelQuest = 1; NameQuest = "SkyQuest"; CFrameQuest = CFrame.new(-4844, 718, -2621); CFrameMon = CFrame.new(-4953, 296, -2899)
    elseif MyLevel <= 189 then
        Mon = "Dark Master"; LevelQuest = 2; NameQuest = "SkyQuest"; CFrameQuest = CFrame.new(-4844, 718, -2621); CFrameMon = CFrame.new(-5260, 391, -2229)
    elseif MyLevel <= 209 then
        Mon = "Prisoner"; LevelQuest = 1; NameQuest = "PrisonerQuest"; CFrameQuest = CFrame.new(5306, 2, 477); CFrameMon = CFrame.new(5099, 0, 474)
    elseif MyLevel <= 249 then
        Mon = "Dangerous Prisoner"; LevelQuest = 2; NameQuest = "PrisonerQuest"; CFrameQuest = CFrame.new(5306, 2, 477); CFrameMon = CFrame.new(5655, 16, 866)
    end

    -- LÓGICA DE EXECUÇÃO
    if _G.Config.FarmAtivo then
        if not game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible then
            -- Vai pro NPC pegar missão
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrameQuest
            task.wait(0.5)
            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest", NameQuest, LevelQuest)
        else
            -- Vai pros monstros atacar
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrameMon
        end
    end
end)

-- [[ CONTINUAÇÃO DA TABELA DE MISSÕES - SEA 1 ]]

elseif MyLevel >= 250 and MyLevel <= 274 then
    Mon = "Warden";
    LevelQuest = 1;
    NameQuest = "PrisonerQuest2";
    NameMon = "Warden";
    CFrameQuest = CFrame.new(5306, 2, 477);
    CFrameMon = CFrame.new(4879, 6, 734);
elseif MyLevel >= 275 and MyLevel <= 299 then
    Mon = "Chief Warden";
    LevelQuest = 2;
    NameQuest = "PrisonerQuest2";
    NameMon = "Chief Warden";
    CFrameQuest = CFrame.new(5306, 2, 477);
    CFrameMon = CFrame.new(5185, 4, 642);
elseif MyLevel >= 300 and MyLevel <= 329 then
    Mon = "Military Soldier";
    LevelQuest = 1;
    NameQuest = "MagmaQuest";
    NameMon = "Military Soldier";
    CFrameQuest = CFrame.new(-5313, 12, 8515);
    CFrameMon = CFrame.new(-5410, 11, 8448);
elseif MyLevel >= 330 and MyLevel <= 374 then
    Mon = "Military Spy";
    LevelQuest = 2;
    NameQuest = "MagmaQuest";
    NameMon = "Military Spy";
    CFrameQuest = CFrame.new(-5313, 12, 8515);
    CFrameMon = CFrame.new(-5816, 84, 8823);
elseif MyLevel >= 375 and MyLevel <= 399 then
    Mon = "Fishman Warrior";
    LevelQuest = 1;
    NameQuest = "FishmanQuest";
    NameMon = "Fishman Warrior";
    CFrameQuest = CFrame.new(61123, 18, 1569);
    CFrameMon = CFrame.new(60860, 18, 1533);
elseif MyLevel >= 400 and MyLevel <= 449 then
    Mon = "Fishman Commando";
    LevelQuest = 2;
    NameQuest = "FishmanQuest";
    NameMon = "Fishman Commando";
    CFrameQuest = CFrame.new(61123, 18, 1569);
    CFrameMon = CFrame.new(61845, 18, 1471);
elseif MyLevel >= 450 and MyLevel <= 474 then
    Mon = "God's Guard";
    LevelQuest = 1;
    NameQuest = "SkyExp1Quest";
    NameMon = "God's Guard";
    CFrameQuest = CFrame.new(-4721, 845, -1954);
    CFrameMon = CFrame.new(-4664, 845, -1867);
elseif MyLevel >= 475 and MyLevel <= 524 then
    Mon = "Shanda";
    LevelQuest = 2;
    NameQuest = "SkyExp1Quest";
    NameMon = "Shanda";
    CFrameQuest = CFrame.new(-4721, 845, -1954);
    CFrameMon = CFrame.new(-4571, 845, -2372);
elseif MyLevel >= 525 and MyLevel <= 549 then
    Mon = "Royal Squad";
    LevelQuest = 1;
    NameQuest = "SkyExp2Quest";
    NameMon = "Royal Squad";
    CFrameQuest = CFrame.new(-7903, 1812, -755);
    CFrameMon = CFrame.new(-7667, 1812, -754);
elseif MyLevel >= 550 and MyLevel <= 624 then
    Mon = "Royal Soldier";
    LevelQuest = 2;
    NameQuest = "SkyExp2Quest";
    NameMon = "Royal Soldier";
    CFrameQuest = CFrame.new(-7903, 1812, -755);
    CFrameMon = CFrame.new(-7735, 1812, -526);
elseif MyLevel >= 625 and MyLevel <= 649 then
    Mon = "Galley Pirate";
    LevelQuest = 1;
    NameQuest = "FountainQuest";
    NameMon = "Galley Pirate";
    CFrameQuest = CFrame.new(5259, 38, 4050);
    CFrameMon = CFrame.new(5532, 38, 3968);
elseif MyLevel >= 650 then
    Mon = "Galley Captain";
    LevelQuest = 2;
    NameQuest = "FountainQuest";
    NameMon = "Galley Captain";
    CFrameQuest = CFrame.new(5259, 38, 4050);
    CFrameMon = CFrame.new(5663, 38, 4474);

    loadstring(game:HttpGet("https://raw.githubusercontent.com/AnhDzaiScript/Setting/refs/heads/main/FastMax.lua"))() local function GetBladeHits() local targets = {} local function GetDistance(v) return (v.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude end

for _, part in pairs({game.Workspace.Enemies, game.Workspace.Characters}) do
    for _, v in pairs(part:GetChildren()) do
        if v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Head") and v:FindFirstChild("Humanoid") then
            if GetDistance(v.HumanoidRootPart) < 60 then
                table.insert(targets, v)
            end
        end
    end
end

return targets
end

local function AttackAll() local player = game.Players.LocalPlayer local character = player.Character if not character then return end

local equippedWeapon = character:FindFirstChild("EquippedWeapon")
if not equippedWeapon then return end


local enemies = GetBladeHits()
if #enemies > 0 then
    local netModule = game:GetService("ReplicatedStorage"):WaitForChild("Modules"):WaitForChild("Net")
    netModule:WaitForChild("RE/RegisterAttack"):FireServer(-math.huge)
    
    local args = {nil, {}}
    for i, v in pairs(enemies) do
        if not args[1] then
            args[1] = v.Head
        end
        args[2][i] = {v, v.HumanoidRootPart}
    end
    
    netModule:WaitForChild("RE/RegisterHit"):FireServer(unpack(args))
end
end

spawn(function() while task.wait() do AttackAll() end end)

local Players = game:GetService("Players") local RunService = game:GetService("RunService") local ReplicatedStorage = game:GetService("ReplicatedStorage") local Workspace = game:GetService("Workspace") local VirtualInputManager = game:GetService("VirtualInputManager")

local Player = Players.LocalPlayer local Modules = ReplicatedStorage:WaitForChild("Modules") local Net = Modules:WaitForChild("Net") local RegisterAttack = Net:WaitForChild("RE/RegisterAttack") local RegisterHit = Net:WaitForChild("RE/RegisterHit") local ShootGunEvent = Net:WaitForChild("RE/ShootGunEvent") local GunValidator = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("Validator2")

local Config = { AttackDistance = 95, AttackMobs = true, AttackPlayers = true, AttackCooldown = 0.2, ComboResetTime = 0.6, MaxCombo = 18, HitboxLimbs = {"RightLowerArm", "RightUpperArm", "LeftLowerArm", "LeftUpperArm", "RightHand", "LeftHand"}, AutoClickEnabled = true }

local FastAttack = {} FastAttack.__index = FastAttack

function FastAttack.new() local self = setmetatable({ Debounce = 0, ComboDebounce = 0, ShootDebounce = 0, M1Combo = 1, EnemyRootPart = nil, Connections = {}, Overheat = {Dragonstorm = {MaxOverheat = 3, Cooldown = 0, TotalOverheat = 0, Distance = 350, Shooting = false}}, ShootsPerTarget = {["Dual Flintlock"] = 2}, SpecialShoots = {["Skull Guitar"] = "TAP", ["Bazooka"] = "Position", ["Cannon"] = "Position", ["Dragonstorm"] = "Overheat"} }, FastAttack)

pcall(function()
    self.CombatFlags = require(Modules.Flags).COMBAT_REMOTE_THREAD
    self.ShootFunction = getupvalue(require(ReplicatedStorage.Controllers.CombatController).Attack, 9)
    local LocalScript = Player:WaitForChild("PlayerScripts"):FindFirstChildOfClass("LocalScript")
    if LocalScript and getsenv then
        self.HitFunction = getsenv(LocalScript)._G.SendHitsToServer
    end
end)

return self
end

function FastAttack:IsEntityAlive(entity) local humanoid = entity and entity:FindFirstChild("Humanoid") return humanoid and humanoid.Health > 0 end

function FastAttack:CheckStun(Character, Humanoid, ToolTip) local Stun = Character:FindFirstChild("Stun") local Busy = Character:FindFirstChild("Busy") if Humanoid.Sit and (ToolTip == "Sword" or ToolTip == "Melee" or ToolTip == "Blox Fruit") then return false elseif Stun and Stun.Value > 0 or Busy and Busy.Value then return false end return true end

function FastAttack:GetBladeHits(Character, Distance) local Position = Character:GetPivot().Position local BladeHits = {} Distance = Distance or Config.AttackDistance

local function ProcessTargets(Folder, CanAttack)
    for _, Enemy in ipairs(Folder:GetChildren()) do
        if Enemy ~= Character and self:IsEntityAlive(Enemy) then
            local BasePart = Enemy:FindFirstChild(Config.HitboxLimbs[math.random(#Config.HitboxLimbs)]) or Enemy:FindFirstChild("HumanoidRootPart")
            if BasePart and (Position - BasePart.Position).Magnitude <= Distance then
                if not self.EnemyRootPart then
                    self.EnemyRootPart = BasePart
                else
                    table.insert(BladeHits, {Enemy, BasePart})
                end
            end
        end
    end
end

if Config.AttackMobs then ProcessTargets(Workspace.Enemies) end
if Config.AttackPlayers then ProcessTargets(Workspace.Characters, true) end

return BladeHits
end

function FastAttack:GetClosestEnemy(Character, Distance) local BladeHits = self:GetBladeHits(Character, Distance) local Closest, MinDistance = nil, math.huge

for _, Hit in ipairs(BladeHits) do
    local Magnitude = (Character:GetPivot().Position - Hit[2].Position).Magnitude
    if Magnitude < MinDistance then
        MinDistance = Magnitude
        Closest = Hit[2]
    end
end
return Closest
end

function FastAttack:GetCombo() local Combo = (tick() - self.ComboDebounce) <= Config.ComboResetTime and self.M1Combo or 0 Combo = Combo >= Config.MaxCombo and 1 or Combo + 1 self.ComboDebounce = tick() self.M1Combo = Combo return Combo end

function FastAttack:ShootInTarget(TargetPosition) local Character = Player.Character if not self:IsEntityAlive(Character) then return end

local Equipped = Character:FindFirstChildOfClass("Tool")
if not Equipped or Equipped.ToolTip ~= "Gun" then return end

local Cooldown = Equipped:FindFirstChild("Cooldown") and Equipped.Cooldown.Value or 0.3
if (tick() - self.ShootDebounce) < Cooldown then return end

local ShootType = self.SpecialShoots[Equipped.Name] or "Normal"
if ShootType == "Position" or (ShootType == "TAP" and Equipped:FindFirstChild("RemoteEvent")) then
    Equipped:SetAttribute("LocalTotalShots", (Equipped:GetAttribute("LocalTotalShots") or 0) + 1)
    GunValidator:FireServer(self:GetValidator2())
    
    if ShootType == "TAP" then
        Equipped.RemoteEvent:FireServer("TAP", TargetPosition)
    else
        ShootGunEvent:FireServer(TargetPosition)
    end
    self.ShootDebounce = tick()
else
    VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 1)
    task.wait(0.05)
    VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 1)
    self.ShootDebounce = tick()
end
end

function FastAttack:GetValidator2() local v1 = getupvalue(self.ShootFunction, 15) local v2 = getupvalue(self.ShootFunction, 13) local v3 = getupvalue(self.ShootFunction, 16) local v4 = getupvalue(self.ShootFunction, 17) local v5 = getupvalue(self.ShootFunction, 14) local v6 = getupvalue(self.ShootFunction, 12) local v7 = getupvalue(self.ShootFunction, 18)

local v8 = v6 * v2
local v9 = (v5 * v2 + v6 * v1) % v3
v9 = (v9 * v3 + v8) % v4
v5 = math.floor(v9 / v3)
v6 = v9 - v5 * v3
v7 = v7 + 1

setupvalue(self.ShootFunction, 15, v1)
setupvalue(self.ShootFunction, 13, v2)
setupvalue(self.ShootFunction, 16, v3)
setupvalue(self.ShootFunction, 17, v4)
setupvalue(self.ShootFunction, 14, v5)
setupvalue(self.ShootFunction, 12, v6)
setupvalue(self.ShootFunction, 18, v7)

return math.floor(v9 / v4 * 16777215), v7
end

function FastAttack:UseNormalClick(Character, Humanoid, Cooldown) self.EnemyRootPart = nil local BladeHits = self:GetBladeHits(Character)

if self.EnemyRootPart then
    RegisterAttack:FireServer(Cooldown)
    if self.CombatFlags and self.HitFunction then
        self.HitFunction(self.EnemyRootPart, BladeHits)
    else
        RegisterHit:FireServer(self.EnemyRootPart, BladeHits)
    end
end
end

function FastAttack:UseFruitM1(Character, Equipped, Combo) local Targets = self:GetBladeHits(Character) if not Targets[1] then return end

local Direction = (Targets[1][2].Position - Character:GetPivot().Position).Unit
Equipped.LeftClickRemote:FireServer(Direction, Combo)
end

function FastAttack:Attack() if not Config.AutoClickEnabled or (tick() - self.Debounce) < Config.AttackCooldown then return end local Character = Player.Character if not Character or not self:IsEntityAlive(Character) then return end

local Humanoid = Character.Humanoid
local Equipped = Character:FindFirstChildOfClass("Tool")
if not Equipped then return end

local ToolTip = Equipped.ToolTip
if not table.find({"Melee", "Blox Fruit", "Sword", "Gun"}, ToolTip) then return end

local Cooldown = Equipped:FindFirstChild("Cooldown") and Equipped.Cooldown.Value or Config.AttackCooldown
if not self:CheckStun(Character, Humanoid, ToolTip) then return end

local Combo = self:GetCombo()
Cooldown = Cooldown + (Combo >= Config.MaxCombo and 0.05 or 0)
self.Debounce = Combo >= Config.MaxCombo and ToolTip ~= "Gun" and (tick() + 0.05) or tick()

if ToolTip == "Blox Fruit" and Equipped:FindFirstChild("LeftClickRemote") then
    self:UseFruitM1(Character, Equipped, Combo)
elseif ToolTip == "Gun" then
    local Target = self:GetClosestEnemy(Character, 120)
    if Target then
        self:ShootInTarget(Target.Position)
    end
else
    self:UseNormalClick(Character, Humanoid, Cooldown)
end
end

local AttackInstance = FastAttack.new() table.insert(AttackInstance.Connections, RunService.Stepped:Connect(function() AttackInstance:Attack() end))

for _, v in pairs(getgc(true)) do if typeof(v) == "function" and iscclosure(v) then local name = debug.getinfo(v).name if name == "Attack" or name == "attack" or name == "RegisterHit" then hookfunction(v, function(...) AttackInstance:Attack() return v(...) end) end end end ---Fast 2 --- local Modules = game.ReplicatedStorage.Modules local Net = Modules.Net local Register_Hit, Register_Attack = Net:WaitForChild("RE/RegisterHit"), Net:WaitForChild("RE/RegisterAttack") local Funcs = {} function GetAllBladeHits() bladehits = {} for _, v in pairs(workspace.Enemies:GetChildren()) do if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 and (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 65 then table.insert(bladehits, v) end end return bladehits end function Getplayerhit() bladehits = {} for _, v in pairs(workspace.Characters:GetChildren()) do if v.Name ~= game.Players.LocalPlayer.Name and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 and (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 65 then table.insert(bladehits, v) end end return bladehits end function Funcs:Attack() local bladehits = {} for r,v in pairs(GetAllBladeHits()) do table.insert(bladehits, v) end for r,v in pairs(Getplayerhit()) do table.insert(bladehits, v) end if #bladehits == 0 then return end local args = { [1] = nil; [2] = {}, [4] = "078da341" } for r, v in pairs(bladehits) do Register_Attack:FireServer(0) if not args[1] then args[1] = v.Head end args[2][r] = { [1] = v, [2] = v.HumanoidRootPart } end Register_Hit:FireServer(unpack(args)) end

-- [[ COELHO HUB V18 - BARRINHA DE LOADING + AUTO QUEST COMPLETO ]]

local CoreGui = game:GetService("CoreGui")
local TS = game:GetService("TweenService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- [[ LIMPEZA ANTES DE INICIAR ]]
if CoreGui:FindFirstChild("CoelhoLoading") then CoreGui.CoelhoLoading:Destroy() end
if CoreGui:FindFirstChild("CoelhoHubBeta") then CoreGui.CoelhoHubBeta:Destroy() end

-- [[ 1. CRIAÇÃO DA BARRINHA DE CARREGAMENTO ]]
local LoadUI = Instance.new("ScreenGui", CoreGui)
LoadUI.Name = "CoelhoLoading"

local LoadMain = Instance.new("Frame", LoadUI)
LoadMain.Size = UDim2.new(0, 450, 0, 120)
LoadMain.Position = UDim2.new(0.5, -225, 0.5, -60)
LoadMain.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
Instance.new("UICorner", LoadMain)
Instance.new("UIStroke", LoadMain).Color = Color3.fromRGB(255, 0, 0)

local LoadTitle = Instance.new("TextLabel", LoadMain)
LoadTitle.Size = UDim2.new(1, 0, 0, 50)
LoadTitle.Text = "COELHO HUB V18 - SINCRONIZANDO MISSÕES..."
LoadTitle.TextColor3 = Color3.fromRGB(255, 0, 0)
LoadTitle.Font = Enum.Font.GothamBold
LoadTitle.TextSize = 16
LoadTitle.BackgroundTransparency = 1

local BarBack = Instance.new("Frame", LoadMain)
BarBack.Size = UDim2.new(0, 380, 0, 12)
BarBack.Position = UDim2.new(0.5, -190, 0.7, 0)
BarBack.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Instance.new("UICorner", BarBack)

local BarFill = Instance.new("Frame", BarBack)
BarFill.Size = UDim2.new(0, 0, 1, 0) -- Começa vazia
BarFill.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
Instance.new("UICorner", BarFill)

-- [[ 2. FUNÇÃO DA UI PRINCIPAL (SÓ ABRE DEPOIS) ]]
local function BuildUI()
    local UI = Instance.new("ScreenGui", CoreGui)
    UI.Name = "CoelhoHubBeta"

    local Main = Instance.new("Frame", UI)
    Main.Name = "MainFrame"
    Main.Size = UDim2.new(0, 650, 0, 600) -- UI Larga como você pediu
    Main.Position = UDim2.new(0.5, -325, 0.5, -300)
    Main.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
    Main.Active = true
    Main.Draggable = true
    Instance.new("UICorner", Main)
    
    local Bg = Instance.new("ImageLabel", Main)
    Bg.Size = UDim2.new(1, 0, 1, 0)
    Bg.Image = "rbxassetid://4705269510"
    Bg.ImageTransparency = 0.4
    Bg.ScaleType = Enum.ScaleType.Stretch
    Instance.new("UICorner", Bg)
    Instance.new("UIStroke", Main).Color = Color3.fromRGB(255, 0, 0)

    -- [[ ONDE AS MISSÕES QUE VOCÊ COPIOU VÃO TRABALHAR ]]
    -- Aqui entraria o seu loop de Farm que pega a tabela de MyLevel
    
    local Title = Instance.new("TextLabel", Main)
    Title.Size = UDim2.new(1, 0, 0, 50)
    Title.Text = "COELHO HUB V18 - SEA 1 READY"
    Title.TextColor3 = Color3.new(1,1,1)
    Title.Font = Enum.Font.GothamBold
    Title.BackgroundTransparency = 1

    print("Coelho Hub: UI Aberta com sucesso!")
end

-- [[ 3. ANIMAÇÃO DA BARRINHA ]]
-- A barrinha vai encher em 4 segundos para dar tempo de carregar tudo
local loadAnim = TS:Create(BarFill, TweenInfo.new(4, Enum.EasingStyle.Quart, Enum.EasingStyle.Out), {
    Size = UDim2.new(1, 0, 1, 0)
})

loadAnim:Play()

-- Quando a barrinha termina:
loadAnim.Completed:Connect(function()
    task.wait(0.5)
    -- Efeito de sumir suave
    TS:Create(LoadMain, TweenInfo.new(0.5), {BackgroundTransparency = 1}):Play()
    TS:Create(LoadTitle, TweenInfo.new(0.5), {TextTransparency = 1}):Play()
    
    task.wait(0.5)
    LoadUI:Destroy() -- Destrói a barrinha
    BuildUI()       -- Abre a UI larga com seus botões e missões
end)

print("Coelho Hub: Módulo de Compatibilidade (PC/Mobile) pronto! 🕶️🔥")

    print("Coelho Hub: UI Expandida com todos os botões preservados! 🕶️🔥")
else
    warn("Erro ao localizar os componentes da UI.")
end
print("Coelho Hub: UI com Busca e Créditos carregada! 🕶️🔥🐇")
