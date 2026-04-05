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

-- ATALHO CONTROL
UIS.InputBegan:Connect(function(input, proc)
    if not proc and (input.KeyCode == Enum.KeyCode.LeftControl or input.KeyCode == Enum.KeyCode.RightControl) then
        Main.Visible = not Main.Visible
    end
end)
-- [[ COELHO HUB - LOADING CONTROLLER EXTERNO ]]
-- Execute este script JUNTO ou DEPOIS do script principal

local TS = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")

-- 1. IDENTIFICAR A GUI DO HOMER
-- Coloque aqui o nome exato que você deu para a ScreenGui no script principal
local GUIName = "CoelhoHubV18" 

local function IniciarLoading()
    local MainGUI = CoreGui:WaitForChild(GUIName, 10)
    
    if MainGUI then
        local MainFrame = MainGUI:FindFirstChildOfClass("Frame")
        if MainFrame then
            -- ESCONDE O PAINEL NA HORA
            MainFrame.Visible = false 
            
            -- 2. CRIA A BARRA DE CARREGAMENTO NA TELA
            local LoadUI = Instance.new("ScreenGui", CoreGui)
            LoadUI.Name = "TempLoading"

            local Bg = Instance.new("Frame", LoadUI)
            Bg.Size = UDim2.new(0, 320, 0, 90)
            Bg.Position = UDim2.new(0.5, -160, 0.5, -45)
            Bg.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
            Bg.BorderSizePixel = 0
            Instance.new("UICorner", Bg)
            Instance.new("UIStroke", Bg).Color = Color3.fromRGB(150, 0, 0)

            local Status = Instance.new("TextLabel", Bg)
            Status.Size = UDim2.new(1, 0, 0, 30)
            Status.Position = UDim2.new(0, 0, 0, 15)
            Status.Text = "INICIANDO COELHO HUB..."
            Status.TextColor3 = Color3.new(1, 1, 1)
            Status.Font = Enum.Font.GothamBold
            Status.TextSize = 14
            Status.BackgroundTransparency = 1

            local BarBack = Instance.new("Frame", Bg)
            BarBack.Size = UDim2.new(0, 260, 0, 10)
            BarBack.Position = UDim2.new(0.5, -130, 0, 60)
            BarBack.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
            BarBack.BorderSizePixel = 0
            Instance.new("UICorner", BarBack)

            local BarFill = Instance.new("Frame", BarBack)
            BarFill.Size = UDim2.new(0, 0, 1, 0)
            BarFill.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
            BarFill.BorderSizePixel = 0
            Instance.new("UICorner", BarFill)

            -- 3. ANIMAÇÃO DE 5 SEGUNDOS
            local Tween = TS:Create(BarFill, TweenInfo.new(5, Enum.EasingStyle.Linear), {Size = UDim2.new(1, 0, 1, 0)})
            Tween:Play()

            task.wait(2)
            Status.Text = "CARREGANDO SCRIPTS LUA..."
            task.wait(2)
            Status.Text = "HOMER ESTÁ CHEGANDO..."
            
            Tween.Completed:Wait()
            
            -- 4. FINALIZAÇÃO: MOSTRA O PAINEL PRINCIPAL
            LoadUI:Destroy()
            MainFrame.Visible = true
            print("Coelho Hub: Carregamento Concluído!")
        end
    else
        warn("Erro: O Script Principal não foi encontrado. Execute o principal primeiro!")
    end
end

task.spawn(IniciarLoading)

-- [[ COELHO HUB - SISTEMA DE LOADING SINCRONIZADO ]]
local TS = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")

-- 1. IDENTIFICAR A GUI PRINCIPAL
local MainUI = CoreGui:FindFirstChild("CoelhoHubBeta")
local MainFrame = MainUI and MainUI:FindFirstChild("MainFrame")

-- 2. ESCONDER A GUI IMEDIATAMENTE
if MainFrame then
    MainFrame.Visible = false
end

-- 3. CRIAR A INTERFACE DA BARRINHA
local LoadUI = Instance.new("ScreenGui", CoreGui)
LoadUI.Name = "CoelhoLoadingBar"

