-- [[ COELHO HUB LEGACY - EMERGENCY FIX ]]
local Players = game:GetService("Players")
local RS = game:GetService("ReplicatedStorage")
local UIS = game:GetService("UserInputService")

local player = Players.LocalPlayer
-- Espera o personagem carregar para não dar erro de "nil"
local char = player.Character or player.CharacterAdded:Wait()
local root = char:WaitForChild("HumanoidRootPart")

_G.Config = { Farm = false, Saw = false, Fruit = false, AutoSaber = false }

-- [[ ATAQUE COM PROTEÇÃO CONTRA ERROS ]]
local function SafeAttack()
    local success, err = pcall(function()
        local Net = RS:FindFirstChild("Modules") and RS.Modules:FindFirstChild("Net")
        if not Net then return end
        
        local targets = {}
        for _, g in pairs({workspace.Enemies, workspace.Characters}) do
            for _, v in pairs(g:GetChildren()) do
                if v ~= player.Character and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
                    local hrp = v:FindFirstChild("HumanoidRootPart")
                    if hrp and (hrp.Position - player.Character.HumanoidRootPart.Position).Magnitude <= 70 then
                        table.insert(targets, v)
                    end
                end
            end
        end

        if #targets > 0 then
            Net:WaitForChild("RE/RegisterAttack"):FireServer(0)
            local args = {[1] = targets[1].Head or targets[1].HumanoidRootPart, [2] = {}}
            for i, v in pairs(targets) do args[2][i] = {v, v.HumanoidRootPart} end
            Net:WaitForChild("RE/RegisterHit"):FireServer(unpack(args))
        end
    end)
    if not success then warn("Erro no Ataque: " .. err) end
end

-- [[ GUI SIMPLIFICADA PARA TESTE DE CARREGAMENTO ]]
local ScreenGui = Instance.new("ScreenGui", player.PlayerGui)
ScreenGui.Name = "CoelhoFix"

local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0, 400, 0, 250)
Main.Position = UDim2.new(0.5, -200, 0.5, -125)
Main.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
Main.Active = true
Main.Draggable = true
Instance.new("UIStroke", Main).Color = Color3.fromRGB(255, 0, 0)

local Title = Instance.new("TextLabel", Main)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.Text = "COELHO LEGACY - MODO SEGURANÇA"
Title.TextColor3 = Color3.new(1,0,0)
Title.BackgroundTransparency = 1

local Btn = Instance.new("TextButton", Main)
Btn.Size = UDim2.new(0.8, 0, 0, 50)
Btn.Position = UDim2.new(0.1, 0, 0.4, 0)
Btn.Text = "TESTAR ATAQUE: OFF"
Btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
Btn.TextColor3 = Color3.new(1,1,1)

Btn.MouseButton1Click:Connect(function()
    _G.Config.Farm = not _G.Config.Farm
    Btn.Text = "TESTAR ATAQUE: " .. (_G.Config.Farm and "ON" or "OFF")
end)

-- Loop de Segurança
task.spawn(function()
    while task.wait(0.1) do
        if _G.Config.Farm then SafeAttack() end
    end
end)
