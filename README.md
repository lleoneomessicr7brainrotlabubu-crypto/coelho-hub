-- [[ COELHO HUB BETA - VERSÃO FINALIZADA ]]
local player = game.Players.LocalPlayer
local VirtualUser = game:GetService("VirtualUser")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local farmLevel = false

-- OTIMIZAÇÃO (DO SCRIPT DO DEV)
hookfunction(warn, function() end)
hookfunction(error, function() end)

-- DICIONÁRIO DE MISSÕES (TODAS AS ILHAS DO SEA 1)
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

-- [[ INTERFACE GRÁFICA - COELHO HUB BETA ]]
local ScreenGui = Instance.new("ScreenGui", player.PlayerGui)
ScreenGui.Name = "CoelhoHubBetaGUI"
ScreenGui.ResetOnSpawn = false

local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0, 230, 0, 160)
Main.Position = UDim2.new(0.5, -115, 0.5, -80)
Main.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
Main.Draggable = true
Main.Active = true
Instance.new("UICorner", Main).CornerRadius = UDim.new(0, 15)

-- Brilho na borda (Stroke)
local Stroke = Instance.new("UIStroke", Main)
Stroke.Color = Color3.fromRGB(150, 0, 0) -- Vermelho Coelho
Stroke.Thickness = 2

-- Título Centralizado
local Title = Instance.new("TextLabel", Main)
Title.Size = UDim2.new(1, 0, 0, 50)
Title.Text = "COELHO HUB BETA"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.GothamBold
Title.TextSize = 18

local BtnFarm = Instance.new("TextButton", Main)
BtnFarm.Size = UDim2.new(0, 190, 0, 55)
BtnFarm.Position = UDim2.new(0, 20, 0, 75)
BtnFarm.Text = "AUTO FARM: OFF"
BtnFarm.Font = Enum.Font.GothamBold
BtnFarm.TextColor3 = Color3.new(1, 1, 1)
BtnFarm.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Instance.new("UICorner", BtnFarm)

-- [[ FUNÇÕES DE SUPORTE ]]

local function voarPara(posicao)
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local p = player.Character.HumanoidRootPart
        local dist = (p.Position - posicao).Magnitude
        local speed = 300
        game:GetService("TweenService"):Create(p, TweenInfo.new(dist/speed, Enum.EasingStyle.Linear), {CFrame = CFrame.new(posicao)}):Play()
        task.wait(dist/speed)
    end
end

local function atacarETrazer(npcNome)
    for _, v in pairs(workspace.Enemies:GetChildren()) do
        if v.Name == npcNome and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
            v.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, -4)
            v.HumanoidRootPart.CanCollide = false
            VirtualUser:CaptureController()
            VirtualUser:ClickButton1(Vector2.new(9e9,9e9))
            local tool = player.Character:FindFirstChildOfClass("Tool")
            if tool then tool:Activate() end
        end
    end
end

-- [[ LÓGICA DE FARM COM DELAY LONGO ]]
local function gerenciarAutoFarm()
    while farmLevel do
        local lvl = player.Data.Level.Value
        local meuAlvo = ""

        -- Seleção por Nível (Sea 1)
        if lvl >= 625 then meuAlvo = "Galley Pirate"
        elseif lvl >= 450 then meuAlvo = "God's Guard"
        elseif lvl >= 375 then meuAlvo = "Fishman Warrior"
        elseif lvl >= 300 then meuAlvo = "Military Soldier"
        elseif lvl >= 250 then meuAlvo = "Toga Warrior"
        elseif lvl >= 190 then meuAlvo = "Prisoner"
        elseif lvl >= 175 then meuAlvo = "Dark Master"
        elseif lvl >= 150 then meuAlvo = "Sky Bandit"
        elseif lvl >= 120 then meuAlvo = "Chief Petty Officer"
        elseif lvl >= 100 then meuAlvo = "Snowman"
        elseif lvl >= 90 then meuAlvo = "Snow Bandits"
        elseif lvl >= 75 then meuAlvo = "Desert Officer"
        elseif lvl >= 60 then meuAlvo = "Desert Bandit"
        elseif lvl >= 40 then meuAlvo = "Brute"
        elseif lvl >= 30 then meuAlvo = "Pirate"
        elseif lvl >= 15 then meuAlvo = "Gorilla"
        elseif lvl >= 10 then meuAlvo = "Monkey"
        else meuAlvo = "Bandit" end

        local info = QuestData[meuAlvo]
        
        if not player.PlayerGui.Main.Quest.Visible then
            task.wait(5) -- Delay longo de segurança
            voarPara(info.Pos)
            task.wait(2)
            ReplicatedStorage.Remotes.CommF_:InvokeServer("StartQuest", info.Quest, info.ID)
            task.wait(1)
        end

        voarPara(info.Pos)
        local timer = tick()
        while farmLevel and tick() - timer < 15 and player.PlayerGui.Main.Quest.Visible do
            atacarETrazer(meuAlvo)
            task.wait(0.1)
        end
        task.wait(2) -- Respira entre ciclos
    end
end

-- Ativação do Botão
BtnFarm.MouseButton1Click:Connect(function()
    farmLevel = not farmLevel
    BtnFarm.Text = farmLevel and "AUTO FARM: ON" or "AUTO FARM: OFF"
    BtnFarm.BackgroundColor3 = farmLevel and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(30, 30, 30)
    if farmLevel then task.spawn(gerenciarAutoFarm) end
end)
