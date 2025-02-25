local player = game.Players.LocalPlayer
local tweenService = game:GetService("TweenService")
local replicatedStorage = game:GetService("ReplicatedStorage")
local workspace = game:GetService("Workspace")

-- Função para criar movimento suave com TweenService
local function teleportSmooth(targetPosition)
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local humanoidRootPart = player.Character.HumanoidRootPart
        local tweenInfo = TweenInfo.new(2, Enum.EasingStyle.Linear)
        local tween = tweenService:Create(humanoidRootPart, tweenInfo, {CFrame = targetPosition})
        
        tween:Play()
        tween.Completed:Wait()
    end
end

-- AUTO COLLECT CHESTS (BAÚS)
local function collectChests()
    for _, chest in pairs(workspace:GetChildren()) do
        if chest:IsA("Model") and chest:FindFirstChild("HumanoidRootPart") and string.find(chest.Name, "Chest") then
            teleportSmooth(chest.HumanoidRootPart.CFrame)
            wait(1.5)
        end
    end
end

-- AUTO COLLECT FRUITS (FRUTAS)
local function collectFruits()
    for _, fruit in pairs(workspace:GetChildren()) do
        if fruit:IsA("Tool") and string.find(fruit.Name, "Fruit") then
            teleportSmooth(fruit.Handle.CFrame)
            wait(1)
        end
    end
end

-- AUTO FARM NPCs
local function autoFarmNPCs()
    local enemies = {}

    -- Encontrar NPCs inimigos
    for _, npc in pairs(workspace:GetChildren()) do
        if npc:IsA("Model") and npc:FindFirstChild("HumanoidRootPart") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
            table.insert(enemies, npc)
        end
    end

    -- Atacar cada inimigo encontrado
    for _, enemy in pairs(enemies) do
        if enemy and enemy:FindFirstChild("HumanoidRootPart") then
            teleportSmooth(enemy.HumanoidRootPart.CFrame)
            wait(0.5)
            
            -- Simular ataque (Requer arma equipada)
            for i = 1, 5 do
                replicatedStorage.Remotes.CommF_:InvokeServer("DamageNPC", enemy, 10, "Melee")
                wait(0.2)
            end
        end
    end
end

-- Loop principal
while wait(5) do
    collectChests()  -- Coleta baús
    collectFruits()  -- Coleta frutas
    autoFarmNPCs()   -- Auto farm de NPCs
end
