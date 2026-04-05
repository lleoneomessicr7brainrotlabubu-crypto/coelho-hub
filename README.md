-- [[ COELHO HUB V18 - O INTANKÁVEL (EDIÇÃO BETA) ]]

local Players = game:GetService("Players")
local RS = game:GetService("ReplicatedStorage")
local TS = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")
local player = Players.LocalPlayer
local CommF = RS:WaitForChild("Remotes"):WaitForChild("CommF_")

-- [[ DATABASE DE MISSÕES ]]
local AllQuests = {
    {Level = 0, Mob = "Bandit", Quest = "BanditQuest1", ID = 1, NPC = Vector3.new(1059, 15, 1549)},
    {Level = 10, Mob = "Monkey", Quest = "MonkeyQuest1", ID = 1, NPC = Vector3.new(-1599, 36, 151)},
    {Level = 15, Mob = "Gorilla", Quest = "MonkeyQuest1", ID = 2, NPC = Vector3.new(-1599, 36, 151)},
    {Level = 30, Mob = "Pirate", Quest = "PirateQuest1", ID = 1, NPC = Vector3.new(-1141, 4, 3832)},
    {Level = 60, Mob = "Desert Bandit", Quest = "DesertQuest", ID = 1, NPC = Vector3.new(894, 6, 4390)},
    {Level = 90, Mob = "Snow Bandit", Quest = "SnowQuest", ID = 1, NPC = Vector3.new(1386, 15, -1322)},
    {Level = 190, Mob = "Prisoner", Quest = "PrisonerQuest", ID = 1, NPC = Vector3.new(5308, 1, 485)},
    {Level = 250, Mob = "Fishman Warrior", Quest = "FishmanQuest", ID = 1, NPC = Vector3.new(61122, 18, 1565)},
    {Level = 300, Mob = "Military Soldier", Quest = "MagmaQuest", ID = 1, NPC = Vector3.new(-5313, 12, 8515)},
    {Level = 450, Mob = "God's Guard", Quest = "SkyExpQuest1", ID = 1, NPC = Vector3.new(-4721, 845, -1953)},
    {Level = 625, Mob = "Galley Pirate", Quest = "FountainQuest", ID = 1, NPC = Vector3.new(5135, 4, 4007)}
}

-- [[ CONFIGURAÇÃO INICIAL ]]
_G.Config = {
    FarmAtivo = false,
    StatsAtivo = false,
    RandomAtivo = false,
    StoreAtivo = false,
    TeleportFruit = false
}

-- [[ LIMPEZA DE GUI ANTIGA ]]
if game.CoreGui:FindFirstChild("CoelhoHubBeta") then game.CoreGui.CoelhoHubBeta:Destroy() end

-- [[ SISTEMA DE GUI ]]
local UI = Instance.new("ScreenGui", game.CoreGui)
UI.Name = "CoelhoHubBeta"

local Main = Instance.new("Frame", UI)
Main.Name = "MainFrame"
Main.Size = UDim2.new(0, 450, 0, 650) -- LARGA E ALTA
Main.Position = UDim2.new(0.5, -225, 0.5, -325)
Main.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
Main.Active, Main.Draggable = true, true
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 15)

-- BORDA VERMELHA NEON
local Stroke = Instance.new("UIStroke", Main)
Stroke.Color, Stroke.Thickness = Color3.fromRGB(255, 0, 0), 3.5

-- TÍTULO
local Title = Instance.new("TextLabel", Main)
Title.Size, Title.Position = UDim2.new(1, 0, 0, 40), UDim2.new(0, 0, 0, 10)
Title.Text, Title.TextColor3 = "COELHO HUB BETA", Color3.fromRGB(255, 0, 0)
Title.Font, Title.TextSize, Title.BackgroundTransparency = Enum.Font.GothamBold, 22, 1

-- FOTO DO HOMER
local Homer = Instance.new("ImageLabel", Main)
Homer.Size, Homer.Position = UDim2.new(1, -40, 0, 180), UDim2.new(0, 20, 0, 55)
Homer.Image = "rbxassetid://13426742337"
Homer.BackgroundTransparency = 1
Instance.new("UICorner", Homer).CornerRadius = UDim.new(0, 10)

-- CONTAINER DE BOTÕES
local Holder = Instance.new("ScrollingFrame", Main)
Holder.Size, Holder.Position = UDim2.new(1, -30, 1, -270), UDim2.new(0, 15, 0, 250)
Holder.BackgroundTransparency, Holder.ScrollBarThickness = 1, 3
Holder.CanvasSize = UDim2.new(0, 0, 0, 1100)
Instance.new("UIListLayout", Holder).Padding = UDim.new(0, 10)

