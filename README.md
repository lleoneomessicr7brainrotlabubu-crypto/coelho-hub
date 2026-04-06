- [[ COELHO HUB V18 - O INTANKÁVEL (EDIÇÃO FINAL INTEGRADA) ]]

local Players = game:GetService("Players")
local RS = game:GetService("ReplicatedStorage")
local TS = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local player = Players.LocalPlayer
local CommF = RS:WaitForChild("Remotes"):WaitForChild("CommF_")

-- [[ CONFIGURAÇÃO GLOBAL ]]
_G.Config = { FarmAtivo = false, StatsAtivo = false, RandomAtivo = false, StoreAtivo = false }

-- [[ UI PRINCIPAL ]]
if CoreGui:FindFirstChild("CoelhoHubBeta") then CoreGui.CoelhoHubBeta:Destroy() end
local UI = Instance.new("ScreenGui", CoreGui); UI.Name = "CoelhoHubBeta"
local Main = Instance.new("Frame", UI); Main.Size = UDim2.new(0, 450, 0, 680); Main.Position = UDim2.new(0.5, -225, 0.5, -340)
Main.BackgroundColor3 = Color3.fromRGB(5, 5, 5); Main.Visible = false; Main.Active = true; Main.Draggable = true
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 15)
local Stroke = Instance.new("UIStroke", Main); Stroke.Color = Color3.fromRGB(255, 0, 0); Stroke.Thickness = 5

-- [[ COMPONENTES DA UI ]]
local Title = Instance.new("TextLabel", Main); Title.Size = UDim2.new(1, 0, 0, 40); Title.Text = "COELHO HUB V18"; Title.TextColor3 = Color3.fromRGB(255, 0, 0); Title.Font = Enum.Font.GothamBold; Title.BackgroundTransparency = 1
local Search = Instance.new("TextBox", Main); Search.Size = UDim2.new(1, -60, 0, 35); Search.Position = UDim2.new(0, 30, 0, 50); Search.PlaceholderText = "Pesquisar Função... 🔍"; Search.BackgroundColor3 = Color3.fromRGB(15, 15, 15); Search.TextColor3 = Color3.new(1,1,1); Instance.new("UICorner", Search)
local Homer = Instance.new("ImageLabel", Main); Homer.Size = UDim2.new(0, 410, 0, 180); Homer.Position = UDim2.new(0, 20, 0, 95); Homer.Image = "rbxassetid://13426742337"; Homer.BackgroundTransparency = 1
local Holder = Instance.new("ScrollingFrame", Main); Holder.Size = UDim2.new(1, -30, 1, -380); Holder.Position = UDim2.new(0, 15, 0, 290); Holder.BackgroundTransparency = 1; Holder.CanvasSize = UDim2.new(0, 0, 0, 1000); Instance.new("UIListLayout", Holder).Padding = UDim.new(0, 10)

-- [[ CRÉDITOS ]]
local CreditsFrame = Instance.new("Frame", Main); CreditsFrame.Size = UDim2.new(1, -30, 0, 65); CreditsFrame.Position = UDim2.new(0, 15, 1, -80); CreditsFrame.BackgroundColor3 = Color3.fromRGB(15, 2, 2); Instance.new("UICorner", CreditsFrame)
local CreditsText = Instance.new("TextLabel", CreditsFrame); CreditsText.Size = UDim2.new(1, 0, 1, 0); CreditsText.BackgroundTransparency = 1; CreditsText.TextColor3 = Color3.new(1,1,1); CreditsText.TextSize = 12; CreditsText.Text = "OWNER: COELHO | DEV HELP: MISTER DEV\nSTATUS: INTANKÁVEL V18"

-- [[ FUNÇÕES DE BOTÃO ]]
local function CreateToggle(text, key)
    local B = Instance.new("TextButton", Holder); B.Size = UDim2.new(1, -10, 0, 45); B.BackgroundColor3 = Color3.fromRGB(20, 20, 20); B.Text = text; B.TextColor3 = Color3.new(1,1,1); B.Font = Enum.Font.GothamBold; Instance.new("UICorner", B)
    B.MouseButton1Click:Connect(function() _G.Config[key] = not _G.Config[key]; B.BackgroundColor3 = _G.Config[key] and Color3.fromRGB(200, 0, 0) or Color3.fromRGB(20, 20, 20) end)
