-- [[ COELHO HUB V20 - FULL EDITION (SEA 1) ]]

local Players = game:GetService("Players")
local RS = game:GetService("ReplicatedStorage")
local TS = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local player = Players.LocalPlayer

-- [[ 1. LIMPEZA E CONFIGURAÇÕES ]]
for _, v in pairs(CoreGui:GetChildren()) do
    if v.Name == "CoelhoLoading" or v.Name == "CoelhoHubV20" then v:Destroy() end
end

_G.AutoFarm = false
_G.CurrentRouteIndex = 1

-- [[ 2. BARRINHA DE CARREGAMENTO (5 SEGUNDOS) ]]
local LoadUI = Instance.new("ScreenGui", CoreGui); LoadUI.Name = "CoelhoLoading"
local LoadMain = Instance.new("Frame", LoadUI); LoadMain.Size = UDim2.new(0, 450, 0, 120); LoadMain.Position = UDim2.new(0.5, -225, 0.5, -60); LoadMain.BackgroundColor3 = Color3.fromRGB(3, 3, 3); Instance.new("UICorner", LoadMain); Instance.new("UIStroke", LoadMain).Color = Color3.fromRGB(255, 0, 0)
local BarBack = Instance.new("Frame", LoadMain); BarBack.Size = UDim2.new(0, 380, 0, 10); BarBack.Position = UDim2.new(0.5, -190, 0.7, 0); BarBack.BackgroundColor3 = Color3.fromRGB(15, 15, 15); Instance.new("UICorner", BarBack)
local BarFill = Instance.new("Frame", BarBack); BarFill.Size = UDim2.new(0, 0, 1, 0); BarFill.BackgroundColor3 = Color3.fromRGB(200, 0, 0); Instance.new("UICorner", BarFill)
local LoadTxt = Instance.new("TextLabel", LoadMain); LoadTxt.Size = UDim2.new(1, 0, 0, 40); LoadTxt.Position = UDim2.new(0,0,0,15); LoadTxt.Text = "INICIANDO COELHO HUB..."; LoadTxt.TextColor3 = Color3.new(1,0,0); LoadTxt.BackgroundTransparency = 1; LoadTxt.Font = Enum.Font.GothamBold; LoadTxt.TextSize = 16

-- [[ 3. MOTOR DE LÓGICA DE NÍVEIS E ROTAS ]]
local function UpdateQuestData()
    local MyLevel = player.Data.Level.Value
    local World1 = true 

    -- Suas Rotas Personalizadas
    local CustomRoute = {
        CFrame.new(-1287, 12, 3937),
        CFrame.new(-1269, 11, 3849),
        CFrame.new(-1144, 17, 3903),
        CFrame.new(-967, 22, 3949),
        CFrame.new(-967, 22, 3949)
    }
    
    local DesertBanditRoute = {
        CFrame.new(937, 18, 4432),
        CFrame.new(985, 14, 4497),
        CFrame.new(931, 13, 4534),
        CFrame.new(859, 12, 4489)
    }

    local DesertOfficerRoute = {
        CFrame.new(1611, 13, 4469),
        CFrame.new(1676, 21, 4386),
        CFrame.new(1668, 26, 4319),
        CFrame.new(1584, 17, 4301)
    }

    if World1 then
        if MyLevel <= 9 then
            Mon = "Bandit"; LevelQuest = 1; NameQuest = "BanditQuest1"; CFrameQuest = CFrame.new(1059, 17, 1546)
            CFrameMon = CustomRoute[(_G.CurrentRouteIndex % 5) + 1]
        elseif MyLevel <= 14 then
            Mon = "Monkey"; LevelQuest = 1; NameQuest = "JungleQuest"; CFrameQuest = CFrame.new(-1598, 37, 153)
            CFrameMon = CustomRoute[(_G.CurrentRouteIndex % 5) + 1]
        elseif MyLevel <= 29 then
            Mon = "Gorilla"; LevelQuest = 2; NameQuest = "JungleQuest"; CFrameQuest = CFrame.new(-1598, 37, 153); CFrameMon = CFrame.new(-1128, 40, -451)
        elseif MyLevel <= 39 then
            Mon = "Pirate"; LevelQuest = 1; NameQuest = "BuggyQuest1"; CFrameQuest = CFrame.new(-1140, 4, 3829)
            CFrameMon = CustomRoute[(_G.CurrentRouteIndex % 5) + 1]
        elseif MyLevel <= 59 then
            Mon = "Brute"; LevelQuest = 2; NameQuest = "BuggyQuest1"; CFrameQuest = CFrame.new(-1140, 4, 3829); CFrameMon = CFrame.new(-976, 55, 4304)
        elseif MyLevel <= 74 then
            Mon = "Desert Bandit"; LevelQuest = 1; NameQuest = "DesertQuest"; CFrameQuest = CFrame.new(897, 6, 4389)
            CFrameMon = DesertBanditRoute[(_G.CurrentRouteIndex % 4) + 1]
        elseif MyLevel <= 89 then
            Mon = "Desert Officer"; LevelQuest = 2; NameQuest = "DesertQuest"; CFrameQuest = CFrame.new(897, 6, 4389)
            CFrameMon = DesertOfficerRoute[(_G.CurrentRouteIndex % 4) + 1]
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
    end