-- [[ FUNÇÕES DE CRIAÇÃO ]]
local function CreateToggle(text, configKey)
    local B = Instance.new("TextButton", Holder)
    B.Size, B.BackgroundColor3 = UDim2.new(1, -10, 0, 45), Color3.fromRGB(15, 15, 15)
    B.Text, B.TextColor3, B.Font, B.TextSize = text, Color3.new(1, 1, 1), Enum.Font.GothamBold, 15
    Instance.new("UICorner", B)

    B.MouseButton1Click:Connect(function()
        _G.Config[configKey] = not _G.Config[configKey]
        B.BackgroundColor3 = _G.Config[configKey] and Color3.fromRGB(200, 0, 0) or Color3.fromRGB(15, 15, 15)
    end)
end

local function CreateAction(text, func)
    local B = Instance.new("TextButton", Holder)
    B.Size, B.BackgroundColor3 = UDim2.new(1, -10, 0, 45), Color3.fromRGB(30, 30, 30)
    B.Text, B.TextColor3, B.Font, B.TextSize = text, Color3.new(1, 1, 1), Enum.Font.GothamBold, 15
    Instance.new("UICorner", B)
    B.MouseButton1Click:Connect(func)
end

-- [[ ADICIONANDO BOTÕES ]]
CreateToggle("AUTO FARM LEVEL", "FarmAtivo")
CreateToggle("AUTO STATS (MELEE)", "StatsAtivo")
CreateToggle("AUTO RANDOM FRUIT", "RandomAtivo")
CreateToggle("AUTO STORE FRUIT", "StoreAtivo")
CreateToggle("TELEPORT TO FRUITS", "TeleportFruit")

CreateAction("AUTO SABER (PUZZLE)", function()
    local Botoes = {Vector3.new(-1612, 37, 147), Vector3.new(-1526, 37, 201), Vector3.new(-1403, 34, 30), Vector3.new(-1610, 52, -180), Vector3.new(-1653, 34, -36)}
    for _, p in pairs(Botoes) do player.Character.HumanoidRootPart.CFrame = CFrame.new(p); task.wait(0.5) end
    CommF:InvokeServer("ProQuestProgress", "RichMan")
end)

CreateAction("BUY HAKI/GEPPO/SORU", function()
    CommF:InvokeServer("BuyHaki", "Skyjump")
    CommF:InvokeServer("BuyHaki", "Buso")
    CommF:InvokeServer("BuyHaki", "FlashStep")
end)

CreateAction("TRAVEL TO SEA 2", function()
    if player.Data.Level.Value >= 700 then
        CommF:InvokeServer("DressAndBoats", "Detective", 1)
        task.wait(2); CommF:InvokeServer("TravelMain")
    end
end)

CreateAction("FECHAR PAINEL", function() UI:Destroy() end)

-- [[ LOOPS DE EXECUÇÃO ]]
task.spawn(function()
    while task.wait(0.5) do
        pcall(function()
            if _G.Config.FarmAtivo then
                local lvl = player.Data.Level.Value
                local data = AllQuests[1]
                for i = #AllQuests, 1, -1 do if lvl >= AllQuests[i].Level then data = AllQuests[i]; break end end
                
                if not player.PlayerGui.Main.Quest.Visible then
                    player.Character.HumanoidRootPart.CFrame = CFrame.new(data.NPC)
                    CommF:InvokeServer("StartQuest", data.Quest, data.ID)
                else
                    for _, e in pairs(game.Workspace.Enemies:GetChildren()) do
                        if e.Name == data.Mob and e:FindFirstChild("HumanoidRootPart") and e.Humanoid.Health > 0 then
                            player.Character.HumanoidRootPart.CFrame = e.HumanoidRootPart.CFrame * CFrame.new(0, 10, 0)
                            game:GetService("ReplicatedStorage").Modules.Net["RE/RegisterAttack"]:FireServer(0)
                        end
                    end
                end
            end
            if _G.Config.StatsAtivo then
                local p = player.Data.Points.Value
                if p > 0 then CommF:InvokeServer("AddPoint", "Melee", p) end
            end
            if _G.Config.RandomAtivo then CommF:InvokeServer("Cousin", "Buy") end
        end)
    end
end)
-- [[ COELHO HUB - LOADING DESIGN LINHA GROSSA ]]
local TS = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")

-- 1. IDENTIFICAR A GUI PRINCIPAL PARA ESCONDER
local MainUI = CoreGui:FindFirstChild("CoelhoHubBeta")
local MainFrame = MainUI and MainUI:FindFirstChild("MainFrame")