end
CreateToggle("AUTO FARM LEVEL (LOGICA NOVA)", "FarmAtivo")
CreateToggle("AUTO STATS (MELEE)", "StatsAtivo")

-- [[ O CÉREBRO: LÓGICA DE MISSÕES (MUNDO 1) ]]
task.spawn(function()
    while task.wait(0.5) do
        if _G.Config.FarmAtivo then
            pcall(function()
                local MyLevel = player.Data.Level.Value
                local Mon, LevelQuest, NameQuest, NameMon, CFrameQuest, CFrameMon
                local World1 = true -- Definido como true para ativar sua lógica

                if World1 then
                    if ((MyLevel == 1) or (MyLevel <= 9)) then
                        Mon = "Bandit"; LevelQuest = 1; NameQuest = "BanditQuest1"; NameMon = "Bandit"; CFrameQuest = CFrame.new(1059, 17, 1546); CFrameMon = CFrame.new(943, 45, 1562)
                    elseif ((MyLevel == 10) or (MyLevel <= 14)) then
                        Mon = "Monkey"; LevelQuest = 1; NameQuest = "JungleQuest"; NameMon = "Monkey"; CFrameQuest = CFrame.new(-1598, 37, 153); CFrameMon = CFrame.new(-1524, 50, 37)
                    elseif ((MyLevel == 15) or (MyLevel <= 29)) then
                        Mon = "Gorilla"; LevelQuest = 2; NameQuest = "JungleQuest"; NameMon = "Gorilla"; CFrameQuest = CFrame.new(-1598, 37, 153); CFrameMon = CFrame.new(-1128, 40, -451)
                    elseif ((MyLevel == 30) or (MyLevel <= 39)) then
                        Mon = "Pirate"; LevelQuest = 1; NameQuest = "BuggyQuest1"; NameMon = "Pirate"; CFrameQuest = CFrame.new(-1140, 4, 3829); CFrameMon = CFrame.new(-1262, 40, 3905)
                    elseif ((MyLevel == 40) or (MyLevel <= 59)) then
                        Mon = "Brute"; LevelQuest = 2; NameQuest = "BuggyQuest1"; NameMon = "Brute"; CFrameQuest = CFrame.new(-1140, 4, 3829); CFrameMon = CFrame.new(-976, 55, 4304)
                    elseif ((MyLevel == 60) or (MyLevel <= 74)) then
                        Mon = "Desert Bandit"; LevelQuest = 1; NameQuest = "DesertQuest"; NameMon = "Desert Bandit"; CFrameQuest = CFrame.new(897, 6, 4389); CFrameMon = CFrame.new(924, 7, 4482)
                    elseif ((MyLevel == 75) or (MyLevel <= 89)) then
                        Mon = "Desert Officer"; LevelQuest = 2; NameQuest = "DesertQuest"; NameMon = "Desert Officer"; CFrameQuest = CFrame.new(897, 6, 4389); CFrameMon = CFrame.new(1608, 9, 4371)
                    elseif ((MyLevel == 90) or (MyLevel <= 99)) then
                        Mon = "Snow Bandit"; LevelQuest = 1; NameQuest = "SnowQuest"; NameMon = "Snow Bandit"; CFrameQuest = CFrame.new(1385, 87, -1298); CFrameMon = CFrame.new(1362, 120, -1531)
                    elseif ((MyLevel == 100) or (MyLevel <= 119)) then
                        Mon = "Snowman"; LevelQuest = 2; NameQuest = "SnowQuest"; NameMon = "Snowman"; CFrameQuest = CFrame.new(1385, 87, -1298); CFrameMon = CFrame.new(1243, 140, -1437)
                    elseif ((MyLevel == 120) or (MyLevel <= 149)) then
                        Mon = "Chief Petty Officer"; LevelQuest = 1; NameQuest = "MarineQuest2"; NameMon = "Chief Petty Officer"; CFrameQuest = CFrame.new(-5035, 29, 4326); CFrameMon = CFrame.new(-4881, 23, 4274)
                    elseif ((MyLevel == 150) or (MyLevel <= 174)) then
                        Mon = "Sky Bandit"; LevelQuest = 1; NameQuest = "SkyQuest"; NameMon = "Sky Bandit"; CFrameQuest = CFrame.new(-4844, 718, -2621); CFrameMon = CFrame.new(-4953, 296, -2899)
                    elseif ((MyLevel == 175) or (MyLevel <= 189)) then
                        Mon = "Dark Master"; LevelQuest = 2; NameQuest = "SkyQuest"; NameMon = "Dark Master"; CFrameQuest = CFrame.new(-4844, 718, -2621); CFrameMon = CFrame.new(-5260, 391, -2229)
                    elseif ((MyLevel == 190) or (MyLevel <= 209)) then
                        Mon = "Prisoner"; LevelQuest = 1; NameQuest = "PrisonerQuest"; NameMon = "Prisoner"; CFrameQuest = CFrame.new(5306, 2, 477); CFrameMon = CFrame.new(5099, 0, 474)
                    elseif ((MyLevel == 210) or (MyLevel <= 249)) then
                        Mon = "Dangerous Prisoner"; LevelQuest = 2; NameQuest = "PrisonerQuest"; NameMon = "Dangerous Prisoner"; CFrameQuest = CFrame.new(5306, 2, 477); CFrameMon = CFrame.new(5655, 16, 866)
                    end
                end

                -- EXECUÇÃO DO FARM
                if not player.PlayerGui.Main.Quest.Visible then
                    player.Character.HumanoidRootPart.CFrame = CFrameQuest
                    task.wait(0.5)
                    CommF:InvokeServer("StartQuest", NameQuest, LevelQuest)
                else
                    for _, e in pairs(game.Workspace.Enemies:GetChildren()) do
                        if e.Name == Mon and e:FindFirstChild("HumanoidRootPart") and e.Humanoid.Health > 0 then
                            player.Character.HumanoidRootPart.CFrame = e.HumanoidRootPart.CFrame * CFrame.new(0, 10, 0)
                            RS.Modules.Net["RE/RegisterAttack"]:FireServer(0)
                        end
                    end
                end
            end)
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

-- [[ ANIMAÇÃO DE LOADING ]]
local LoadUI = Instance.new("ScreenGui", CoreGui); local LBg = Instance.new("Frame", LoadUI); LBg.Size = UDim2.new(0, 320, 0, 85); LBg.Position = UDim2.new(0.5, -160, 0.5, -42); LBg.BackgroundColor3 = Color3.fromRGB(5, 5, 5); Instance.new("UICorner", LBg)
local Fill = Instance.new("Frame", LBg); Fill.Size = UDim2.new(0, 0, 1, 0); Fill.BackgroundColor3 = Color3.fromRGB(255, 0, 0); Instance.new("UICorner", Fill)
TS:Create(Fill, TweenInfo.new(5), {Size = UDim2.new(1, -50, 0, 8)}):Play(); task.delay(5, function() LoadUI:Destroy(); Main.Visible = true end)

end
-- [[ COELHO HUB V18 - INTERFACE FINAL COM CRÉDITOS E ESTILO ]]

local CoreGui = game:GetService("CoreGui")
local TS = game:GetService("TweenService")

-- [[ LIMPEZA ]]
if CoreGui:FindFirstChild("CoelhoHubBeta") then CoreGui.CoelhoHubBeta:Destroy() end

-- [[ SCREEN GUI ]]
local UI = Instance.new("ScreenGui", CoreGui)
UI.Name = "CoelhoHubBeta"

-- [[ FRAME PRINCIPAL ]]
local Main = Instance.new("Frame", UI)
Main.Name = "MainFrame"
Main.Size = UDim2.new(0, 450, 0, 680)
Main.Position = UDim2.new(0.5, -225, 0.5, -340)
Main.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
Main.Active = true
Main.Draggable = true
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 15)

