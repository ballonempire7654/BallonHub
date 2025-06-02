--[[
===========================
BALLON Hub - Muscle Legends 💪
===========================
]]

-- Variáveis principais
local player = game.Players.LocalPlayer
local maxRebirths = nil -- Variável para armazenar o limite de rebirths escolhido pelo jogador

-- GUI principal
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "BALLONHubGui"

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 500, 0, 400) -- Tamanho padrão
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -200) -- Centralizado
MainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40) -- Cor escura moderna
MainFrame.BorderSizePixel = 0 -- Sem bordas
MainFrame.Active = true
MainFrame.Draggable = true -- Interface arrastável

-- Barra de título estilizada
local TitleBar = Instance.new("Frame", MainFrame)
TitleBar.Size = UDim2.new(1, 0, 0, 50)
TitleBar.Position = UDim2.new(0, 0, 0, 0)
TitleBar.BackgroundColor3 = Color3.fromRGB(20, 20, 20) -- Cor da barra de título

local TitleText = Instance.new("TextLabel", TitleBar)
TitleText.Size = UDim2.new(1, -60, 1, 0)
TitleText.Position = UDim2.new(0, 10, 0, 0)
Title.Text = "BALLON HUB - Muscle Legends 💪"
TitleText.Font = Enum.Font.SourceSansBold
TitleText.TextSize = 20
TitleText.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleText.BackgroundTransparency = 1 -- Sem fundo no texto

-- Container para o limite80)
RebirthContainer.Position = UDim2.new(0, 10, 0, 60)
RebirthContainer.BackgroundTransparency = 1 -- Sem fundo

-- Campo de entrada para limite de rebirths
local RebirthInput = Instance.new("TextBox", RebirthContainer)
RebirthInput.Size =)
RebirthInput.Position = UDim2.new(0, 0, 0, 0)
RebirthInput.PlaceholderText = "Digite o limite de rebirths"
RebirthInput.Font = Enum.Font.SourceSansBold
RebirthInput.TextSize = 16
RebirthInput.TextColor3 = ColorTextButton", RebirthContainer)
ConfirmButton.Size = UDim2.new(0, 230, 0, 50)
ConfirmButton.Position = UDim2.new(0, 240, 0, 0)
ConfirmButton.Text = "Confirmar Limite"
ConfirmButton.Font = Enum.Font.SourceSansBold.TextSize = 16
ConfirmButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ConfirmButton.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
ConfirmButton.BorderSizePixel = 0 -- Sem bordas

-- Botões organizados em linhas
local150)
ButtonContainer.BackgroundTransparency = 1 -- Sem fundo

local UIGridLayout = Instance.new("UIGridLayout", ButtonContainer)
UIGridLayout.CellSize = UDim2.new(0, 230, 0, 50) -- Tamanho dos botões
UIGridLayout.CellPadding = UDim2.new(0, 10, 0, 10) -- Espaçamento entre botões

-- Função para criar botão
local function createButton(parent, text, color, callback)
    local button = Instance.new("TextButton", parent)
    button.Size = UDim2.new(0, 230, 0, 50)
    button.Text = text
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 16
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.BackgroundColor3 = color
    button.BorderSizePixel = 0 -- Sem bordas

    -- Conectar a função ao botão
    button.MouseButton1Click:Connect(function()
        if type(callback) == "function" then
            callback()
        else
            warn("Callback fornecido não é uma função!")
        end
    end)

    return button
end

-- Função para confirmar limite de rebirths
local function confirmRebirthLimit()
    local input = tonumber(RebirthInput.Text)
    if input and input > 0 then
        maxRebirths = input
        print("Limite de rebirths configurado para: " .. maxRebirths)
    else
        print("Por favor, insira um número válido!")
    end
end

-- Conectar botão de confirmação ao campo de entrada
ConfirmButton.MouseButton1Click:Connect(confirmRebirthLimit)

-- Coordenadas ajustadas das pedras
local rockCoordinates = {
    {name = "0 Dura Rock", position = Vector3.new(17.641, -1.31, 2106.489)},
    {name = "1M Dura Rock", position = Vector3.new(4140.418, 987.453, -4089.349)},
    {name = "5M Dura Rock", position = Vector3.new(8971.566, 27.403, -6060.277)},
    {name = "10M Dura Rock", position = Vector3.new(-7612.118, 32.403, 2910.784)}
}

-- Funções para teletransportar e socar automaticamente
local function teleportToRockAndPunch(coordinates)
    player.Character.HumanoidRootPart.CFrame = CFrame.new(coordinates)
    _G.autoPunch = true
    while _G.autoPunch do
        local args = { [1] = "punch" }
        game:GetService("Players").LocalPlayer.muscleEvent:FireServer(unpack(args))
        wait(0.1)
    end
end

local function disableAutoPunch()
    _G.autoPunch = false
    print("Soco automático desativado!")
end

-- Criar botões para pedras
for _, rock in pairs(rockCoordinates) do
    createButton(ButtonContainer, rock.name, Color3.fromRGB(0, 150, 150), function()
        teleportToRockAndPunch(rock.position)
    end)
end

-- Treino rápido e funções automáticas
local function fastTrain()
    _G.fastTrain = true
    while _G.fastTrain do
        local args = { [1] = "rep" }
        game:GetService("Players").LocalPlayer.muscleEvent:FireServer(unpack(args))
        wait(0.05)
    end
end

local function disableFastTrain()
    _G.fastTrain = false
    print("Treino rápido desativado!")
end

local function autoRebirth()
    if not maxRebirths then
        print("Por favor, configure um limite de rebirths antes de ativar!")
        return
    end

    _G.autoRebirth = true
    while _G.autoRebirth do
        local leaderstats = player:WaitForChild("leaderstats")
        local strength = leaderstats:WaitForChild("Strength")
        local currentStrength = strength.Value
        local requiredStrength = (leaderstats.Rebirths.Value + 1) * 5000
        local currentRebirths = leaderstats.Rebirths.Value

        if currentStrength >= requiredStrength and currentRebirths < maxRebirths then
            local args = { [1] = "rebirth" }
            game:GetService("Players").LocalPlayer.rebirthEvent:FireServer(unpack(args))
            print("Reencarnação " .. currentRebirths)
        elseif currentRebirths >= maxRebirths then
            print("Você atingiu o limite de rebirths configurado: " .. maxRebirths)
            _G.autoRebirth = false
        else
            print("Força insuficiente. Atual: " .. currentStrength .. ", Necessária: " .. requiredStrength)
        end
        wait(1)
    end
end

local function disableAutoRebirth()
    _G.autoRebirth = false
    print("Reencarnações automáticas desativadas!")
end

-- Adicionando botões à interface
createButton(ButtonContainer, "Ativar Treino Rápido", Color3.fromRGB(0, 150, 0), fastTrain)
createButton(ButtonContainer, "Desativar Treino Rápido", Color3.fromRGB(150, 0, 0), disableFastTrain)
createButton(ButtonContainer, "Ativar Reencarnações", Color3.fromRGB(0, 150, 0), autoRebirth)
createButton(ButtonContainer, "Desativar Reencarnações", Color3.fromRGB(150, 0, 0), disableAutoRebirth)
createButton(ButtonContainer, "Desativar Soco Automático", Color3.fromRGB(150, 0, 0), disableAutoPunch)