if MainFrame then
    MainFrame.Visible = false
    -- Aproveita para engrossar a borda da GUI principal também
    local MainStroke = MainFrame:FindFirstChildOfClass("UIStroke")
    if MainStroke then MainStroke.Thickness = 5 end 
end

-- 2. CRIAR A INTERFACE
local LoadUI = Instance.new("ScreenGui", CoreGui)
LoadUI.Name = "CoelhoLoadingBar"

local Bg = Instance.new("Frame", LoadUI)
Bg.Size = UDim2.new(0, 320, 0, 85)
Bg.Position = UDim2.new(0.5, -160, 0.5, -42)
Bg.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
Bg.BorderSizePixel = 0
Instance.new("UICorner", Bg).CornerRadius = UDim.new(0, 10)

-- [[ LINHA VERMELHA MAIS GROSSA (THICKNESS 5) ]]
local Stroke = Instance.new("UIStroke", Bg)
Stroke.Color = Color3.fromRGB(255, 0, 0)
Stroke.Thickness = 5 -- Linha bem grossa e visível
Stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

-- TEXTO
local TextLabel = Instance.new("TextLabel", Bg)
TextLabel.Size = UDim2.new(1, 0, 0, 35)
TextLabel.Position = UDim2.new(0, 0, 0, 12)
TextLabel.Text = "PREPARANDO COELHO HUB"
TextLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TextLabel.Font = Enum.Font.GothamBold
TextLabel.TextSize = 15
TextLabel.BackgroundTransparency = 1

-- BARRINHA
local BarBg = Instance.new("Frame", Bg)
BarBg.Size = UDim2.new(1, -50, 0, 8)
BarBg.Position = UDim2.new(0, 25, 0.7, 3)
BarBg.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Instance.new("UICorner", BarBg)

local Fill = Instance.new("Frame", BarBg)
Fill.Size = UDim2.new(0, 0, 1, 0)
Fill.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
Instance.new("UICorner", Fill)

-- 3. ANIMAÇÃO DE 5 SEGUNDOS
local Tween = TS:Create(Fill, TweenInfo.new(5, Enum.EasingStyle.Quad), {Size = UDim2.new(1, 0, 1, 0)})
Tween:Play()

Tween.Completed:Connect(function()
    task.wait(0.2)
    LoadUI:Destroy() -- DELETA A BARRINHA TOTALMENTE
    
    task.wait(1) -- 1 SEGUNDO DE TELA LIMPA (CONFORME PEDIDO)
    
    if MainFrame then
        MainFrame.Visible = true
        print("Coelho Hub: Borda Grossa Ativada!")
    end
end)
-- [[ COELHO HUB - SISTEMA DE PESQUISA FILTRADA ]]
local CoreGui = game:GetService("CoreGui")

local function IniciarPesquisa()
    local UI = CoreGui:FindFirstChild("CoelhoHubBeta")
    local Main = UI and UI:FindFirstChild("MainFrame")
    local Holder = Main and Main:FindFirstChildOfClass("ScrollingFrame")

    if not Main or not Holder then 
        warn("Erro: Execute a GUI principal primeiro!") 
        return 
    end

    -- 1. CRIAR A BARRA DE PESQUISA (ESTILO PRETO E VERMELHO)
    local SearchBox = Instance.new("Pesquisar Função... 🔍", Main)
    SearchBox.Name = "Pesquisar Função... 🔍"
    SearchBox.Size = UDim2.new(1, -60, 0, 35)
    SearchBox.Position = UDim2.new(0, 30, 0, 50) -- Fica abaixo do Título
    SearchBox.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    SearchBox.Text = ""
    SearchBox.PlaceholderText = "Pesquisar Função... 🔍"
    SearchBox.PlaceholderColor3 = Color3.fromRGB(150, 0, 0)
    SearchBox.TextColor3 = Color3.new(1, 1, 1)
    SearchBox.Font = Enum.Font.GothamBold
    SearchBox.TextSize = 14
    SearchBox.ClipsDescendants = true
    
    Instance.new("UICorner", SearchBox).CornerRadius = UDim.new(0, 8)
    local Stroke = Instance.new("UIStroke", SearchBox)
    Stroke.Color, Stroke.Thickness = Color3.fromRGB(200, 0, 0), 1.5

    -- 2. AJUSTAR POSIÇÃO DOS OUTROS ITENS (PARA NÃO ATROPELAR)
    -- Move o Homer um pouco para baixo para dar espaço à pesquisa
    local Homer = Main:FindFirstChildOfClass("ImageLabel")
    if Homer then
        Homer.Position = UDim2.new(0, 20, 0, 95)
    end
    -- Ajusta o Container de botões
    Holder.Position = UDim2.new(0, 15, 0, 290)
    Holder.Size = UDim2.new(1, -30, 1, -310)

    -- 3. LÓGICA DE FILTRO EM TEMPO REAL
    SearchBox:GetPropertyChangedSignal("Text"):Connect(function()
        local Input = SearchBox.Text:lower()
        
        for _, Item in pairs(Holder:GetChildren()) do
            if Item:IsA("TextButton") then
                local ButtonText = Item.Text:lower()
                
                if string.find(ButtonText, Input) then
                    Item.Visible = true
                else
                    Item.Visible = false
                end
            end
        end
    end)
    
    print("Pesquisa do Coelho Hub: Ativada! 🔍🐇")
