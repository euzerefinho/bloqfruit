-- Criar GUI Simples para Mobile
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local FarmButton = Instance.new("TextButton")
local FruitButton = Instance.new("TextButton")
local ChestButton = Instance.new("TextButton")

ScreenGui.Parent = game.CoreGui

Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Frame.Size = UDim2.new(0, 200, 0, 150)
Frame.Position = UDim2.new(0.1, 0, 0.1, 0)

FarmButton.Parent = Frame
FarmButton.Text = "Auto Farm: OFF"
FarmButton.Size = UDim2.new(1, 0, 0.3, 0)
FarmButton.Position = UDim2.new(0, 0, 0, 0)
FarmButton.BackgroundColor3 = Color3.fromRGB(100, 100, 255)

FruitButton.Parent = Frame
FruitButton.Text = "Coletar Frutas: OFF"
FruitButton.Size = UDim2.new(1, 0, 0.3, 0)
FruitButton.Position = UDim2.new(0, 0, 0.35, 0)
FruitButton.BackgroundColor3 = Color3.fromRGB(100, 255, 100)

ChestButton.Parent = Frame
ChestButton.Text = "Coletar Baús: OFF"
ChestButton.Size = UDim2.new(1, 0, 0.3, 0)
ChestButton.Position = UDim2.new(0, 0, 0.7, 0)
ChestButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)

-- Variáveis de controle
local farmEnabled = false
local fruitEnabled = false
local chestEnabled = false

local player = game.Players.LocalPlayer
local workspace = game:GetService("Workspace")
local virtualUser = game:GetService("VirtualUser")

-- Evita ser kickado por inatividade
player.Idled:Connect(function()
    virtualUser:Button2Down(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
    wait(1)
    virtualUser:Button2Up(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
end)

-- Função para teleporte rápido
local function teleport(target)
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        player.Character.HumanoidRootPart.CFrame = target
        wait(1.5)
    end
end

-- Auto Collect Chests (Baús)
local function collectChests()
    while chestEnabled do
        for _, obj in pairs(workspace:GetChildren()) do
            if obj:IsA("Model") and obj:FindFirstChild("HumanoidRootPart") and obj.Name:find("Chest") then
                teleport(obj.HumanoidRootPart.CFrame)
            end
        end
        wait(5)
    end
end

-- Auto Collect Fruits (Frutas)
local function collectFruits()
    while fruitEnabled do
        for _, fruit in pairs(workspace:GetChildren()) do
            if fruit:IsA("Tool") and fruit.Name:find("Fruit") then
                teleport(fruit.Handle.CFrame)
            end
        end
        wait(5)
    end
end

-- Auto Farm NPCs
local function autoFarmNPCs()
    while farmEnabled do
        for _, npc in pairs(workspace.Enemies:GetChildren()) do
            if npc:IsA("Model") and npc:FindFirstChild("HumanoidRootPart") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                teleport(npc.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3))
                wait(0.5)
                for i = 1, 5 do
                    virtualUser:Button1Down(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
                    wait(0.3)
                end
            end
        end
        wait(5)
    end
end

-- Botões para ativar/desativar funções
FarmButton.MouseButton1Click:Connect(function()
    farmEnabled = not farmEnabled
    FarmButton.Text = farmEnabled and "Auto Farm: ON" or "Auto Farm: OFF"
    if farmEnabled then
        spawn(autoFarmNPCs)
    end
end)

FruitButton.MouseButton1Click:Connect(function()
    fruitEnabled = not fruitEnabled
    FruitButton.Text = fruitEnabled and "Coletar Frutas: ON" or "Coletar Frutas: OFF"
    if fruitEnabled then
        spawn(collectFruits)
    end
end)

ChestButton.MouseButton1Click:Connect(function()
    chestEnabled = not chestEnabled
    ChestButton.Text = chestEnabled and "Coletar Baús: ON" or "Coletar Baús: OFF"
    if chestEnabled then
        spawn(collectChests)
    end
end)