-- BORDA NEON VERMELHA GROSSA (MARCA REGISTRADA)
local Stroke = Instance.new("UIStroke", Main)
Stroke.Color = Color3.fromRGB(255, 0, 0)
Stroke.Thickness = 5

-- TÍTULO DO HUB
local Title = Instance.new("TextLabel", Main)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.Position = UDim2.new(0, 0, 0, 10)
Title.Text = "COELHO HUB V18"
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 22
Title.BackgroundTransparency = 1

-- BARRA DE PESQUISA 🔍
local Search = Instance.new("TextBox", Main)
Search.Name = "SearchBox"
Search.Size = UDim2.new(1, -60, 0, 35)
Search.Position = UDim2.new(0, 30, 0, 50)
Search.PlaceholderText = "Pesquisar Função... 🔍"
Search.PlaceholderColor3 = Color3.fromRGB(100, 0, 0)
Search.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
Search.TextColor3 = Color3.new(1, 1, 1)
Search.Font = Enum.Font.GothamBold
Search.Text = ""
Instance.new("UICorner", Search).CornerRadius = UDim.new(0, 8)
local sStroke = Instance.new("UIStroke", Search)
sStroke.Color = Color3.fromRGB(200, 0, 0)
sStroke.Thickness = 1.5

-- FOTO DO HOMER (O BRABO)
local Homer = Instance.new("ImageLabel", Main)
Homer.Size = UDim2.new(0, 410, 0, 180)
Homer.Position = UDim2.new(0, 20, 0, 95)
Homer.Image = "rbxassetid://13426742337"
Homer.BackgroundTransparency = 1
Instance.new("UICorner", Homer)