end
-- [[ COELHO HUB V18 - O INTANKÁVEL (EDIÇÃO BETA) ]]

local Players = game:GetService("Players")
local RS = game:GetService("ReplicatedStorage")
local TS = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")
local player = Players.LocalPlayer
local CommF = RS:WaitForChild("Remotes"):WaitForChild("CommF_")

-- [[ DATABASE DE MISSÕES ]]
local AllQuests = {
    {Level = 0, Mob = "Bandit", Quest = "BanditQuest1", ID = 1, NPC = Vector3.new(1059, 15, 1549)},
    {Level = 10, Mob = "Monkey", Quest = "MonkeyQuest1", ID = 1, NPC = Vector3.new(-1599, 36, 151)},
    {Level = 15, Mob = "Gorilla", Quest = "MonkeyQuest1", ID = 2, NPC = Vector3.new(-1599, 36, 151)},
    {Level = 30, Mob = "Pirate", Quest = "PirateQuest1", ID = 1, NPC = Vector3.new(-1141, 4, 3832)},
    {Level = 60, Mob = "Desert Bandit", Quest = "DesertQuest", ID = 1, NPC = Vector3.new(894, 6, 4390)},
    {Level = 90, Mob = "Snow Bandit", Quest = "SnowQuest", ID = 1, NPC = Vector3.new(1386, 15, -1322)},
    {Level = 190, Mob = "Prisoner", Quest = "PrisonerQuest", ID = 1, NPC = Vector3.new(5308, 1, 485)},
    {Level = 250, Mob = "Fishman Warrior", Quest = "FishmanQuest", ID = 1, NPC = Vector3.new(61122, 18, 1565)},
    {Level = 300, Mob = "Military Soldier", Quest = "MagmaQuest", ID = 1, NPC = Vector3.new(-5313, 12, 8515)},
    {Level = 450, Mob = "God's Guard", Quest = "SkyExpQuest1", ID = 1, NPC = Vector3.new(-4721, 845, -1953)},
    {Level = 625, Mob = "Galley Pirate", Quest = "FountainQuest", ID = 1, NPC = Vector3.new(5135, 4, 4007)}
}

-- [[ CONFIGURAÇÃO INICIAL ]]
_G.Config = {
    FarmAtivo = false,
    StatsAtivo = false,
    RandomAtivo = false,
    StoreAtivo = false,
    TeleportFruit = false
}

-- [[ LIMPEZA DE GUI ANTIGA ]]
if game.CoreGui:FindFirstChild("CoelhoHubBeta") then game.CoreGui.CoelhoHubBeta:Destroy() end

-- [[ SISTEMA DE GUI ]]
local UI = Instance.new("ScreenGui", game.CoreGui)
UI.Name = "CoelhoHubBeta"

local Main = Instance.new("Frame", UI)
Main.Name = "MainFrame"
Main.Size = UDim2.new(0, 450, 0, 650) -- LARGA E ALTA
Main.Position = UDim2.new(0.5, -225, 0.5, -325)
Main.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
Main.Active, Main.Draggable = true, true
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 15)

-- BORDA VERMELHA NEON
local Stroke = Instance.new("UIStroke", Main)
Stroke.Color, Stroke.Thickness = Color3.fromRGB(255, 0, 0), 3.5

-- TÍTULO
local Title = Instance.new("TextLabel", Main)
Title.Size, Title.Position = UDim2.new(1, 0, 0, 40), UDim2.new(0, 0, 0, 10)
Title.Text, Title.TextColor3 = "COELHO HUB BETA", Color3.fromRGB(255, 0, 0)
Title.Font, Title.TextSize, Title.BackgroundTransparency = Enum.Font.GothamBold, 22, 1

-- FOTO DO HOMER
local Homer = Instance.new("ImageLabel", Main)
Homer.Size, Homer.Position = UDim2.new(1, -40, 0, 180), UDim2.new(0, 20, 0, 55)
Homer.Image = "rbxassetid://13426742337"
Homer.BackgroundTransparency = 1
Instance.new("UICorner", Homer).CornerRadius = UDim.new(0, 10)

