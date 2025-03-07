local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer

-- Função para encontrar o jogador pelo nome
local function getPlayerByName(targetName)
    for _, plr in pairs(Players:GetPlayers()) do
        if string.lower(plr.Name) == string.lower(targetName) then
            return plr
        end
    end
    return nil
end

-- Função para puxar partes do mapa para o jogador
local function pullMapToPlayer(targetName)
    local targetPlayer = getPlayerByName(targetName)
    
    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local targetPosition = targetPlayer.Character.HumanoidRootPart.Position
        
        for _, part in pairs(Workspace:GetDescendants()) do
            if part:IsA("BasePart") and part.Anchored then
                part.Anchored = false -- Destrava o objeto para puxar
                part.CFrame = CFrame.new(targetPosition + Vector3.new(math.random(-5, 5), math.random(1, 5), math.random(-5, 5)))
            end
        end
    end
end

-- **Executar a função**
pullMapToPlayer("NomeDoJogador") -- Substitua pelo nome do jogador alvo