local Bg = Instance.new("Frame", LoadUI)
Bg.Size = UDim2.new(0, 350, 0, 70)
Bg.Position = UDim2.new(0.5, -175, 0.5, -35)
Bg.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
Instance.new("UICorner", Bg).CornerRadius = UDim.new(0, 10)
local Stroke = Instance.new("UIStroke", Bg)
Stroke.Color, Stroke.Thickness = Color3.fromRGB(255, 0, 0), 2

local BarBg = Instance.new("Frame", Bg)
BarBg.Size = UDim2.new(1, -40, 0, 10)
BarBg.Position = UDim2.new(0, 20, 0.7, 0)
BarBg.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Instance.new("UICorner", BarBg)

local BarFill = Instance.new("Frame", BarBg)
BarFill.Size = UDim2.new(0, 0, 1, 0)
BarFill.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
Instance.new("UICorner", BarFill)

local Status = Instance.new("TextLabel", Bg)
Status.Size = UDim2.new(1, 0, 0, 30)
Status.Position = UDim2.new(0, 0, 0, 10)
Status.Text = "SINCRONIZANDO COELHO HUB BETA..."
Status.TextColor3, Status.Font = Color3.new(1, 1, 1), Enum.Font.GothamBold
Status.TextSize, Status.BackgroundTransparency = 14, 1

-- 4. ANIMAÇÃO DE 5 SEGUNDOS
local Tween = TS:Create(BarFill, TweenInfo.new(5, Enum.EasingStyle.Linear), {Size = UDim2.new(1, 0, 1, 0)})
Tween:Play()