-- CONTAINER DE BOTÕES
local Holder = Instance.new("ScrollingFrame", Main)
Holder.Size, Holder.Position = UDim2.new(1, -30, 1, -270), UDim2.new(0, 15, 0, 250)
Holder.BackgroundTransparency, Holder.ScrollBarThickness = 1, 3
Holder.CanvasSize = UDim2.new(0, 0, 0, 1100)
Instance.new("UIListLayout", Holder).Padding = UDim.new(0, 10)

-- [[ FUNÇÕES DE CRIAÇÃO ]]
local function CreateToggle(text, configKey)
    local B = Instance.new("TextButton", Holder)
    B.Size, B.BackgroundColor3 = UDim2.new(1, -10, 0, 45), Color3.fromRGB(15, 15, 15)
    B.Text, B.TextColor3, B.Font, B.TextSize = text, Color3.new(1, 1, 1), Enum.Font.GothamBold, 15
    Instance.new("UICorner", B)

    B.MouseButton1Click:Connect(function()
        _G.Config[configKey] = not _G.Config[configKey]
        B.BackgroundColor3 = _G.Config[configKey] and Color3.fromRGB(200, 0, 0) or Color3.fromRGB(15, 15, 15)
    end)
end

local function CreateAction(text, func)
    local B = Instance.new("TextButton", Holder)
    B.Size, B.BackgroundColor3 = UDim2.new(1, -10, 0, 45), Color3.fromRGB(30, 30, 30)
    B.Text, B.TextColor3, B.Font, B.TextSize = text, Color3.new(1, 1, 1), Enum.Font.GothamBold, 15
    Instance.new("UICorner", B)
    B.MouseButton1Click:Connect(func)
end

-- [[ ADICIONANDO BOTÕES ]]
CreateToggle("AUTO FARM LEVEL", "FarmAtivo")
CreateToggle("AUTO STATS (MELEE)", "StatsAtivo")
CreateToggle("AUTO RANDOM FRUIT", "RandomAtivo")
CreateToggle("AUTO STORE FRUIT", "StoreAtivo")
CreateToggle("TELEPORT TO FRUITS", "TeleportFruit")

CreateAction("AUTO SABER (PUZZLE)", function()
    local Botoes = {Vector3.new(-1612, 37, 147), Vector3.new(-1526, 37, 201), Vector3.new(-1403, 34, 30), Vector3.new(-1610, 52, -180), Vector3.new(-1653, 34, -36)}
    for _, p in pairs(Botoes) do player.Character.HumanoidRootPart.CFrame = CFrame.new(p); task.wait(0.5) end
    CommF:InvokeServer("ProQuestProgress", "RichMan")
end)

CreateAction("BUY HAKI/GEPPO/SORU", function()
    CommF:InvokeServer("BuyHaki", "Skyjump")
    CommF:InvokeServer("BuyHaki", "Buso")
    CommF:InvokeServer("BuyHaki", "FlashStep")
end)

CreateAction("TRAVEL TO SEA 2", function()
    if player.Data.Level.Value >= 700 then
        CommF:InvokeServer("DressAndBoats", "Detective", 1)
        task.wait(2); CommF:InvokeServer("TravelMain")
    end
end)

