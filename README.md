-- [[ COELHO HUB BETA - FULL UNIFIED VERSION ]]
local player = game.Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")

-- VARIÁVEIS DE CONTROLE
local farmLevel = false
local farmSaw = false
local autoFruit = false

-- 1. SEU ATAQUE PROFISSIONAL (LINK DO GITHUB)
pcall(function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/lleoneomessicr7brainrotlabubu-crypto/atack-coelho-hub/refs/heads/main/README.md"))()
end)

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
