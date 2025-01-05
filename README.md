local player = game.Players.LocalPlayer
local camera = game.Workspace.CurrentCamera
local character = player.Character or player.CharacterAdded:Wait()
local lineColor = Color3.fromRGB(255, 255, 255) -- Cor branca para as linhas

-- Função para criar o nome do jogador
local function criarNomeJogador(jogador)
    local screenGui = Instance.new("ScreenGui")
    screenGui.Parent = player.PlayerGui

    local textLabel = Instance.new("TextLabel")
    textLabel.Text = jogador.Name
    textLabel.TextSize = 24
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    textLabel.BackgroundTransparency = 1
    textLabel.Size = UDim2.new(0, 200, 0, 50)
    textLabel.Position = UDim2.new(0.5, -100, 0.1, 0)
    textLabel.Parent = screenGui

    -- Destroi o nome após 5 segundos
    game.Debris:AddItem(screenGui, 5)
end

-- Função para desenhar as linhas
local function desenharLinha(part1, part2)
    local beam = Instance.new("Beam")
    beam.Parent = part1
    beam.Color = ColorSequence.new(lineColor)
    beam.Width0 = 0.2
    beam.Width1 = 0.2

    local attachment1 = Instance.new("Attachment")
    attachment1.Parent = part1
    attachment1.Position = Vector3.new(0, 0, 0) -- Ajuste a posição do attachment, se necessário

    local attachment2 = Instance.new("Attachment")
    attachment2.Parent = part2
    attachment2.Position = Vector3.new(0, 0, 0)

    beam.Attachment0 = attachment1
    beam.Attachment1 = attachment2
end

-- Monitora outros jogadores no servidor
game.Players.PlayerAdded:Connect(function(jogador)
    if jogador == player then return end

    -- Monitorar a adição de personagem do outro jogador
    jogador.CharacterAdded:Connect(function(outroCharacter)
        local outroHumanoid = outroCharacter:WaitForChild("Humanoid")
        local outroHumanoidRootPart = outroCharacter:WaitForChild("HumanoidRootPart")

        -- Cria o nome do jogador
        criarNomeJogador(jogador)

        -- Desenha a linha do seu personagem para o personagem do outro jogador
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

        -- Desenha a linha branca entre os dois jogadores
        desenharLinha(humanoidRootPart, outroHumanoidRootPart)
    end)
end)