-- [[ LOOPS DE EXECUÇÃO ]]
task.spawn(function()
    while task.wait(0.5) do
        pcall(function()
            if _G.Config.FarmAtivo then
                local lvl = player.Data.Level.Value
                local data = AllQuests[1]
                for i = #AllQuests, 1, -1 do if lvl >= AllQuests[i].Level then data = AllQuests[i]; break end end
                
                if not player.PlayerGui.Main.Quest.Visible then
                    player.Character.HumanoidRootPart.CFrame = CFrame.new(data.NPC)
                    CommF:InvokeServer("StartQuest", data.Quest, data.ID)
                else
                    for _, e in pairs(game.Workspace.Enemies:GetChildren()) do
                        if e.Name == data.Mob and e:FindFirstChild("HumanoidRootPart") and e.Humanoid.Health > 0 then
                            player.Character.HumanoidRootPart.CFrame = e.HumanoidRootPart.CFrame * CFrame.new(0, 10, 0)
                            game:GetService("ReplicatedStorage").Modules.Net["RE/RegisterAttack"]:FireServer(0)

                            MyLevel = game:GetService("Players").LocalPlayer.Data.Level.Value;
	if World1 then
		if ((MyLevel == 1) or (MyLevel <= 9)) then
			Mon = "Bandit";
			LevelQuest = 1;
			NameQuest = "BanditQuest1";
			NameMon = "Bandit";
			CFrameQuest = CFrame.new(1059, 17, 1546);
			CFrameMon = CFrame.new(943, 45, 1562);
		elseif ((MyLevel == 10) or (MyLevel <= 14)) then
			Mon = "Monkey";
			LevelQuest = 1;
			NameQuest = "JungleQuest";
			NameMon = "Monkey";
			CFrameQuest = CFrame.new( -1598, 37, 153);
			CFrameMon = CFrame.new( -1524, 50, 37);
		elseif ((MyLevel == 15) or (MyLevel <= 29)) then
			Mon = "Gorilla";
			LevelQuest = 2;
			NameQuest = "JungleQuest";
			NameMon = "Gorilla";
			CFrameQuest = CFrame.new( -1598, 37, 153);
			CFrameMon = CFrame.new( -1128, 40, -451);
		elseif ((MyLevel == 30) or (MyLevel <= 39)) then
			Mon = "Pirate";
			LevelQuest = 1;
			NameQuest = "BuggyQuest1";
			NameMon = "Pirate";
			CFrameQuest = CFrame.new( -1140, 4, 3829);
			CFrameMon = CFrame.new( -1262, 40, 3905);
		elseif ((MyLevel == 40) or (MyLevel <= 59)) then
			Mon = "Brute";
			LevelQuest = 2;
			NameQuest = "BuggyQuest1";
			NameMon = "Brute";
			CFrameQuest = CFrame.new( -1140, 4, 3829);
			CFrameMon = CFrame.new( -976, 55, 4304);
		elseif ((MyLevel == 60) or (MyLevel <= 74)) then
			Mon = "Desert Bandit";
			LevelQuest = 1;
			NameQuest = "DesertQuest";
			NameMon = "Desert Bandit";
			CFrameQuest = CFrame.new(897, 6, 4389);
			CFrameMon = CFrame.new(924, 7, 4482);
		elseif ((MyLevel == 75) or (MyLevel <= 89)) then
			Mon = "Desert Officer";
			LevelQuest = 2;
			NameQuest = "DesertQuest";
			NameMon = "Desert Officer";
			CFrameQuest = CFrame.new(897, 6, 4389);
			CFrameMon = CFrame.new(1608, 9, 4371);
		elseif ((MyLevel == 90) or (MyLevel <= 99)) then
			Mon = "Snow Bandit";
			LevelQuest = 1;
			NameQuest = "SnowQuest";
			NameMon = "Snow Bandit";
			CFrameQuest = CFrame.new(1385, 87, -1298);
			CFrameMon = CFrame.new(1362, 120, -1531);
		elseif ((MyLevel == 100) or (MyLevel <= 119)) then
			Mon = "Snowman";
			LevelQuest = 2;
			NameQuest = "SnowQuest";
			NameMon = "Snowman";
			CFrameQuest = CFrame.new(1385, 87, -1298);
			CFrameMon = CFrame.new(1243, 140, -1437);
		elseif ((MyLevel == 120) or (MyLevel <= 149)) then
			Mon = "Chief Petty Officer";
			LevelQuest = 1;
			NameQuest = "MarineQuest2";
			NameMon = "Chief Petty Officer";
			CFrameQuest = CFrame.new( -5035, 29, 4326);
			CFrameMon = CFrame.new( -4881, 23, 4274);
		elseif ((MyLevel == 150) or (MyLevel <= 174)) then
			Mon = "Sky Bandit";
			LevelQuest = 1;
			NameQuest = "SkyQuest";
			NameMon = "Sky Bandit";
			CFrameQuest = CFrame.new( -4844, 718, -2621);
			CFrameMon = CFrame.new( -4953, 296, -2899);
		elseif ((MyLevel == 175) or (MyLevel <= 189)) then
			Mon = "Dark Master";
			LevelQuest = 2;
			NameQuest = "SkyQuest";
			NameMon = "Dark Master";
			CFrameQuest = CFrame.new( -4844, 718, -2621);
			CFrameMon = CFrame.new( -5260, 391, -2229);
		elseif ((MyLevel == 190) or (MyLevel <= 209)) then
			Mon = "Prisoner";
			LevelQuest = 1;
			NameQuest = "PrisonerQuest";
			NameMon = "Prisoner";
			CFrameQuest = CFrame.new(5306, 2, 477);
			CFrameMon = CFrame.new(5099, "-0", 474);
		elseif ((MyLevel == 210) or (MyLevel <= 249)) then
			Mon = "Dangerous Prisoner";
			LevelQuest = 2;
			NameQuest = "PrisonerQuest";
			NameMon = "Dangerous Prisoner";
			CFrameQuest = CFrame.new(5306, 2, 477);
			CFrameMon = CFrame.new(5655, 16, 866);
                        end
                    end
                end
            end
            if _G.Config.StatsAtivo then
                local p = player.Data.Points.Value
                if p > 0 then CommF:InvokeServer("AddPoint", "Melee", p) end
            end
            if _G.Config.RandomAtivo then CommF:InvokeServer("Cousin", "Buy") end
        end)
    end
end)
-- [[ COELHO HUB - LOADING DESIGN LINHA GROSSA ]]
local TS = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")

