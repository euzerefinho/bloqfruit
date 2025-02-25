local player = game.Players.LocalPlayer
local tweenService = game:GetService("TweenService")
local chests = {}

-- Função para encontrar todos os baús no jogo
local function findChests()
    chests = {} -- Limpa a lista antes de atualizar
    for _, v in pairs(game.Workspace:GetChildren()) do
        if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") and string.find(v.Name, "Chest") then
            table.insert(chests, v)
        end
    end
end

-- Função para mover o player suavemente até o alvo usando TweenService
local function teleportSmooth(targetPosition)
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local humanoidRootPart = player.Character.HumanoidRootPart

        -- Configuração da animação de teleporte
        local tweenInfo = TweenInfo.new(2, Enum.EasingStyle.Linear)
        local tween = tweenService:Create(humanoidRootPart, tweenInfo, {CFrame = targetPosition})

        tween:Play()
        tween.Completed:Wait() -- Aguarda o tween terminar
    end
end

-- Função para coletar os baús
local function collectChests()
    for _, chest in pairs(chests) do
        if chest and chest:FindFirstChild("HumanoidRootPart") then
            teleportSmooth(chest.HumanoidRootPart.CFrame)
            wait(1.5) -- Tempo para evitar detecção
        end
    end
end

-- Loop para coletar os baús automaticamente
while wait(5) do
    findChests() -- Busca novos baús
    collectChests() -- Coleta os baús encontrados
end