Tween.Completed:Connect(function()
    Status.Text = "CONCLUÍDO!"
    task.wait(0.5)
    
    -- 5. DESTRUIR A BARRINHA PRIMEIRO
    LoadUI:Destroy()
    
    -- 6. SÓ AGORA MOSTRAR A GUI PRINCIPAL
    if MainFrame then
        MainFrame.Visible = true
        print("Coelho Hub Beta: Reativado com sucesso!")
    else
        warn("Erro: GUI Principal não encontrada para reativar.")
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
    local SearchBox = Instance.new("TextBox", Main)
    SearchBox.Name = "SearchBar"
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

-- Executa a função
IniciarPesquisa()

-- No seu Script da GUI, dentro da função do botão:
CreateAction("AUTO SABER (PUZZLE)", function()
    _G.SaberQuestAtiva = true -- Liga a chave
    print("Missão Saber Quest Habilitada!")
    
    -- Chama a função da quest (que está no outro script)
    if IniciarSaberQuestPersonalizado then
        IniciarSaberQuestPersonalizado()
    end
end)

-- [[ COELHO HUB - SABER QUEST (APENAS COM ATIVAÇÃO) ]]
local Players = game:GetService("Players")
local RS = game:GetService("ReplicatedStorage")
local player = Players.LocalPlayer
local CommF = RS:WaitForChild("Remotes"):WaitForChild("CommF_")

-- [[ HITBOX SÓ FUNCIONA SE A MISSÃO ESTIVER ATIVA ]]
task.spawn(function()
    while task.wait(0.1) do
        -- SÓ RODA SE A VARIÁVEL FOR TRUE
        if _G.SaberQuestAtiva then
            pcall(function()
                for _, enemy in pairs(game.Workspace.Enemies:GetChildren()) do
                    -- Filtra apenas os alvos da missão
                    if enemy.Name == "Saber Expert" or enemy.Name == "Mob Leader" then
                        local hum = enemy:FindFirstChild("Humanoid")
                        local eRoot = enemy:FindFirstChild("HumanoidRootPart")
                        
                        if hum and eRoot and hum.Health > 0 then
                            -- Suas medidas: 15 up, 30 down, 24 sides
                            eRoot.Size = Vector3.new(48, 45, 48) 
                            eRoot.CanCollide = false
                            
                            -- Registro de Ataque
                            RS.Modules.Net["RE/RegisterAttack"]:FireServer(0)
                            RS.Modules.Net["RE/RegisterHit"]:FireServer(eRoot, {{enemy, eRoot}})
                        end
                    end
                end
            end)
        end
    end
end)

-- [[ FUNÇÃO PRINCIPAL DA QUEST ]]
function IniciarSaberQuestPersonalizado()
    -- Verificação de segurança: Se o botão não foi apertado, para aqui.
    if not _G.SaberQuestAtiva then return end

    local root = player.Character:WaitForChild("HumanoidRootPart")

    local function VoarPara(pos)
        root.CFrame = CFrame.new(pos)
        task.wait(2)
    end

    -- Início da Sequência que você dita:
    VoarPara(Vector3.new(-1421, 48, 22))
    VoarPara(Vector3.new(-1181, 21, 189))
    VoarPara(Vector3.new(-1651, 23, 438))
    VoarPara(Vector3.new(-1325, 30, -463))

    -- Puzzle Richman e Bandido
    CommF:InvokeServer("ProQuestProgress", "RichMan")
    VoarPara(Vector3.new(-1104, 5, 4308)) -- Local do Bandido
    
    -- Após derrotar e terminar o processo:
    -- VoarPara(Vector3.new(-1461, 25, -51)) -- Saber Expert
    
    -- Opcional: Desativar após concluir
    -- _G.SaberQuestAtiva = false
end
-- [[ COELHO HUB - FORCE WALLPAPER ]]
local CoreGui = game:GetService("CoreGui")

local function AplicarFundoDireto()
    local UI = CoreGui:FindFirstChild("CoelhoHubBeta")
    local Main = UI and UI:FindFirstChild("MainFrame")

    if not Main then 
        warn("ERRO: O Script Principal do Coelho Hub não foi encontrado!") 
        return 
    end

    -- 1. LIMPAR FUNDOS ANTIGOS
    if Main:FindFirstChild("HubWallpaper") then Main.HubWallpaper:Destroy() end
    Main.BackgroundTransparency = 1 -- Deixa o fundo preto invisível

    -- 2. CRIAR O NOVO FUNDO (IMAGEM 139820301456905)
    local Wallpaper = Instance.new("ImageLabel")
    Wallpaper.Name = "HubWallpaper"
    Wallpaper.Parent = Main
    Wallpaper.Size = UDim2.new(1, 0, 1, 0)
    Wallpaper.Position = UDim2.new(0, 0, 0, 0)
    
    -- FORMATO CORRETO DO ID
    Wallpaper.Image = "rbxassetid://139820301456905"
    
    -- CONFIGURAÇÕES DE CAMADA E ESTÉTICA
    Wallpaper.ZIndex = 0 -- Fica atrás de tudo
    Wallpaper.BackgroundTransparency = 1
    Wallpaper.ScaleType = Enum.ScaleType.Stretch
    
    -- Arredondamento
    local Corner = Instance.new("UICorner", Wallpaper)
    Corner.CornerRadius = UDim.new(0, 15)

    -- 3. FORÇAR ELEMENTOS PARA A FRENTE
    for _, item in pairs(Main:GetChildren()) do
        if item:IsA("ScrollingFrame") or item:IsA("TextButton") or item:IsA("TextLabel") or item:IsA("ImageLabel") then
            if item.Name ~= "HubWallpaper" then
                item.ZIndex = 10 -- Joga botões e textos para a camada 10 (Frente)
            end
        end
    end

    print("Coelho Hub: Imagem 139820301456905 aplicada com sucesso!")
end

-- Executa agora
AplicarFundoDireto()

-- [[ COELHO HUB - MODO CRÉDITOS (FULL SCREEN SWAP) ]]
local CoreGui = game:GetService("CoreGui")
local UI = CoreGui:FindFirstChild("CoelhoHubBeta")
local Main = UI and UI:FindFirstChild("MainFrame")

if not Main then 
    warn("Erro: Hub Principal não encontrado!") 
    return 
end

-- 1. LOCALIZAR O CONTAINER DE BOTÕES DE FARM
local BotoesComuns = Main:FindFirstChildOfClass("ScrollingFrame")

-- 2. CRIAR PÁGINA DE CRÉDITOS (INVISÍVEL POR PADRÃO)
local CreditsPage = Main:FindFirstChild("CreditsPage") or Instance.new("Frame", Main)
CreditsPage.Name = "CreditsPage"
CreditsPage.Size = UDim2.new(1, -20, 1, -80)
CreditsPage.Position = UDim2.new(0, 10, 0, 60)
CreditsPage.BackgroundTransparency = 1
CreditsPage.Visible = false
CreditsPage.ZIndex = 50

-- TEXTO EM VERMELHO COM ESPAÇAMENTO DE 2 VERSOS
local TextoInfo = CreditsPage:FindFirstChild("Info") or Instance.new("TextLabel", CreditsPage)
TextoInfo.Name = "Info"
TextoInfo.Size = UDim2.new(1, 0, 1, 0)
TextoInfo.BackgroundTransparency = 1
TextoInfo.TextColor3 = Color3.fromRGB(255, 0, 0) -- VERMELHO
TextoInfo.Font = Enum.Font.GothamBold
TextoInfo.TextSize = 18
TextoInfo.TextWrapped = true
-- Texto com as 2 linhas de distância ( \n\n\n )
TextoInfo.Text = "Atack: Eclipse Hub\n\n\nCreator: https://www.youtube.com/@café-k3y"
TextoInfo.ZIndex = 55

-- 3. BOTÃO EXIT (CANTO SUPERIOR ESQUERDO)
local ExitBtn = Main:FindFirstChild("ExitCredits") or Instance.new("TextButton", Main)
ExitBtn.Name = "ExitCredits"
ExitBtn.Size = UDim2.new(0, 80, 0, 30)
ExitBtn.Position = UDim2.new(0, 10, 0, 10) -- SUPERIOR ESQUERDO
ExitBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
ExitBtn.Text = "EXIT"
ExitBtn.TextColor3 = Color3.new(1, 1, 1)
ExitBtn.Font = Enum.Font.GothamBold
ExitBtn.Visible = false
ExitBtn.ZIndex = 60
Instance.new("UICorner", ExitBtn).CornerRadius = UDim.new(0, 6)

-- 4. BOTÃO CREDITS (CANTO SUPERIOR DIREITO - O GATILHO)
local CreditBtn = Main:FindFirstChild("CreditsButton") or Instance.new("TextButton", Main)
CreditBtn.Name = "CreditsButton"
CreditBtn.Size = UDim2.new(0, 100, 0, 30)
CreditBtn.Position = UDim2.new(1, -110, 0, 10) -- SUPERIOR DIREITO
CreditBtn.Text = "CREDITS"
CreditBtn.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
CreditBtn.TextColor3 = Color3.new(1, 1, 1)
CreditBtn.Font = Enum.Font.GothamBold
CreditBtn.ZIndex = 60
local Stroke = CreditBtn:FindFirstChildOfClass("UIStroke") or Instance.new("UIStroke", CreditBtn)
Stroke.Color = Color3.new(1, 0, 0)
Stroke.Thickness = 2

-- 5. LÓGICA DE TROCA DE TELAS
CreditBtn.MouseButton1Click:Connect(function()
    -- ESCONDE TUDO
    if BotoesComuns then BotoesComuns.Visible = false end
    CreditBtn.Visible = false
    
    -- MOSTRA CRÉDITOS
    CreditsPage.Visible = true
    ExitBtn.Visible = true
end)

ExitBtn.MouseButton1Click:Connect(function()
    -- VOLTA AO NORMAL
    if BotoesComuns then BotoesComuns.Visible = true end
    CreditBtn.Visible = true
    
    -- ESCONDE CRÉDITOS
    CreditsPage.Visible = false
    ExitBtn.Visible = false
end)

print("Coelho Hub: Sistema de Créditos Vermelhos ativado! 🕶️🔥🐇")

print("COELHO HUB BETA CARREGADO! 🕶️🔥🐇")
