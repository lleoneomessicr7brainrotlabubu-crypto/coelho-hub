-- [[ COELHO HUB LEGACY - ORGANIZED VERSION ]]
local Players = game:GetService("Players")
local RS = game:GetService("ReplicatedStorage")
local UIS = game:GetService("UserInputService")

local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local root = char:WaitForChild("HumanoidRootPart")

-- ESTADOS DO SCRIPT
_G.Config = {
    Farm = false,
    Saw = false,
    Fruit = false,
    AutoSaber = false,
    Visible = true
}

-- [[ 1. SISTEMA DE ATAQUE (FAST ATTACK) ]]
local Net = RS:WaitForChild("Modules"):WaitForChild("Net")
local RegAtk, RegHit = Net:WaitForChild("RE/RegisterAttack"), Net:WaitForChild("RE/RegisterHit")

local function Attack()
    local targets = {}
    for _, g in pairs({workspace.Enemies, workspace.Characters}) do
        for _, v in pairs(g:GetChildren()) do
            if v ~= char and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
                local hrp = v:FindFirstChild("HumanoidRootPart")
                if hrp and (hrp.Position - root.Position).Magnitude <= 75 then
                    table.insert(targets, v)
                end
            end
        end
    end
    if #targets > 0 then
        RegAtk:FireServer(0)
        local args = {[1] = targets[1].Head or targets[1].HumanoidRootPart, [2] = {}}
        for i, v in pairs(targets) do args[2][i] = {v, v.HumanoidRootPart} end
        RegHit:FireServer(unpack(args))
    end
end

-- [[ 2. LÓGICA DO AUTO SABER ]]
local function SolveSaber()
    local saber = workspace.Enemies:FindFirstChild("Saber Expert")
    if saber and saber:FindFirstChild("HumanoidRootPart") and saber.Humanoid.Health > 0 then
        root.CFrame = saber.HumanoidRootPart.CFrame * CFrame.new(0, 0, 5)
    else
        -- Aqui entra a sequência de placas da Jungle que configuramos
        root.CFrame = CFrame.new(-1583, 35, 7) -- Exemplo: Primeira Placa
    end
end

-- [[ 3. INTERFACE (GUI) ]]
local ScreenGui = Instance.new("ScreenGui", player.PlayerGui)
ScreenGui.Name = "CoelhoHubLegacy"
ScreenGui.ResetOnSpawn = false

local Main = Instance.new("Frame", ScreenGui)
Main.Size, Main.Position = UDim2.new(0, 480, 0, 320), UDim2.new(0.5, -240, 0.5, -160)
Main.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
Main.Active, Main.Draggable = true, true
Instance.new("UICorner", Main)
Instance.new("UIStroke", Main).Color = Color3.fromRGB(255, 0, 0)

-- Ocultar (RightControl)
UIS.InputBegan:Connect(function(i, p)
    if not p and i.KeyCode == Enum.KeyCode.RightControl then Main.Visible = not Main.Visible end
end)

-- Sidebar
local Side = Instance.new("Frame", Main)
Side.Size, Side.BackgroundColor3 = UDim2.new(0, 140, 1, 0), Color3.fromRGB(18, 18, 18)
Instance.new("UICorner", Side)

local Title = Instance.new("TextLabel", Side)
Title.Size, Title.Text = UDim2.new(1, 0, 0, 50), "COELHO LEGACY"
Title.TextColor3, Title.Font, Title.TextSize = Color3.new(1,0,0), Enum.Font.GothamBold, 16
Title.BackgroundTransparency = 1

-- Containers
local Content = Instance.new("Frame", Main)
Content.Size, Content.Position = UDim2.new(1, -150, 1, -10), UDim2.new(0, 145, 0, 5)
Content.BackgroundTransparency = 1

local Pages = {
    Main = Instance.new("ScrollingFrame", Content),
    Items = Instance.new("ScrollingFrame", Content),
    Shop = Instance.new("ScrollingFrame", Content),
    Fruit = Instance.new("ScrollingFrame", Content)
}

for _, p in pairs(Pages) do 
    p.Size, p.BackgroundTransparency, p.Visible, p.ScrollBarThickness = UDim2.new(1,0,1,0), 1, false, 0 
end
Pages.Main.Visible = true

-- Navegação
local function Tab(txt, pos, page)
    local b = Instance.new("TextButton", Side)
    b.Size, b.Position = UDim2.new(0.9, 0, 0, 35), UDim2.new(0.05, 0, 0, pos)
    b.Text, b.BackgroundColor3, b.TextColor3 = txt, Color3.fromRGB(30, 30, 30), Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    Instance.new("UICorner", b)
    b.MouseButton1Click:Connect(function()
        for _, p in pairs(Pages) do p.Visible = false end
        Pages[page].Visible = true
    end)
end

Tab("Main", 60, "Main")
Tab("Itens & Missões", 100, "Items")
Tab("Shop", 140, "Shop")
Tab("Fruit", 180, "Fruit")

-- Botões Utilitários
local function AddToggle(txt, page, pos, cfg)
    local b = Instance.new("TextButton", Pages[page])
    b.Size, b.Position = UDim2.new(0.9, 0, 0, 45), UDim2.new(0.05, 0, 0, pos)
    b.Text = txt .. ": OFF"
    b.BackgroundColor3, b.TextColor3 = Color3.fromRGB(35, 35, 35), Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    Instance.new("UICorner", b)
    b.MouseButton1Click:Connect(function()
        _G.Config[cfg] = not _G.Config[cfg]
        b.Text = txt .. ": " .. (_G.Config[cfg] and "ON" or "OFF")
        b.BackgroundColor3 = _G.Config[cfg] and Color3.fromRGB(200, 0, 0) or Color3.fromRGB(35, 35, 35)
    end)
end

-- Configurando os Botões
AddToggle("AUTO FARM LEVEL", "Main", 10, "Farm")
AddToggle("AUTO FARM SAW", "Main", 65, "Saw")

-- AUTO SABER NA CATEGORIA CERTA!
AddToggle("AUTO SABER (QUEST)", "Items", 10, "AutoSaber")

AddToggle("AUTO RANDOM FRUIT", "Fruit", 10, "Fruit")

-- Botões do Shop
local function AddShop(txt, abi, pos)
    local b = Instance.new("TextButton", Pages.Shop)
    b.Size, b.Position = UDim2.new(0.9, 0, 0, 45), UDim2.new(0.05, 0, 0, pos)
    b.Text, b.BackgroundColor3 = txt, Color3.fromRGB(40, 40, 40)
    b.TextColor3, b.Font = Color3.new(1,1,1), Enum.Font.GothamBold
    Instance.new("UICorner", b)
    b.MouseButton1Click:Connect(function() RS.Remotes.CommF_:InvokeServer("BuyAbility", abi) end)
end

AddShop("Buy Geppo (10k)", "Skyjump", 10)
AddShop("Buy Haki (25k)", "Buso", 65)
AddShop("Buy Soru (100k)", "Step", 120)

-- [[ 4. LOOP MESTRE ]]
task.spawn(function()
    while task.wait(0.1) do
        if _G.Config.Farm or _G.Config.Saw or _G.Config.AutoSaber then Attack() end
        if _G.Config.AutoSaber then SolveSaber() end
    end
end)

print("Coelho Hub Legacy: Organizado com Sucesso!")
