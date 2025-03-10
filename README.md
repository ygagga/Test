local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/hacked-prototype/RedzLibV4/main/Source.lua"))()

-- Criando a janela principal
local Window = Library:MakeWindow({
    Title = "Meu Script de Teste",
    SubTitle = "Usando Redz Library V4",
    LoadText = "Carregando...",
    Flags = "MeuScript.lua"
})

-- Criando uma aba
local Tab = Window:MakeTab({ Name = "Funções", Icon = "Home" })

-- Criando uma seção
local Section = Tab:AddSection({"Opções"})

-- Adicionando um botão
local Button = Tab:AddButton({
    Name = "Clique Aqui",
    Callback = function()
        print("Botão pressionado!")
    end
})

-- Adicionando um toggle
local Toggle = Tab:AddToggle({
    Name = "Ativar Função",
    Default = false,
    Callback = function(Value)
        print("Toggle:", Value)
    end
})

-- Adicionando um slider
local Slider = Tab:AddSlider({
    Name = "Ajuste de Valor",
    MinValue = 1,
    MaxValue = 100,
    Default = 50,
    Increase = 1,
    Callback = function(Value)
        print("Valor do slider:", Value)
    end
})

-- Adicionando um dropdown
local Dropdown = Tab:AddDropdown({
    Name = "Selecione uma opção",
    Options = {"Opção 1", "Opção 2", "Opção 3"},
    Default = {"Opção 2"},
    MultiSelect = false,
    Callback = function(Value)
        print("Selecionado:", Value)
    end
})

-- Adicionando um TextBox
local TextBox = Tab:AddTextBox({
    "Digite algo",
    "",
    "< input >",
    Callback = function(Text)
        print("Texto digitado:", Text)
    end
})

-- Criando uma notificação de teste
Library:MakeNotify({
    Title = "Teste Concluído",
    Text = "O teste foi executado com sucesso!",
    Time = 5
})
