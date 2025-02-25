local player = game.Players.LocalPlayer
local chests = {}

-- Função para encontrar todos os baús no jogo
local function findChests()
    for _, v in pairs(game.Workspace:GetChildren()) do
        if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") and string.find(v.Name, "Chest") then
            table.insert(chests, v)
        end
    end
end

-- Função para teleportar suavemente até os baús
local function teleportSmooth(target)
    if target and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local humanoidRootPart = player.Character.HumanoidRootPart
        for i = 1, 10 do -- Teleporte em 10 pequenos passos
            humanoidRootPart.CFrame = humanoidRootPart.CFrame:Lerp(target.CFrame, 0.2)
            wait(0.1) -- Pequeno delay para evitar kick
        end
    end
end

-- Função para coletar baús de forma segura
local function collectChests()
    for _, chest in pairs(chests) do
        if chest and chest:FindFirstChild("HumanoidRootPart") then
            teleportSmooth(chest.HumanoidRootPart.CFrame)
            wait(1) -- Aguardar um pouco antes do próximo teleporte
        end
    end
end

-- Loop para farmar baús automaticamente
while wait(5) do
    chests = {} -- Limpa a lista antes de atualizar
    findChests() -- Busca novos baús
    collectChests() -- Coleta os baús encontrados
end
