-- [[ COELHO HUB BETA - FULL UNIFIED VERSION ]]
local player = game.Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")

-- VARIÁVEIS DE CONTROLE
local farmLevel = false
local farmSaw = false
local autoFruit = false

}

-- 2. LISTA DE BOSSES PARA PRIORIDADE (SEA 1)
local BossList = {
    "The Gorilla King", "Bobby", "The Saw", "Yeti", "Mob Leader", 
    "Vice Admiral", "Saber Expert", "Warden", "Chief Warden", 
    "Swan", "Magma Admiral", "Fishman Lord", "Wysper", 
    "Thunder God", "Cyborg", "Ice Admiral", "Greybeard"
}

-- 3. DICIONÁRIO DE MISSÕES SEA 1 (PROGRESSÃO COMPLETA)
local QuestData = {
    ["Bandit"] = {Quest = "BanditQuest1", Pos = Vector3.new(1059, 15, 1549), Level = 0, ID = 1},
    ["Monkey"] = {Quest = "MonkeyQuest1", Pos = Vector3.new(-1598, 36, 155), Level = 10, ID = 1},
    ["Gorilla"] = {Quest = "GorillaQuest1", Pos = Vector3.new(-1598, 36, 155), Level = 15, ID = 2},
    ["Pirate"] = {Quest = "BuggyQuest1", Pos = Vector3.new(-1141, 4, 3828), Level = 30, ID = 1},
    ["Brute"] = {Quest = "BuggyQuest1", Pos = Vector3.new(-1141, 4, 3828), Level = 40, ID = 2},
    ["Desert Bandit"] = {Quest = "DesertQuest", Pos = Vector3.new(894, 6, 4388), Level = 60, ID = 1},
    ["Desert Officer"] = {Quest = "DesertQuest", Pos = Vector3.new(894, 6, 4388), Level = 75, ID = 2},
    ["Snow Bandits"] = {Quest = "SnowQuest", Pos = Vector3.new(1385, 15, -1322), Level = 90, ID = 1},
    ["Snowman"] = {Quest = "SnowQuest", Pos = Vector3.new(1385, 15, -1322), Level = 100, ID = 2},
    ["Chief Petty Officer"] = {Quest = "MarineQuest2", Pos = Vector3.new(-4855, 23, 4338), Level = 120, ID = 1},
    ["Sky Bandit"] = {Quest = "SkyQuest", Pos = Vector3.new(-1148, 164, -4666), Level = 150, ID = 1},
    ["Dark Master"] = {Quest = "SkyQuest", Pos = Vector3.new(-1148, 164, -4666), Level = 175, ID = 2},
    ["Prisoner"] = {Quest = "PrisonQuest", Pos = Vector3.new(5307, 1, 474), Level = 190, ID = 1},
    ["Toga Warrior"] = {Quest = "ColosseumQuest", Pos = Vector3.new(-1580, 7, -2980), Level = 250, ID = 1},
    ["Military Soldier"] = {Quest = "MagmaQuest", Pos = Vector3.new(-5313, 12, 8515), Level = 300, ID = 1},
    ["Fishman Warrior"] = {Quest = "FishmanQuest", Pos = Vector3.new(61122, 18, 1569), Level = 375, ID = 1},
    ["God's Guard"] = {Quest = "SkyExp1Quest", Pos = Vector3.new(-4721, 845, -1954), Level = 450, ID = 1},
    ["Galley Pirate"] = {Quest = "FountainQuest", Pos = Vector3.new(5256, 38, 4236), Level = 625, ID = 1}
}

-- [[ ESTRUTURA DA GUI - COELHO HUB BETA ]]
local ScreenGui = Instance.new("ScreenGui", player.PlayerGui)
ScreenGui.Name = "CoelhoHubBeta"
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 480, 0, 320)
MainFrame.Position = UDim2.new(0.5, -240, 0.5, -160)
MainFrame.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
MainFrame.Active = true
MainFrame.Draggable = true
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)
Instance.new("UIStroke", MainFrame).Color = Color3.fromRGB(255, 0, 0)

-- Sidebar
local Sidebar = Instance.new("Frame", MainFrame)
Sidebar.Size = UDim2.new(0, 140, 1, 0)
Sidebar.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
Instance.new("UICorner", Sidebar).CornerRadius = UDim.new(0, 12)

local Title = Instance.new("TextLabel", Sidebar)
Title.Size = UDim2.new(1, 0, 0, 50)
Title.Text = "COELHO HUB"
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16
Title.BackgroundTransparency = 1

