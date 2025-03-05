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

-- Criando a aba Troll (agora sempre inicializada corretamente)
local TrollTab = Window:AddTab({ Title = "🤡 Troll", Icon = "alert" })

-- 🔥 APLICAR ACESSÓRIO AUTOMÁTICO 🔥
local function applyAccessory()
    local character = game.Players.LocalPlayer.Character
    if character then
        local accessoryId = 18687134102
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid:AddAccessory(game:GetService("InsertService"):LoadAsset(accessoryId):GetChildren()[1])
        end
    end
end

applyAccessory() -- Aplica automaticamente ao abrir o script

-----------------------------------------------------------
-- 🔍 ESP (Nome e Distância)
-----------------------------------------------------------
local espEnabled = false
local function toggleESP()
    espEnabled = not espEnabled
    for _, v in pairs(game.Players:GetPlayers()) do
        if v ~= game.Players.LocalPlayer then
            if espEnabled then
                if not v.Character or not v.Character:FindFirstChild("Head") then continue end
                local bill = Instance.new("BillboardGui", v.Character.Head)
                bill.Name = "ESP"
                bill.Size = UDim2.new(0, 200, 0, 50)
                bill.StudsOffset = Vector3.new(0, 2, 0)
                bill.AlwaysOnTop = true

                local nameTag = Instance.new("TextLabel", bill)
                nameTag.Size = UDim2.new(1, 0, 1, 0)
                nameTag.TextColor3 = Color3.fromRGB(255, 0, 0)
                nameTag.BackgroundTransparency = 1
                nameTag.TextStrokeTransparency = 0.5
                nameTag.TextScaled = true
                nameTag.Text = v.Name

                game:GetService("RunService").RenderStepped:Connect(function()
                    if v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                        local dist = math.floor((game.Players.LocalPlayer.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude)
                        nameTag.Text = v.Name .. " [" .. dist .. "m]"
                    end
                end)
            else
                if v.Character and v.Character.Head:FindFirstChild("ESP") then
                    v.Character.Head.ESP:Destroy()
                end
            end
        end
    end
end

TrollTab:AddButton({
    Title = "Ativar/Desativar ESP 🔍",
    Description = "Mostra nome e distância dos jogadores",
    Callback = toggleESP
})

-----------------------------------------------------------
-- ⚡ SUPER VELOCIDADE E PULO INFINITO
-----------------------------------------------------------
local speedEnabled = false
TrollTab:AddButton({
    Title = "Ativar/Desativar Super Velocidade ⚡",
    Callback = function()
        speedEnabled = not speedEnabled
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = speedEnabled and 100 or 16
    end
})

local jumpEnabled = false
TrollTab:AddButton({
    Title = "Ativar/Desativar Pulo Infinito 🦘",
    Callback = function()
        jumpEnabled = not jumpEnabled
        game:GetService("UserInputService").JumpRequest:Connect(function()
            if jumpEnabled then
                game.Players.LocalPlayer.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end)
    end
})

-----------------------------------------------------------
-- 🚪 ATRAVESSAR PAREDES
-----------------------------------------------------------
local noClipEnabled = false
TrollTab:AddButton({
    Title = "Ativar/Desativar NoClip 🚪",
    Callback = function()
        noClipEnabled = not noClipEnabled
        game:GetService("RunService").Stepped:Connect(function()
            if noClipEnabled then
                for _, part in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
    end
})

-----------------------------------------------------------
-- 🎭 COPIAR SKIN DE OUTRO JOGADOR
-----------------------------------------------------------
TrollTab:AddTextbox({
    Title = "Copiar Skin de Jogador",
    Placeholder = "Digite o nome do jogador",
    Callback = function(targetName)
        local target = game.Players:FindFirstChild(targetName)
        if target and target.Character then
            local char = game.Players.LocalPlayer.Character
            for _, v in pairs(target.Character:GetChildren()) do
                if v:IsA("Shirt") or v:IsA("Pants") or v:IsA("Accessory") then
                    v:Clone().Parent = char
                end
            end
        end
    end
})

-----------------------------------------------------------
-- 🏃 SELECIONAR JOGADOR PARA TELEPORTAR, SPECTAR OU MATAR
-----------------------------------------------------------
local selectedPlayer
TrollTab:AddDropdown({
    Title = "Selecionar Jogador",
    Values = (function()
        local playerList = {}
        for _, v in pairs(game.Players:GetPlayers()) do
            if v ~= game.Players.LocalPlayer then
                table.insert(playerList, v.Name)
            end
        end
        return playerList
    end)(),
    Callback = function(value)
        selectedPlayer = value
    end
})

TrollTab:AddButton({
    Title = "Teleportar até o Jogador",
    Callback = function()
        if selectedPlayer then
            local target = game.Players:FindFirstChild(selectedPlayer)
            if target and target.Character then
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = target.Character.HumanoidRootPart.CFrame
            end
        end
    end
})

TrollTab:AddButton({
    Title = "Spectar Jogador",
    Callback = function()
        if selectedPlayer then
            local target = game.Players:FindFirstChild(selectedPlayer)
            if target and target.Character then
                game.Workspace.CurrentCamera.CameraSubject = target.Character.Humanoid
            end
        end
    end
})

TrollTab:AddButton({
    Title = "Matar Jogador",
    Callback = function()
        if selectedPlayer then
            local target = game.Players:FindFirstChild(selectedPlayer)
            if target and target.Character and target.Character:FindFirstChild("Humanoid") then
                target.Character.Humanoid.Health = 0
            end
        end
    end
})