-- 1. IDENTIFICAR A GUI PRINCIPAL PARA ESCONDER
local MainUI = CoreGui:FindFirstChild("CoelhoHubBeta")
local MainFrame = MainUI and MainUI:FindFirstChild("MainFrame")

if MainFrame then
    MainFrame.Visible = false
    -- Aproveita para engrossar a borda da GUI principal também
    local MainStroke = MainFrame:FindFirstChildOfClass("UIStroke")
    if MainStroke then MainStroke.Thickness = 5 end 
end

-- 2. CRIAR A INTERFACE
local LoadUI = Instance.new("ScreenGui", CoreGui)
LoadUI.Name = "CoelhoLoadingBar"

local Bg = Instance.new("Frame", LoadUI)
Bg.Size = UDim2.new(0, 320, 0, 85)
Bg.Position = UDim2.new(0.5, -160, 0.5, -42)
Bg.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
Bg.BorderSizePixel = 0
Instance.new("UICorner", Bg).CornerRadius = UDim.new(0, 10)

-- [[ LINHA VERMELHA MAIS GROSSA (THICKNESS 5) ]]
local Stroke = Instance.new("UIStroke", Bg)
Stroke.Color = Color3.fromRGB(255, 0, 0)
Stroke.Thickness = 5 -- Linha bem grossa e visível
Stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

-- TEXTO
local TextLabel = Instance.new("TextLabel", Bg)
TextLabel.Size = UDim2.new(1, 0, 0, 35)
TextLabel.Position = UDim2.new(0, 0, 0, 12)
TextLabel.Text = "PREPARANDO COELHO HUB"
TextLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TextLabel.Font = Enum.Font.GothamBold
TextLabel.TextSize = 15
TextLabel.BackgroundTransparency = 1

-- BARRINHA
local BarBg = Instance.new("Frame", Bg)
BarBg.Size = UDim2.new(1, -50, 0, 8)
BarBg.Position = UDim2.new(0, 25, 0.7, 3)
BarBg.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Instance.new("UICorner", BarBg)

local Fill = Instance.new("Frame", BarBg)
Fill.Size = UDim2.new(0, 0, 1, 0)
Fill.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
Instance.new("UICorner", Fill)

-- 3. ANIMAÇÃO DE 5 SEGUNDOS
local Tween = TS:Create(Fill, TweenInfo.new(5, Enum.EasingStyle.Quad), {Size = UDim2.new(1, 0, 1, 0)})
Tween:Play()

Tween.Completed:Connect(function()
    task.wait(0.2)
    LoadUI:Destroy() -- DELETA A BARRINHA TOTALMENTE
    
    task.wait(1) -- 1 SEGUNDO DE TELA LIMPA (CONFORME PEDIDO)
    
    if MainFrame then
        MainFrame.Visible = true
        print("Coelho Hub: Borda Grossa Ativada!")
    end
end)
-- [[ COELHO HUB - SISTEMA DE PESQUISA FILTRADA ]]
local CoreGui = game:GetService("CoreGui")

