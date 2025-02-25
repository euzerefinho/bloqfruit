local player = game.Players.LocalPlayer
local workspace = game:GetService("Workspace")
local tweenService = game:GetService("TweenService")
local virtualUser = game:GetService("VirtualUser")

-- Evita que o jogador seja kickado por inatividade
player.Idled:Connect(function()
    virtualUser:Button2Down(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
    wait(1)
    virtualUser:Button2Up(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
end)

-- Função para teleporte seguro
local function teleport(target)
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local humanoidRootPart = player.Character.HumanoidRootPart
        humanoidRootPart.CFrame = target
        wait(1.5) -- Pequeno delay para evitar detecção
    end
end

-- Auto Collect Chests (Baús)
local function collectChests()
    for _, obj in pairs(workspace:GetChildren()) do
        if obj:IsA("Model") and obj:FindFirstChild("HumanoidRootPart") and obj.Name:find("Chest") then
            teleport(obj.HumanoidRootPart.CFrame)
        end
    end
end

-- Auto Collect Fruits (Frutas)
local function collectFruits()
    for _, fruit in pairs(workspace:GetChildren()) do
        if fruit:IsA("Tool") and fruit.Name:find("Fruit") then
            teleport(fruit.Handle.CFrame)
        end
    end
end

-- Auto Farm NPCs
local function autoFarmNPCs()
    for _, npc in pairs(workspace.Enemies:GetChildren()) do
        if npc:IsA("Model") and npc:FindFirstChild("HumanoidRootPart") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
            teleport(npc.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)) -- Fica perto do NPC
            wait(0.5)

            -- Simula ataques (pressionando a tecla virtual)
            for i = 1, 5 do
                virtualUser:Button1Down(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
                wait(0.3)
            end
        end
    end
end

-- Loop Principal
while wait(5) do
    collectChests()  -- Coletar baús
    collectFruits()  -- Coletar frutas
    autoFarmNPCs()   -- Farm de NPCs
end
