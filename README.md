-- Carregar a Redz Library V4
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/hacked-prototype/RedzLibV4/main/Source.lua"))()

-- Criando a Interface
local Window = Library:MakeWindow({
    Title = "👾ZenithCore👾",
    SubTitle = "Zoando geral!",
    LoadText = "ZenithCore 👾",
    Flags = "ZenithCore_Config"
})

-- Criar as Abas
local TrollTab = Window:MakeTab({ Name = "Troll", Icon = "Home" })
local MusicTab = Window:MakeTab({ Name = "Música", Icon = "Music" })
local HacksTab = Window:MakeTab({ Name = "Hacks", Icon = "Bolt" })
local ScriptsTab = Window:MakeTab({ Name = "Scripts", Icon = "Code" })
local AboutTab = Window:MakeTab({ Name = "Sobre", Icon = "Info" })

-----------------------------------------------------------
-- 🤡 TROLL
-----------------------------------------------------------
local selectedPlayer = ""

TrollTab:AddSection({ "Controle de Jogadores" })

TrollTab:AddTextBox({
    "Nome do Jogador", "", "Digite o nome do jogador",
    Callback = function(value)
        selectedPlayer = value
    end
})

TrollTab:AddButton({
    Name = "Teleportar Todos para Mim",
    Callback = function()
        for _, target in pairs(game.Players:GetPlayers()) do
            if target.Character and target ~= game.Players.LocalPlayer then
                local targetRoot = target.Character:FindFirstChild("HumanoidRootPart")
                if targetRoot then
                    targetRoot.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
                end
            end
        end
    end
})

TrollTab:AddButton({
    Name = "Espectar Jogador",
    Callback = function()
        local target = game.Players:FindFirstChild(selectedPlayer)
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

-----------------------------------------------------------
-- 🎶 MÚSICA
-----------------------------------------------------------
local globalMusicId = ""
local globalSound
local isLoopEnabled = false

MusicTab:AddSection({ "Escolha sua Música" })

MusicTab:AddToggle({
    Name = "Tocar em Loop 🔁",
    Default = false,
    Callback = function(value)
        isLoopEnabled = value
    end
})

MusicTab:AddTextBox({
    "ID da Música Global", "", "Digite o ID...",
    Callback = function(value)
        globalMusicId = value
    end
})

local musicIds = {
    ["🎵 Música 1"] = "6454199333",
    ["🎵 Música 2"] = "6427245762",
    ["🎵 Música 3"] = "6489326185",
    ["🎵 Música 4"] = "6433157341",
    ["🎵 Música 5"] = "6436089393",
    ["🎵 Música 6"] = "18841894272",
    ["🎵 Música 7"] = "16190784547"
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

-----------------------------------------------------------
-- 💻 HACKS (Anti Sit)
-----------------------------------------------------------
local antiSitEnabled = false

HacksTab:AddSection({ "Anti Sit (Desativar/Sentado)" })

HacksTab:AddToggle({
    Name = "Ativar/Desativar Anti Sit 🚫",
    Default = false,
    Callback = function(value)
        antiSitEnabled = value
        if antiSitEnabled then
            game.Players.LocalPlayer.Character.Humanoid.Sit = false
            game.Players.LocalPlayer.Character.Humanoid.Seated:Connect(function()
                game.Players.LocalPlayer.Character.Humanoid.Sit = false
            end)
        end
    end
})

-----------------------------------------------------------
-- 🧑‍💻 SCRIPTS EXTRAS
-----------------------------------------------------------
ScriptsTab:AddSection({ "Executar Scripts" })

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
AboutTab:AddSection({ "Criado por Shelby, user discord: snobodj" })

AboutTab:AddParagraph({ "Troll Hub 🤡", "Criado para trollar no Brookhaven RP! Divirta-se!" })
