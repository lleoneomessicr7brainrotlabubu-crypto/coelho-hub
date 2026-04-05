-- [[ COELHO HUB LEGACY - FULL INTEGRATION ]]
-- Desenvolvido por: Coelho & Amor

-- 1. LIMPEZA DE GUI ANTIGA
if game.CoreGui:FindFirstChild("CoelhoLegacy") then
    game.CoreGui.CoelhoLegacy:Destroy()
end

local Players = game:GetService("Players")
local RS = game:GetService("ReplicatedStorage")
local TS = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local VirtualInputManager = game:GetService("VirtualInputManager")

local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local root = char:WaitForChild("HumanoidRootPart")

-- 2. CONFIGURAÇÕES GLOBAIS
_G.Config = {
    FarmLevel = false,
    BringMob = false,
    DistanciaFarm = 10,
}

-- 3. BANCO DE DADOS DE MISSÕES E COORDENADAS (MAPEADO POR VOCÊ)
local QuestData = {
    ["Bandit"] = {
        Quest = "BanditQuest1", Pos = Vector3.new(1059, 15, 1549), Level = 0, ID = 1,
        Spawns = {
            Vector3.new(943, 23, 1630), Vector3.new(1020, 21, 1566), Vector3.new(927, 22, 1516),
            Vector3.new(1237, 24, 1538), Vector3.new(1283, 26, 1624), Vector3.new(1219, 24, 1675),
            Vector3.new(1124, 27, 1661)
        }
    },
    ["Monkey"] = {
        Quest = "MonkeyQuest1", Pos = Vector3.new(-1598, 36, 155), Level = 10, ID = 1,
        Spawns = {
            Vector3.new(-1201, 20, 282), Vector3.new(-1496, 31, 76), Vector3.new(-1614, 29, 365),
            Vector3.new(-1796, 29, 111), Vector3.new(-1745, 27, -90), Vector3.new(-1602, 29, -46)
        }
    },
    ["Pirate"] = {
        Quest = "BuggyQuest1", Pos = Vector3.new(-1141, 4, 3828), Level = 30, ID = 1,
        Spawns = {
            Vector3.new(-1143, 12, 3902), Vector3.new(-1183, 13, 3973), 
            Vector3.new(-1268, 15, 3853), Vector3.new(-968, 22, 3941)
        }
    },
    ["Brute"] = {
        Quest = "BuggyQuest1", Pos = Vector3.new(-1141, 4, 3828), Level = 40, ID = 2,
        Spawns = {
            Vector3.new(-956, 27, 4225), Vector3.new(-1230, 27, 4331), Vector3.new(-1390, 27, 4187)
        }
    },
    ["Desert Bandit"] = {
        Quest = "DesertQuest", Pos = Vector3.new(894, 6, 4388), Level = 60, ID = 1,
        Spawns = {
            Vector3.new(940, 20, 4429), Vector3.new(1003, 13, 4491),
            Vector3.new(935, 17, 4532), Vector3.new(858, 16, 4485)
        }
    },
    ["Snow Bandit"] = {
        Quest = "SnowQuest", Pos = Vector3.new(1389, 7, -1298), Level = 90, ID = 1,
        Spawns = {
            Vector3.new(1198, 96, -1330), Vector3.new(1277, 97, -1348), 
            Vector3.new(1313, 99, -1395), Vector3.new(1383, 96, -1466), 
            Vector3.new(1459, 104, -1449)
        }
    },
    ["Snowman"] = {
        Quest = "SnowQuest", Pos = Vector3.new(1389, 7, -1298), Level = 100, ID = 2,
        Spawns = {
            Vector3.new(1263, 115, -1484), Vector3.new(1145, 117, -1429), 
            Vector3.new(1036, 116, -1489), Vector3.new(1187, 119, -1627)
        }
    }
}

-- 4. SEU SISTEMA DE ATAQUE PERSONALIZADO (INTEGRADO)
local Modules = RS:WaitForChild("Modules")
local Net = Modules:WaitForChild("Net")
local RegisterAttack = Net:WaitForChild("RE/RegisterAttack")
local RegisterHit = Net:WaitForChild("RE/RegisterHit")

local FastAttack = {}
FastAttack.__index = FastAttack

function FastAttack.new()
    local self = setmetatable({Debounce = 0}, FastAttack)
    pcall(function()
        self.HitFunction = getsenv(player.PlayerScripts:FindFirstChildOfClass("LocalScript"))._G.SendHitsToServer
    end)
    return self
end