end

-- [[ 4. SISTEMAS AUXILIARES (AUTO EQUIP & BRING) ]]
task.spawn(function()
    while task.wait(0.5) do
        if _G.AutoFarm then
            pcall(function()
                -- Auto Equip
                if not player.Character:FindFirstChildOfClass("Tool") then
                    for _, tool in pairs(player.Backpack:GetChildren()) do
                        if tool.ToolTip == "Melee" or tool.Name == "Combat" then
                            player.Character.Humanoid:EquipTool(tool)
                        end
                    end
                end
                -- Smart Bring Mob (Apenas com quest ativa)
                if player.PlayerGui.Main.Quest.Visible then
                    for _, mob in pairs(workspace.Enemies:GetChildren()) do
                        if mob:FindFirstChild("HumanoidRootPart") and (mob.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Magnitude <= 500 then
                            mob.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,-1)
                            mob.HumanoidRootPart.CanCollide = false
                            mob.HumanoidRootPart.Velocity = Vector3.new(0,0,0)
                        end
                    end
                end
            end)
        end
    end
end)

-- [[ 5. INTERFACE PRINCIPAL ]]
local function BuildUI()
    local UI = Instance.new("ScreenGui", CoreGui); UI.Name = "CoelhoHubV20"
    local Main = Instance.new("Frame", UI); Main.Size = UDim2.new(0, 650, 0, 480); Main.Position = UDim2.new(0.5, -325, 0.5, -240); Main.BackgroundColor3 = Color3.fromRGB(0, 0, 0); Main.Active = true; Main.Draggable = true; Instance.new("UICorner", Main)
    local Bg = Instance.new("ImageLabel", Main); Bg.Size = UDim2.new(1, 0, 1, 0); Bg.Image = "rbxassetid://74321670735720"; Bg.ImageTransparency = 0; Bg.ScaleType = Enum.ScaleType.Stretch; Instance.new("UICorner", Bg)
    Instance.new("UIStroke", Main).Color = Color3.fromRGB(200, 0, 0)

    local Title = Instance.new("TextLabel", Main); Title.Size = UDim2.new(0, 200, 0, 50); Title.Position = UDim2.new(0, 20, 0, 10); Title.Text = "COELHO HUB by coelho and eclipse hub"; Title.TextColor3 = Color3.new(1,0,0); Title.Font = Enum.Font.GothamBold; Title.TextSize = 20; Title.BackgroundTransparency = 1; Title.TextXAlignment = Enum.TextXAlignment.Left
    local Search = Instance.new("TextBox", Main); Search.Size = UDim2.new(0, 200, 0, 30); Search.Position = UDim2.new(1, -220, 0, 20); Search.PlaceholderText = "Pesquisar..."; Search.BackgroundColor3 = Color3.fromRGB(15,15,15); Search.TextColor3 = Color3.new(1,1,1); Instance.new("UICorner", Search); Instance.new("UIStroke", Search).Color = Color3.fromRGB(100,0,0)

    local FarmBtn = Instance.new("TextButton", Main); FarmBtn.Size = UDim2.new(0, 300, 0, 60); FarmBtn.Position = UDim2.new(0.5, -150, 0.5, -30); FarmBtn.Text = "AUTO FARM: OFF"; FarmBtn.BackgroundColor3 = Color3.fromRGB(10, 10, 10); FarmBtn.TextColor3 = Color3.new(1,1,1); FarmBtn.Font = Enum.Font.GothamBold; Instance.new("UICorner", FarmBtn); Instance.new("UIStroke", FarmBtn).Color = Color3.fromRGB(150, 0, 0)
    
    FarmBtn.MouseButton1Click:Connect(function()
        _G.AutoFarm = not _G.AutoFarm
        FarmBtn.Text = _G.AutoFarm and "AUTO FARM: ON" or "AUTO FARM: OFF"
    end)

    local Credits = Instance.new("TextLabel", Main); Credits.Size = UDim2.new(1, 0, 0, 30); Credits.Position = UDim2.new(0, 0, 1, -30); Credits.Text = "CRÉDITOS: [COELHO] | V20"; Credits.TextColor3 = Color3.fromRGB(150, 150, 150); Credits.BackgroundTransparency = 1; Credits.Font = Enum.Font.Gotham
