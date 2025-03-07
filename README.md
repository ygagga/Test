-- Carregar serviços necessários
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local Mouse = game:GetService("Players").LocalPlayer:GetMouse()
local RunService = game:GetService("RunService")
local LocalPlayer = game:GetService("Players").LocalPlayer
local Workspace = game:GetService("Workspace")

-- Criar GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = LocalPlayer.PlayerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 400)
frame.Position = UDim2.new(0.5, -150, 0.5, -200)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 0.5
frame.Parent = ScreenGui

local playerList = Instance.new("ScrollingFrame")
playerList.Size = UDim2.new(1, 0, 1, 0)
playerList.CanvasSize = UDim2.new(0, 0, 5, 0)
playerList.ScrollBarThickness = 10
playerList.Parent = frame

local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 100, 0, 50)
closeButton.Position = UDim2.new(0.5, -50, 1, -60)
closeButton.Text = "Fechar"
closeButton.Parent = frame

local selectedPlayer = nil
local Updated = Mouse.Hit + Vector3.new(0, 5, 0)

-- Função para forçar o efeito do black hole
local function ForcePart(v)
    if v:IsA("Part") and v.Anchored == false and v.Parent:FindFirstChild("Humanoid") == nil and v.Parent:FindFirstChild("Head") == nil and v.Name ~= "Handle" then
        Mouse.TargetFilter = v
        -- Remover componentes desnecessários para o efeito funcionar
        for _, x in next, v:GetChildren() do
            if x:IsA("BodyAngularVelocity") or x:IsA("BodyForce") or x:IsA("BodyGyro") or x:IsA("BodyPosition") or x:IsA("BodyThrust") or x:IsA("BodyVelocity") or x:IsA("RocketPropulsion") then
                x:Destroy()
            end
        end
        if v:FindFirstChild("Attachment") then
            v:FindFirstChild("Attachment"):Destroy()
        end
        if v:FindFirstChild("AlignPosition") then
            v:FindFirstChild("AlignPosition"):Destroy()
        end
        if v:FindFirstChild("Torque") then
            v:FindFirstChild("Torque"):Destroy()
        end
        v.CanCollide = false
        -- Criar novos componentes para o efeito
        local Torque = Instance.new("Torque", v)
        Torque.Torque = Vector3.new(100000, 100000, 100000)
        local AlignPosition = Instance.new("AlignPosition", v)
        local Attachment2 = Instance.new("Attachment", v)
        Torque.Attachment0 = Attachment2
        AlignPosition.MaxForce = 9999999999999999
        AlignPosition.MaxVelocity = math.huge
        AlignPosition.Responsiveness = 200
        AlignPosition.Attachment0 = Attachment2 
        AlignPosition.Attachment1 = Attachment1
    end
end

-- Função para criar um botão de jogador
local function createPlayerButton(player)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, 0, 0, 30)
    button.Text = player.Name
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    button.Parent = playerList

    button.MouseButton1Click:Connect(function()
        selectedPlayer = player
        print("Jogador selecionado: " .. player.Name)
        -- Fecha a GUI ao selecionar
        ScreenGui:Destroy()
    end)
end

-- Criar botões para todos os jogadores
for _, player in pairs(Players:GetPlayers()) do
    createPlayerButton(player)
end

Players.PlayerAdded:Connect(function(player)
    createPlayerButton(player)
end)

-- Fechar GUI ao pressionar o botão "Fechar"
closeButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Atualiza a posição do black hole
local Attachment1 = Instance.new("Attachment")
local Part = Instance.new("Part", Workspace)
Part.Anchored = true
Part.CanCollide = false
Part.Transparency = 1
Attachment1.Parent = Part

spawn(function()
    while RunService.RenderStepped
    