function FastAttack:Attack()
    if (tick() - self.Debounce) < 0.2 then return end
    local Character = player.Character
    if not (Character and Character:FindFirstChild("Humanoid") and Character.Humanoid.Health > 0) then return end
    
    local bladehits = {}
    for _, v in pairs(Workspace.Enemies:GetChildren()) do
        if v:FindFirstChild("HumanoidRootPart") and (v.HumanoidRootPart.Position - root.Position).Magnitude <= 65 then
            table.insert(bladehits, v)
        end
    end

    if #bladehits > 0 then
        RegisterAttack:FireServer(0)
        local args = {[1] = bladehits[1].Head, [2] = {}}
        for i, v in pairs(bladehits) do
            args[2][i] = {v, v.HumanoidRootPart}
        end
        RegisterHit:FireServer(unpack(args))
        self.Debounce = tick()
    end
end

local AttackInstance = FastAttack.new()

-- 5. FUNÇÕES DO HUB (TWEEN E QUEST)
local function TweenTo(pos)
    local dist = (root.Position - pos).Magnitude
    local info = TweenInfo.new(dist/300, Enum.EasingStyle.Linear)
    local tween = TS:Create(root, info, {CFrame = CFrame.new(pos)})
    tween:Play()
    return tween
end

local function GetQuest(info)
    if (root.Position - info.Pos).Magnitude > 20 then
        local tw = TweenTo(info.Pos)
        tw.Completed:Wait()
    end
    task.wait(0.5)
    RS.Remotes.CommF_:InvokeServer("StartQuest", info.Quest, info.ID)
end

-- 6. INTERFACE (GUI)
local Gui = Instance.new("ScreenGui", game.CoreGui)
Gui.Name = "CoelhoLegacy"

local Main = Instance.new("Frame", Gui)
Main.Size, Main.Position = UDim2.new(0, 300, 0, 200), UDim2.new(0.5, -150, 0.5, -100)
Main.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
Main.Active, Main.Draggable = true, true
Instance.new("UICorner", Main)
Instance.new("UIStroke", Main).Color = Color3.fromRGB(255, 0, 0)

local Title = Instance.new("TextLabel", Main)
Title.Text = "COELHO HUB LEGACY"
Title.Size = UDim2.new(1, 0, 0, 40)
Title.TextColor3 = Color3.new(1,1,1)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.GothamBold

local function CreateToggle(txt, pos, cfg)
    local b = Instance.new("TextButton", Main)
    b.Size, b.Position = UDim2.new(0.8, 0, 0, 40), UDim2.new(0.1, 0, 0, pos)
    b.Text = txt .. ": OFF"
    b.BackgroundColor3, b.TextColor3 = Color3.fromRGB(30, 30, 30), Color3.new(1,1,1)
    Instance.new("UICorner", b)
    b.MouseButton1Click:Connect(function()
        _G.Config[cfg] = not _G.Config[cfg]
        b.Text = txt .. ": " .. (_G.Config[cfg] and "ON" or "OFF")
        b.BackgroundColor3 = _G.Config[cfg] and Color3.fromRGB(150, 0, 0) or Color3.fromRGB(30, 30, 30)
    end)
end

CreateToggle("AUTO FARM", 50, "FarmLevel")
CreateToggle("BRING MOB", 100, "BringMob")

-- 7. LOOP MESTRE (AUTOMAÇÃO)
RunService.Stepped:Connect(function()
    if _G.Config.FarmLevel then AttackInstance:Attack() end
end)

task.spawn(function()
    while task.wait(0.1) do
        if _G.Config.FarmLevel then
            local lvl = player.Data.Level.Value
            local currentEnemy = "Bandit"
            
            -- Lógica de Level
            if lvl >= 10 and lvl < 30 then currentEnemy = "Monkey"
            elseif lvl >= 30 and lvl < 40 then currentEnemy = "Pirate"
            elseif lvl >= 40 and lvl < 60 then currentEnemy = "Brute"
            elseif lvl >= 60 and lvl < 90 then currentEnemy = "Desert Bandit"
            elseif lvl >= 90 and lvl < 100 then currentEnemy = "Snow Bandit"
            elseif lvl >= 100 then currentEnemy = "Snowman" end
            
            local info = QuestData[currentEnemy]
            
            if not player.PlayerGui.Main.Quest.Visible then
                GetQuest(info)
            else
                -- Bring Mob
                if _G.Config.BringMob then
                    for _, v in pairs(Workspace.Enemies:GetChildren()) do
                        if v.Name == currentEnemy and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
                            v.HumanoidRootPart.CFrame = root.CFrame * CFrame.new(0, 0, -5)
                            v.HumanoidRootPart.CanCollide = false
                        end
                    end
                end
                
                -- Farm nas Coordenadas
                local randomPos = info.Spawns[math.random(#info.Spawns)]
                root.CFrame = CFrame.new(randomPos) * CFrame.new(0, _G.Config.DistanciaFarm, 0)
            end
        end
    end
end)