-- CONTAINER DE BOTÕES (SCROLL)
local Holder = Instance.new("ScrollingFrame", Main)
Holder.Size = UDim2.new(1, -30, 1, -380)
Holder.Position = UDim2.new(0, 15, 0, 290)
Holder.BackgroundTransparency = 1
Holder.ScrollBarThickness = 3
Holder.CanvasSize = UDim2.new(0, 0, 0, 1000)
Instance.new("UIListLayout", Holder).Padding = UDim.new(0, 10)

-- [[ SEÇÃO DE CRÉDITOS INTEGRADA ]]
local CreditsFrame = Instance.new("Frame", Main)
CreditsFrame.Name = "CreditsSection"
CreditsFrame.Size = UDim2.new(1, -30, 0, 65)
CreditsFrame.Position = UDim2.new(0, 15, 1, -80)
CreditsFrame.BackgroundColor3 = Color3.fromRGB(15, 2, 2)
Instance.new("UICorner", CreditsFrame)
local cStroke = Instance.new("UIStroke", CreditsFrame)
cStroke.Color = Color3.fromRGB(150, 0, 0)
cStroke.Thickness = 2

local CreditsText = Instance.new("TextLabel", CreditsFrame)
CreditsText.Size = UDim2.new(1, 0, 1, 0)
CreditsText.BackgroundTransparency = 1
CreditsText.TextColor3 = Color3.fromRGB(255, 255, 255)
CreditsText.Font = Enum.Font.Code
CreditsText.TextSize = 14
CreditsText.Text = "atack e missões =eclipse hub        creator=coelho      infos : @café-k3y"

-- [[ FUNÇÃO DE FILTRO DA PESQUISA ]]
Search:GetPropertyChangedSignal("Text"):Connect(function()
    local input = Search.Text:lower()
    for _, item in pairs(Holder:GetChildren()) do
        if item:IsA("TextButton") then
            item.Visible = item.Text:lower():find(input) and true or false
        end
    end
end)

-- [[ ANIMAÇÃO DE ENTRADA (FADE IN) ]]
Main.GroupTransparency = 1
local CanvasGroup = Instance.new("CanvasGroup", UI) -- Opcional para efeito de fade
Main.Parent = CanvasGroup
CanvasGroup.Size = UDim2.new(1,0,1,0)
CanvasGroup.BackgroundTransparency = 1
TS:Create(CanvasGroup, TweenInfo.new(0.5), {GroupTransparency = 0}):Play()

print("Coelho Hub: UI com Créditos carregada com sucesso! 🕶️🔥🐇")
