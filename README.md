-- Carregar a Redz Library V4
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/hacked-prototype/RedzLibV4/main/Source.lua"))()

-- Criando a Interface
local Window = Library:MakeWindow({
    Title = "👾ZenithCore👾",
    SubTitle = "Zoando geral!",
    LoadText = "ZenithCore 👾",
    Flags = "ZenithCore_Config"
})

-- Adicionar botão para minimizar com a logo personalizada
Window:AddMinimizeButton({
    Button = {
        Image = "rbxassetid://10180536602"
    },
    UICorner = {true,
        CornerRadius = UDim.new(0.5, 0)
    },
    UIStroke = {false},
    Size = UDim2.new(0, 40, 0, 40)  -- Aumentando o tamanho do quadrado
})

-- Criar as Abas (Tabs)
local TrollTab = Window:MakeTab({Name = "Troll", Icon = "Skull"})
local MusicTab = Window:MakeTab({Name = "Música", Icon = "Music"})
local HacksTab = Window:MakeTab({Name = "Hacks", Icon = "Lightning"})
local ScriptsTab = Window:MakeTab({Name = "Scripts", Icon = "Code"})
local AboutTab = Window:MakeTab({Name = "Sobre", Icon = "Info"})

-----------------------------------------------------------
-- 🤡 TROLL (Teleportar, Espectar, Matar)
-----------------------------------------------------------
TrollTab:AddSection({"Controle de Jogadores"})

local selectedPlayer = ""

TrollTab:AddTextBox({"Nome do Jogador", "", "Digite o nome do jogador",
    Callback = function(value)
        selectedPlayer = value
    end
})

TrollTab:AddButton({
    Name = "Teleportar Todos para Mim",
    Callback = function()
        local players = game:GetService("Players")
        local localPlayer = players.LocalPlayer
        local root = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")

        if root then
            for _, target in pairs(players:GetPlayers()) do
                if target.Character and target ~= localPlayer then
                    local targetRoot = target.Character:FindFirstChild("HumanoidRootPart")
                    if targetRoot then
                        targetRoot.CFrame = root.CFrame
                    end
                end
            end
        end
    end
})

TrollTab:AddButton({
    Name = "Espectar Jogador",
    Callback = function()
        local players = game:GetService("Players")
        local target = players:FindFirstChild(selectedPlayer)

        if target and target.Character then
            game.Workspace.CurrentCamera.CameraSubject = target.Character:FindFirstChildOfClass("Humanoid")
        end
    end
})

TrollTab:AddButton({
    Name = "Parar de Espectar",
    Callback = function()
        game.Workspace.CurrentCamera.CameraSubject = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    end
})

--------------------------------------
-- 🎶 Aba Música (Tocar para Todos)
--------------------------------------
MusicTab:AddSection({"Escolha sua Música"})

local globalMusicId = ""
local globalSound
local isLoopEnabled = false

MusicTab:AddToggle({
    Name = "Tocar em Loop 🔁",
    Default = false,
    Callback = function(value)
        isLoopEnabled = value
    end
})

MusicTab:AddTextBox({"ID da Música Global", "", "Digite o ID...",
    Callback = function(value)
        globalMusicId = value
    end
})

local musicIds = {
    ["🎵 Música 1"] = "6454199333",
    ["🎵 Música 2"] = "6427245762",
    ["🎵 Música 3"] = "6489326185"
}

for name, id in pairs(musicIds) do
    MusicTab:AddButton({
        Name = name,
        Callback = function()
            if globalSound then globalSound:Destroy() end
            globalSound = Instance.new("Sound", game.Workspace)
            globalSound.SoundId = "rbxassetid://" .. id
            globalSound.Volume = 10
            globalSound.Looped = isLoopEnabled
            globalSound:Play()
        end
    })
end

MusicTab:AddButton({
    Name = "Tocar ID Personalizado 📢",
    Callback = function()
        if globalMusicId ~= "" then
            if globalSound then globalSound:Destroy() end
            globalSound = Instance.new("Sound", game.Workspace)
            globalSound.SoundId = "rbxassetid://" .. globalMusicId
            globalSound.Volume = 10
            globalSound.Looped = isLoopEnabled
            globalSound:Play()
        end
    end
})

MusicTab:AddButton({
    Name = "Parar Música Global ⛔",
    Callback = function()
        if globalSound then
            globalSound:Stop()
            globalSound:Destroy()
            globalSound = nil
        end
    end
})

--------------------------------------
-- 🧑‍💻 SCRIPTS (Executar Scripts Extras)
--------------------------------------
ScriptsTab:AddSection({"Executar Scripts"})

ScriptsTab:AddButton({
    Name = "Fly Script ✈️",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))()
    end
})

ScriptsTab:AddButton({
    Name = "RAEL Hub 🔧",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/Laelmano24/Rael-Hub/main/main.txt"))()
    end
})

ScriptsTab:AddButton({
    Name = "Sander X 🛸",
    Callback = function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/sXPiterXs1111/Sanderxv3.30/main/sanderx3.30'))()
    end
})

-----------------------------------------------------------
-- ℹ️ SOBRE
-----------------------------------------------------------
AboutTab:AddSection({"Criado por Shelby, user discord: snobodj"})

AboutTab:AddParagraph({"Troll Hub 🤡", "Criado para trollar no Brookhaven RP! Divirta-se!"})
