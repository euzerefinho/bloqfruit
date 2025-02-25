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

-- Função para teleportar até os baús e coletá-los
local function collectChests()
    for _, chest in pairs(chests) do
        if chest and chest:FindFirstChild("HumanoidRootPart") then
            player.Character.HumanoidRootPart.CFrame = chest.HumanoidRootPart.CFrame
            wait(0.5) -- Pequeno delay para evitar bugs
        end
    end
end

-- Loop para farmar baús automaticamente
while wait(5) do
    chests = {} -- Limpa a lista antes de atualizar
    findChests() -- Busca novos baús
    collectChests() -- Coleta os baús encontrados
end