-- Containers de Conteúdo (Abas)
local MainC = Instance.new("Frame", MainFrame)
MainC.Size = UDim2.new(1, -150, 1, -10)
MainC.Position = UDim2.new(0, 145, 0, 5)
MainC.BackgroundTransparency = 1

local ItemsC = Instance.new("Frame", MainFrame)
ItemsC.Size = UDim2.new(1, -150, 1, -10)
ItemsC.Position = UDim2.new(0, 145, 0, 5)
ItemsC.BackgroundTransparency = 1
ItemsC.Visible = false

local FruitC = Instance.new("Frame", MainFrame)
FruitC.Size = UDim2.new(1, -150, 1, -10)
FruitC.Position = UDim2.new(0, 145, 0, 5)
FruitC.BackgroundTransparency = 1
FruitC.Visible = false

-- Função para trocar abas
local function showTab(tab)
    MainC.Visible = (tab == "Main")
    ItemsC.Visible = (tab == "Items")
    FruitC.Visible = (tab == "Fruit")
end

-- Botões da Sidebar
local function createTabBtn(text, pos, tabName)
    local btn = Instance.new("TextButton", Sidebar)
    btn.Size = UDim2.new(0.9, 0, 0, 40)
    btn.Position = UDim2.new(0.05, 0, 0, pos)
    btn.Text = text
    btn.Font = Enum.Font.GothamBold
    btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    btn.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", btn)
    btn.MouseButton1Click:Connect(function() showTab(tabName) end)
end

createTabBtn("Main", 60, "Main")
createTabBtn("Itens & Missões", 110, "Items")
createTabBtn("Fruit and Raid", 160, "Fruit")

-- BOTÕES FUNCIONAIS
local function createToggle(parent, text, pos)
    local btn = Instance.new("TextButton", parent)
    btn.Size = UDim2.new(0.9, 0, 0, 50)
    btn.Position = UDim2.new(0.05, 0, 0, pos)
    btn.Text = text .. ": OFF"
    btn.Font = Enum.Font.GothamBold
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    btn.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", btn)
    return btn
end

local BtnFarm = createToggle(MainC, "AUTO FARM", 20)
local BtnSaw = createToggle(ItemsC, "SAW BOSS", 20)
local BtnFruit = createToggle(FruitC, "AUTO RANDOM FRUIT", 20)

-- [[ LÓGICA DE MOVIMENTAÇÃO ]]
local function voarPara(posicao)
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local p = player.Character.HumanoidRootPart
        local dist = (p.Position - posicao).Magnitude
        game:GetService("TweenService"):Create(p, TweenInfo.new(dist/300, Enum.EasingStyle.Linear), {CFrame = CFrame.new(posicao)}):Play()
        task.wait(dist/300)
    end
end

-- [[ LÓGICA DE BUSCA DE BOSS ]]
local function CheckBoss()
    for _, name in pairs(BossList) do
        local b = workspace.Enemies:FindFirstChild(name) or workspace.Characters:FindFirstChild(name)
        if b and b:FindFirstChild("HumanoidRootPart") and b.Humanoid.Health > 0 then return b end
    end
    return nil
end

-- [[ LOOP DE COMPRA DE FRUTA ]]
task.spawn(function()
    while true do
        if autoFruit then
            ReplicatedStorage.Remotes.CommF_:InvokeServer("Cousin", "Buy")
            task.wait(20)
        end
        task.wait(1)
    end
end)

-- [[ LOOP PRINCIPAL (FARM + BOSS + BRING MOB) ]]
task.spawn(function()
    while true do
        local saw = workspace.Enemies:FindFirstChild("The Saw") or workspace.Characters:FindFirstChild("The Saw")
        local bossVivo = CheckBoss()

        -- PRIORIDADE 1: THE SAW
        if farmSaw and saw and saw:FindFirstChild("HumanoidRootPart") and saw.Humanoid.Health > 0 then
            voarPara(saw.HumanoidRootPart.Position + Vector3.new(0, 10, 0))
            saw.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, -4)
        
        -- PRIORIDADE 2: BOSSES GERAIS (NO AUTO FARM)
        elseif farmLevel and bossVivo then
            voarPara(bossVivo.HumanoidRootPart.Position + Vector3.new(0, 10, 0))
            bossVivo.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, -4)
        
        -- PRIORIDADE 3: FARM DE NÍVEL
        elseif farmLevel then
            local lvl = player.Data.Level.Value
            local alvo = "Bandit"
            for name, data in pairs(QuestData) do if lvl >= data.Level then alvo = name end end
            local info = QuestData[alvo]

            if not player.PlayerGui.Main.Quest.Visible then
                task.wait(5) -- DELAY DE SEGURANÇA
                voarPara(info.Pos)
                ReplicatedStorage.Remotes.CommF_:InvokeServer("StartQuest", info.Quest, info.ID)
            end
            
            voarPara(info.Pos)
            -- BRING MOB
            for _, v in pairs(workspace.Enemies:GetChildren()) do
                if v.Name == alvo and v:FindFirstChild("HumanoidRootPart") then
                    v.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, -4)
                    v.HumanoidRootPart.CanCollide = false
                end
            end
        end
        task.wait(0.1)
    end