local function IniciarPesquisa()
    local UI = CoreGui:FindFirstChild("CoelhoHubBeta")
    local Main = UI and UI:FindFirstChild("MainFrame")
    local Holder = Main and Main:FindFirstChildOfClass("ScrollingFrame")

    if not Main or not Holder then 
        warn("Erro: Execute a GUI principal primeiro!") 
        return 
    end

    -- 1. CRIAR A BARRA DE PESQUISA (ESTILO PRETO E VERMELHO)
    local SearchBox = Instance.new("Pesquisar Função... 🔍", Main)
    SearchBox.Name = "Pesquisar Função... 🔍"
    SearchBox.Size = UDim2.new(1, -60, 0, 35)
    SearchBox.Position = UDim2.new(0, 30, 0, 50) -- Fica abaixo do Título
    SearchBox.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    SearchBox.Text = ""
    SearchBox.PlaceholderText = "Pesquisar Função... 🔍"
    SearchBox.PlaceholderColor3 = Color3.fromRGB(150, 0, 0)
    SearchBox.TextColor3 = Color3.new(1, 1, 1)
    SearchBox.Font = Enum.Font.GothamBold
    SearchBox.TextSize = 14
    SearchBox.ClipsDescendants = true
    
    Instance.new("UICorner", SearchBox).CornerRadius = UDim.new(0, 8)
    local Stroke = Instance.new("UIStroke", SearchBox)
    Stroke.Color, Stroke.Thickness = Color3.fromRGB(200, 0, 0), 1.5

    -- 2. AJUSTAR POSIÇÃO DOS OUTROS ITENS (PARA NÃO ATROPELAR)
    -- Move o Homer um pouco para baixo para dar espaço à pesquisa
    local Homer = Main:FindFirstChildOfClass("ImageLabel")
    if Homer then
        Homer.Position = UDim2.new(0, 20, 0, 95)
    end
    -- Ajusta o Container de botões
    Holder.Position = UDim2.new(0, 15, 0, 290)
    Holder.Size = UDim2.new(1, -30, 1, -310)

    -- 3. LÓGICA DE FILTRO EM TEMPO REAL
    SearchBox:GetPropertyChangedSignal("Text"):Connect(function()
        local Input = SearchBox.Text:lower()
        
        for _, Item in pairs(Holder:GetChildren()) do
            if Item:IsA("TextButton") then
                local ButtonText = Item.Text:lower()
                
                if string.find(ButtonText, Input) then
                    Item.Visible = true
                else
                    Item.Visible = false
                end
            end
        end
    end)
    
    print("Pesquisa do Coelho Hub: Ativada! 🔍🐇")
end
-- [[ COELHO HUB V18 - SABER PUZZLE COMPLETO (ALL-IN-ONE) ]]
-- ORDEM: PLACAS -> TOCHA -> COPO -> SICKMAN -> RICHMAN -> BANDIDO

local Players = game:GetService("Players")
local RS = game:GetService("ReplicatedStorage")
local player = Players.LocalPlayer
local CommF = RS:WaitForChild("Remotes"):WaitForChild("CommF_")

local function ExecutarSaberPuzzleTotal()
    local root = player.Character:WaitForChild("HumanoidRootPart")
    
    local function VoarPara(pos, msg) 
        print("Saber Puzzle: " .. msg)
        root.CFrame = CFrame.new(pos) 
        task.wait(2) -- Tempo de segurança para o mapa carregar
    end

    -- --- ETAPA 1: AS 5 PLACAS DA JUNGLE ---
    VoarPara(Vector3.new(-1598, 35, 153), "Placa 1/5 - Jungle")
    VoarPara(Vector3.new(-1421, 48, 22), "Placa 2/5 - Jungle")
    VoarPara(Vector3.new(-1181, 21, 189), "Placa 3/5 - Jungle")
    VoarPara(Vector3.new(-1651, 23, 438), "Placa 4/5 - Jungle")
    VoarPara(Vector3.new(-1325, 30, -463), "Placa 5/5 - Gorilla")

    -- --- ETAPA 2: DESERTO (TOCHA E COPO) ---
    VoarPara(Vector3.new(1112, 5, 4367), "Buscando Tocha no Deserto")
    task.wait(1)
    VoarPara(Vector3.new(1114, 5, 4363), "Queimando porta e pegando Copo")
    
    -- --- ETAPA 3: GELO (ENCHER O COPO) ---
    VoarPara(Vector3.new(1397, 37, -1299), "Enchendo Copo na Goteira (Gelo)")
    task.wait(2)

    -- --- ETAPA 4: AJUDAR O SICKMAN (PAI DO RICHMAN) ---
    VoarPara(Vector3.new(1389, 15, -1273), "Entregando água para o Sickman")
    CommF:InvokeServer("EmptyCup", "SickMan")
    task.wait(1)

    -- --- ETAPA 5: CONVERSAR COM O RICHMAN (FILHO) ---
    VoarPara(Vector3.new(-1104, 5, 4308), "Falando com o Richman (Vila Pirata)")
    CommF:InvokeServer("ProQuestProgress", "RichMan")
    task.wait(1)

    -- --- ETAPA 6: O BANDIDO (MOBIUS) ---
    VoarPara(Vector3.new(-1104, 5, 4308), "Indo para a caverna do Bandido Mobius")
    
    print("------------------------------------------")
    print("Saber Puzzle Finalizado! 🕶️🔥")
    print("Agora derrote o Boss para ganhar a Relíquia!")
    print("------------------------------------------")
end
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
-- Inicia o processo automático
ExecutarSaberPuzzleTotal()

-- Executa a função

-- Executa a função
IniciarPesquisa()