end

-- [[ 6. EXECUÇÃO ]]
local anim = TS:Create(BarFill, TweenInfo.new(5), {Size = UDim2.new(1, 0, 1, 0)})
anim:Play()
task.spawn(function()
    task.wait(1); LoadTxt.Text = "LIMPANDO BUGS..."
    task.wait(2); LoadTxt.Text = "CONFIGURANDO ROTAS..."
    task.wait(2); LoadTxt.Text = "TUDO PRONTO!"
end)

anim.Completed:Connect(function()
    LoadUI:Destroy()
    BuildUI()
    -- Loop de Movimento
    task.spawn(function()
        while task.wait(0.1) do
            if _G.AutoFarm then
                pcall(function()
                    UpdateQuestData()
                    if not player.PlayerGui.Main.Quest.Visible then
                        player.Character.HumanoidRootPart.CFrame = CFrameQuest
                        RS.Remotes.CommF_:InvokeServer("StartQuest", NameQuest, LevelQuest)
                    else
                        player.Character.HumanoidRootPart.CFrame = CFrameMon
                    end
                end)
            end
        end
    end)
    -- Loop de Giro de Rota (6 Segundos)
    task.spawn(function()
        while task.wait(6) do
            _G.CurrentRouteIndex = _G.CurrentRouteIndex + 1
        end
    end)
end)
-- [[ COELHO HUB - BRING MOB (7 STUDS ABAIXO) ]]

local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- [[ CONFIGURAÇÕES ]]
_G.BringMobActive = true
_G.DistanceMode = 500 -- Mantive o raio de busca em 500 como padrão

task.spawn(function()
    print("Bring Mob (7 Studs Abaixo) Ativado! 🕶️🔥")
    
    while task.wait(0.1) do
        if _G.BringMobActive then
            pcall(function()
                local character = player.Character
                local Root = character:FindFirstChild("HumanoidRootPart")
                local QuestGui = player.PlayerGui.Main.Quest
                
                -- Só funciona se a missão estiver visível
                if Root and QuestGui.Visible then
                    local myPos = Root.Position
                    
                    for _, mob in pairs(workspace.Enemies:GetChildren()) do
                        if mob:FindFirstChild("HumanoidRootPart") and mob:FindFirstChild("Humanoid") and mob.Humanoid.Health > 0 then
                            
                            local distToMob = (mob.HumanoidRootPart.Position - myPos).Magnitude
                            
                            -- Verifica se o mob está dentro do raio selecionado
                            if distToMob <= _G.DistanceMode then
                                
                                -- Desativa colisão para evitar que o boneco "quique"
                                if mob:FindFirstChild("Head") then
                                    mob.Head.CanCollide = false
                                    mob.HumanoidRootPart.CanCollide = false
                                end

                                -- [[ POSICIONAMENTO: 7 STUDS ABAIXO ]]
                                -- O CFrame.new(0, -7, 0) coloca o mob exatamente abaixo de você
                                mob.HumanoidRootPart.CFrame = Root.CFrame * CFrame.new(0, -7, 0)
                                
                                -- Zera a velocidade para o mob não cair no abismo ou fugir
                                mob.HumanoidRootPart.Velocity = Vector3.new(0, 0, 0)
                            end
                        end
                    end
                end
            end)
        end
    end
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

-- [[ COELHO HUB - APENAS ESCURECER UI ]]

local CoreGui = game:GetService("CoreGui")

-- Limpa se já tiver uma aberta
if CoreGui:FindFirstChild("CoelhoDark") then CoreGui.CoelhoDark:Destroy() end

local UI = Instance.new("ScreenGui", CoreGui)
UI.Name = "Coelho hub by coelho and 𝓟𝓱𝓪𝓴𝓪𝓽𝓱𝓲 𝓴𝔀𝓪𝓶𝓪𝓫𝓲"
