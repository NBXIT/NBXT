local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

function criarESP(player)
    if player == LocalPlayer then return end

    local texto = Drawing.new("Text")
    texto.Size = 13
    texto.Center = true
    texto.Outline = true
    texto.Color = Color3.fromRGB(255, 0, 0) -- vermelho
    texto.Text = "ENEMY" -- ou player.Name se quiser

    local function atualizar()
        local success, err = pcall(function()
            local char = player.Character
            if char and char:FindFirstChild("Head") and char:FindFirstChild("Humanoid") and player.Character.Humanoid.Health > 0 then
                local pos, visível = Camera:WorldToViewportPoint(char.Head.Position + Vector3.new(0, 0.5, 0))
                if visível then
                    texto.Position = Vector2.new(pos.X, pos.Y)
                    texto.Visible = true
                    return
                end
            end
        end)
        texto.Visible = false
    end

    game:GetService("RunService").RenderStepped:Connect(atualizar)
end

-- Adiciona ESP para jogadores já na partida
for _, player in ipairs(Players:GetPlayers()) do
    criarESP(player)
end

-- Adiciona ESP para jogadores que entrarem depois
Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        wait(1)
        criarESP(player)
    end)
end)
