-- Carrega as bibliotecas Fluent, SaveManager e InterfaceManager
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

-- Criando a Janela da Interface
local Window = Fluent:CreateWindow({
    Title = "Brookhaven RP 🏡 (Troll Hub 🤡)",
    SubTitle = "🔥 Zoando geral! 💀",
    TabWidth = 160,
    Size = UDim2.fromOffset(500, 320),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

-- Criando Abas
local Tabs = {
    Troll = Window:AddTab({ Title = "🤡 Troll", Icon = "alert" }),
    About = Window:AddTab({ Title = "ℹ️ Sobre", Icon = "info" })
}

-----------------------------------------------------------
-- 🤡 Troll (Selecionar Jogador + Matar)
-----------------------------------------------------------
Tabs.Troll:AddSection("Trollando no servidor!")

local jogadorSelecionado = nil
local dropdownJogadores

-- Atualiza a lista de jogadores automaticamente
local function atualizarListaJogadores()
    local jogadores = {}
    for _, player in pairs(game.Players:GetPlayers()) do
        table.insert(jogadores, player.Name)
    end

    -- Atualiza a Dropdown com os jogadores atuais
    if dropdownJogadores then
        dropdownJogadores:SetValues(jogadores)
    end
end

-- Criar Dropdown de seleção de jogador
dropdownJogadores = Tabs.Troll:AddDropdown("Selecionar Jogador", {
    Title = "Escolha um jogador",
    Values = {},
    Multi = false,
    Callback = function(valor)
        jogadorSelecionado = valor
    end
})

-- Atualiza a lista de jogadores ao abrir a GUI
atualizarListaJogadores()

-- Atualiza a lista automaticamente quando um jogador entra ou sai
game.Players.PlayerAdded:Connect(atualizarListaJogadores)
game.Players.PlayerRemoving:Connect(atualizarListaJogadores)

-- Botão para matar jogador selecionado
Tabs.Troll:AddButton({
    Title = "Matar Jogador ☠️",
    Description = "Teleporta o jogador para a morte",
    Callback = function()
        if jogadorSelecionado then
            local playerAlvo = game.Players:FindFirstChild(jogadorSelecionado)
            if playerAlvo and playerAlvo.Character and playerAlvo.Character:FindFirstChild("HumanoidRootPart") then
                playerAlvo.Character.HumanoidRootPart.CFrame = CFrame.new(-212, -499, -627)
            end
        else
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "Erro",
                Text = "Selecione um jogador primeiro!",
                Duration = 3
            })
        end
    end
})
