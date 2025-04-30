--[[ 
    Script de Teste Autorizado
    Feito por: Nicolas | v1.1
--]]

-- KEYS VÁLIDAS
local keys = {
    ["Nicolas"] = "Dono",
    ["teste"] = "Membro"
}

local ESPEnabled = true
local AimbotEnabled = true

-- SOM DE LOGIN
local function playLoginSound()
    local sound = Instance.new("Sound", game.SoundService)
    sound.SoundId = "rbxassetid://9118823104"
    sound.Volume = 1
    sound:Play()
    game.Debris:AddItem(sound, 3)
end

-- GUI DE LOGIN
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "LoginGui"

local loginFrame = Instance.new("Frame", ScreenGui)
loginFrame.Size = UDim2.new(0, 300, 0, 250)
loginFrame.Position = UDim2.new(0.5, -150, 0.5, -125)
loginFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Instance.new("UICorner", loginFrame).CornerRadius = UDim.new(0, 12)

local image = Instance.new("ImageLabel", loginFrame)
image.Size = UDim2.new(0, 100, 0, 100)
image.Position = UDim2.new(0.5, -50, 0, -90)
image.BackgroundTransparency = 1
image.Image = "rbxassetid://3276713659"

local title = Instance.new("TextLabel", loginFrame)
title.Size = UDim2.new(1, 0, 0, 30)
title.Position = UDim2.new(0, 0, 0, 20)
title.BackgroundTransparency = 1
title.Text = "LOGIN - SCRIPT DE TESTE"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 18

local keyBox = Instance.new("TextBox", loginFrame)
keyBox.Size = UDim2.new(0.8, 0, 0, 40)
keyBox.Position = UDim2.new(0.1, 0, 0.5, -10)
keyBox.PlaceholderText = "Digite sua Key"
keyBox.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
keyBox.TextColor3 = Color3.fromRGB(255, 255, 255)
keyBox.Font = Enum.Font.Gotham
keyBox.TextSize = 16
Instance.new("UICorner", keyBox).CornerRadius = UDim.new(0, 8)

local loginButton = Instance.new("TextButton", loginFrame)
loginButton.Size = UDim2.new(0.5, 0, 0, 35)
loginButton.Position = UDim2.new(0.25, 0, 0.8, 0)
loginButton.BackgroundColor3 = Color3.fromRGB(50, 50, 255)
loginButton.Text = "ENTRAR"
loginButton.TextColor3 = Color3.new(1, 1, 1)
loginButton.Font = Enum.Font.GothamBold
loginButton.TextSize = 16
Instance.new("UICorner", loginButton)

-- ESP
function loadESP()
    while ESPEnabled do
        for _, v in ipairs(game:GetService("Players"):GetPlayers()) do
            if v ~= game.Players.LocalPlayer and v.Team ~= game.Players.LocalPlayer.Team then
                if v.Character and not v.Character:FindFirstChildOfClass("Highlight") then
                    local highlight = Instance.new("Highlight", v.Character)
                    highlight.FillColor = Color3.fromRGB(255, 0, 0)
                    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                    highlight.FillTransparency = 0.5
                    highlight.OutlineTransparency = 0
                end
            end
        end
        task.wait(1)
    end
end

-- AIMBOT
function loadAimbot()
    local Players = game:GetService("Players")
    local LocalPlayer = Players.LocalPlayer
    local RunService = game:GetService("RunService")
    local Camera = workspace.CurrentCamera

    RunService.RenderStepped:Connect(function()
        if not AimbotEnabled then return end
        local closestEnemy, shortestDistance = nil, math.huge
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Team ~= LocalPlayer.Team and player.Character and player.Character:FindFirstChild("Head") then
                local headPos, onScreen = Camera:WorldToViewportPoint(player.Character.Head.Position)
                local distance = (Camera.CFrame.Position - player.Character.Head.Position).Magnitude
                if onScreen and distance < shortestDistance then
                    closestEnemy = player
                    shortestDistance = distance
                end
            end
        end
        if closestEnemy and closestEnemy.Character and closestEnemy.Character:FindFirstChild("Head") then
            Camera.CFrame = CFrame.new(Camera.CFrame.Position, closestEnemy.Character.Head.Position)
        end
    end)
end

-- INTERFACE PRINCIPAL
local function showMainUI()
    -- Créditos
    local credits = Instance.new("TextLabel", ScreenGui)
    credits.Text = "Feito por: Nicolas | v1.1"
    credits.Size = UDim2.new(0, 200, 0, 25)
    credits.Position = UDim2.new(0, 10, 1, -30)
    credits.BackgroundTransparency = 1
    credits.TextColor3 = Color3.fromRGB(255, 255, 255)
    credits.Font = Enum.Font.Gotham
    credits.TextSize = 14

    -- Botão ESP
    local espBtn = Instance.new("TextButton", ScreenGui)
    espBtn.Size = UDim2.new(0, 120, 0, 35)
    espBtn.Position = UDim2.new(0, 10, 1, -70)
    espBtn.BackgroundColor3 = Color3.fromRGB(80, 80, 200)
    espBtn.Text = "ESP: ON"
    espBtn.TextColor3 = Color3.new(1, 1, 1)
    espBtn.Font = Enum.Font.GothamBold
    espBtn.TextSize = 14
    Instance.new("UICorner", espBtn)

    espBtn.MouseButton1Click:Connect(function()
        ESPEnabled = not ESPEnabled
        espBtn.Text = "ESP: " .. (ESPEnabled and "ON" or "OFF")
    end)

    -- Botão Aimbot
    local aimBtn = Instance.new("TextButton", ScreenGui)
    aimBtn.Size = UDim2.new(0, 120, 0, 35)
    aimBtn.Position = UDim2.new(0, 140, 1, -70)
    aimBtn.BackgroundColor3 = Color3.fromRGB(80, 200, 80)
    aimBtn.Text = "Aimbot: ON"
    aimBtn.TextColor3 = Color3.new(1, 1, 1)
    aimBtn.Font = Enum.Font.GothamBold
    aimBtn.TextSize = 14
    Instance.new("UICorner", aimBtn)

    aimBtn.MouseButton1Click:Connect(function()
        AimbotEnabled = not AimbotEnabled
        aimBtn.Text = "Aimbot: " .. (AimbotEnabled and "ON" or "OFF")
    end)

    -- Botão Sair
    local exit = Instance.new("TextButton", ScreenGui)
    exit.Size = UDim2.new(0, 80, 0, 30)
    exit.Position = UDim2.new(1, -90, 1, -40)
    exit.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
    exit.Text = "SAIR"
    exit.TextColor3 = Color3.fromRGB(255, 255, 255)
    exit.Font = Enum.Font.GothamBold
    exit.TextSize = 14
    Instance.new("UICorner", exit)
    exit.MouseButton1Click:Connect(function()
        ScreenGui:Destroy()
    end)

    task.spawn(loadESP)
    loadAimbot()
end

-- VERIFICAR LOGIN
loginButton.MouseButton1Click:Connect(function()
    local key = keyBox.Text
    if keys[key] then
        playLoginSound()
        loginFrame:Destroy()
        showMainUI()
    else
        loginButton.Text = "KEY INVÁLIDA!"
        wait(1.5)
        loginButton.Text = "ENTRAR"
    end
end)