end)

-- CONFIGURAÇÃO DOS BOTÕES
BtnFarm.MouseButton1Click:Connect(function()
    farmLevel = not farmLevel
    BtnFarm.Text = "AUTO FARM: " .. (farmLevel and "ON" or "OFF")
    BtnFarm.BackgroundColor3 = farmLevel and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(40, 40, 40)
end)

BtnSaw.MouseButton1Click:Connect(function()
    farmSaw = not farmSaw
    BtnSaw.Text = "SAW BOSS: " .. (farmSaw and "ON" or "OFF")
    BtnSaw.BackgroundColor3 = farmSaw and Color3.fromRGB(0, 0, 150) or Color3.fromRGB(40, 40, 40)
end)

BtnFruit.MouseButton1Click:Connect(function()
    autoFruit = not autoFruit
    BtnFruit.Text = "AUTO RANDOM FRUIT: " .. (autoFruit and "ON" or "OFF")
    BtnFruit.BackgroundColor3 = autoFruit and Color3.fromRGB(150, 0, 150) or Color3.fromRGB(40, 40, 40)
end)
loadstring(game:HttpGet("https://raw.githubusercontent.com/AnhDzaiScript/Setting/refs/heads/main/FastMax.lua"))()
local function GetBladeHits()
    local targets = {}
    local function GetDistance(v)
        return (v.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
    end
    
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

local function AttackAll()
    local player = game.Players.LocalPlayer
    local character = player.Character
    if not character then return end

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

spawn(function()
    while task.wait() do AttackAll() end
end)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")
local VirtualInputManager = game:GetService("VirtualInputManager")

local Player = Players.LocalPlayer
local Modules = ReplicatedStorage:WaitForChild("Modules")
local Net = Modules:WaitForChild("Net")
local RegisterAttack = Net:WaitForChild("RE/RegisterAttack")
local RegisterHit = Net:WaitForChild("RE/RegisterHit")
local ShootGunEvent = Net:WaitForChild("RE/ShootGunEvent")
local GunValidator = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("Validator2")

local Config = {
    AttackDistance = 95,
    AttackMobs = true,
    AttackPlayers = true,
    AttackCooldown = 0.2,
    ComboResetTime = 0.6,
    MaxCombo = 18,
    HitboxLimbs = {"RightLowerArm", "RightUpperArm", "LeftLowerArm", "LeftUpperArm", "RightHand", "LeftHand"},
    AutoClickEnabled = true
}

local FastAttack = {}
FastAttack.__index = FastAttack

function FastAttack.new()
    local self = setmetatable({
        Debounce = 0,
        ComboDebounce = 0,
        ShootDebounce = 0,
        M1Combo = 1,
        EnemyRootPart = nil,
        Connections = {},
        Overheat = {Dragonstorm = {MaxOverheat = 3, Cooldown = 0, TotalOverheat = 0, Distance = 350, Shooting = false}},
        ShootsPerTarget = {["Dual Flintlock"] = 2},
        SpecialShoots = {["Skull Guitar"] = "TAP", ["Bazooka"] = "Position", ["Cannon"] = "Position", ["Dragonstorm"] = "Overheat"}
    }, FastAttack)
    
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

function FastAttack:IsEntityAlive(entity)
    local humanoid = entity and entity:FindFirstChild("Humanoid")
    return humanoid and humanoid.Health > 0
end

function FastAttack:CheckStun(Character, Humanoid, ToolTip)
    local Stun = Character:FindFirstChild("Stun")
    local Busy = Character:FindFirstChild("Busy")
    if Humanoid.Sit and (ToolTip == "Sword" or ToolTip == "Melee" or ToolTip == "Blox Fruit") then
        return false
    elseif Stun and Stun.Value > 0 or Busy and Busy.Value then
        return false
    end
    return true
end

function FastAttack:GetBladeHits(Character, Distance)
    local Position = Character:GetPivot().Position
    local BladeHits = {}
    Distance = Distance or Config.AttackDistance
    
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

function FastAttack:GetClosestEnemy(Character, Distance)
    local BladeHits = self:GetBladeHits(Character, Distance)
    local Closest, MinDistance = nil, math.huge
    
    for _, Hit in ipairs(BladeHits) do
        local Magnitude = (Character:GetPivot().Position - Hit[2].Position).Magnitude
        if Magnitude < MinDistance then
            MinDistance = Magnitude
            Closest = Hit[2]
        end
    end
    return Closest
end

function FastAttack:GetCombo()
    local Combo = (tick() - self.ComboDebounce) <= Config.ComboResetTime and self.M1Combo or 0
    Combo = Combo >= Config.MaxCombo and 1 or Combo + 1
    self.ComboDebounce = tick()
    self.M1Combo = Combo
    return Combo
end

function FastAttack:ShootInTarget(TargetPosition)
    local Character = Player.Character
    if not self:IsEntityAlive(Character) then return end
    
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

function FastAttack:GetValidator2()
    local v1 = getupvalue(self.ShootFunction, 15)
    local v2 = getupvalue(self.ShootFunction, 13)
    local v3 = getupvalue(self.ShootFunction, 16)
    local v4 = getupvalue(self.ShootFunction, 17)
    local v5 = getupvalue(self.ShootFunction, 14)
    local v6 = getupvalue(self.ShootFunction, 12)
    local v7 = getupvalue(self.ShootFunction, 18)
    
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

function FastAttack:UseNormalClick(Character, Humanoid, Cooldown)
    self.EnemyRootPart = nil
    local BladeHits = self:GetBladeHits(Character)
    
    if self.EnemyRootPart then
        RegisterAttack:FireServer(Cooldown)
        if self.CombatFlags and self.HitFunction then
            self.HitFunction(self.EnemyRootPart, BladeHits)
        else
            RegisterHit:FireServer(self.EnemyRootPart, BladeHits)
        end
    end
end

function FastAttack:UseFruitM1(Character, Equipped, Combo)
    local Targets = self:GetBladeHits(Character)
    if not Targets[1] then return end
    
    local Direction = (Targets[1][2].Position - Character:GetPivot().Position).Unit
    Equipped.LeftClickRemote:FireServer(Direction, Combo)
end

function FastAttack:Attack()
    if not Config.AutoClickEnabled or (tick() - self.Debounce) < Config.AttackCooldown then return end
    local Character = Player.Character
    if not Character or not self:IsEntityAlive(Character) then return end
    
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

local AttackInstance = FastAttack.new()
table.insert(AttackInstance.Connections, RunService.Stepped:Connect(function()
    AttackInstance:Attack()
end))

for _, v in pairs(getgc(true)) do
    if typeof(v) == "function" and iscclosure(v) then
        local name = debug.getinfo(v).name
        if name == "Attack" or name == "attack" or name == "RegisterHit" then
            hookfunction(v, function(...)
                AttackInstance:Attack()
                return v(...)
            end)
        end
    end
end
---Fast 2 ---
local Modules = game.ReplicatedStorage.Modules
local Net = Modules.Net
local Register_Hit, Register_Attack = Net:WaitForChild("RE/RegisterHit"), Net:WaitForChild("RE/RegisterAttack")
local Funcs = {}
function GetAllBladeHits()
    bladehits = {}
    for _, v in pairs(workspace.Enemies:GetChildren()) do
        if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 
        and (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 65 then
            table.insert(bladehits, v)
        end
    end
    return bladehits
end
function Getplayerhit()
    bladehits = {}
    for _, v in pairs(workspace.Characters:GetChildren()) do
        if v.Name ~= game.Players.LocalPlayer.Name and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 
        and (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 65 then
            table.insert(bladehits, v)
        end
    end
    return bladehits
end
function Funcs:Attack()
    local bladehits = {}
    for r,v in pairs(GetAllBladeHits()) do
        table.insert(bladehits, v)
    end
    for r,v in pairs(Getplayerhit()) do
        table.insert(bladehits, v)
    end
    if #bladehits == 0 then return end
    local args = {
        [1] = nil;
        [2] = {},
        [4] = "078da341"
    }
    for r, v in pairs(bladehits) do
        Register_Attack:FireServer(0)
        if not args[1] then
            args[1] = v.Head
        end
        args[2][r] = {
            [1] = v,
            [2] = v.HumanoidRootPart
        }
    end
    Register_Hit:FireServer(unpack(args))
end
