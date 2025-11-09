-- Coloque este LocalScript em StarterPlayerScripts ou StarterGui

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local brainrotName = "Brainrot" -- nome do item
local tpPosition = Vector3.new(0, 10, 0) -- SUBSTITUA pela posição da sua base!

local button -- referência do botão

-- Função para criar o botão na tela
local function createButton()
    if button then button:Destroy() end
    button = Instance.new("TextButton")
    button.Text = "TP BASE"
    button.Size = UDim2.new(0, 120, 0, 40)
    button.Position = UDim2.new(1, -130, 1, -50)
    button.AnchorPoint = Vector2.new(0, 0)
    button.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui"):WaitForChild("ScreenGui")
    button.BackgroundColor3 = Color3.new(0.1, 0.8, 0.1)
    button.TextSize = 24

    button.MouseButton1Click:Connect(function()
        -- Teleporta o personagem para a base
        local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
        if humanoidRootPart then
            humanoidRootPart.CFrame = CFrame.new(tpPosition)
        end
    end)
end

-- Função para verificar se o Brainrot está na mão
local function checkBrainrot()
    character = player.Character or player.CharacterAdded:Wait()
    local tool = character:FindFirstChildOfClass("Tool")
    if tool and tool.Name == brainrotName then
        -- Cria o botão se estiver com o Brainrot
        if not button or not button.Parent then
            createButton()
        end
    else
        -- Some com o botão se não estiver com o Brainrot
        if button then
            button:Destroy()
            button = nil
        end
    end
end

-- Atualiza toda vez que pega ou larga um Tool
character.ChildAdded:Connect(checkBrainrot)
character.ChildRemoved:Connect(checkBrainrot)

-- Quando o personagem respawnar
player.CharacterAdded:Connect(function(char)
    character = char
    char.ChildAdded:Connect(checkBrainrot)
    char.ChildRemoved:Connect(checkBrainrot)
end)

-- Tenta criar ScreenGui se não existir
if not player.PlayerGui:FindFirstChild("ScreenGui") then
    local gui = Instance.new("ScreenGui")
    gui.Name = "ScreenGui"
    gui.Parent = player.PlayerGui
end

