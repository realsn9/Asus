# Asus
Hhhh
local player = game.Players.LocalPlayer
repeat wait() until player and player.Character and player:FindFirstChild("PlayerGui")

local gui = Instance.new("ScreenGui", player.PlayerGui)
gui.Name = "NicolasGUI"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 400, 0, 350)
frame.Position = UDim2.new(0.5, -200, 0.5, -175)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.BorderSizePixel = 0

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, -40, 0, 40)
title.Position = UDim2.new(0, 10, 0, 0)
title.Text = "Menu do Nicolas"
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 22

-- Botão Fechar
local closeBtn = Instance.new("TextButton", frame)
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -35, 0, 5)
closeBtn.Text = "X"
closeBtn.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
closeBtn.TextColor3 = Color3.new(1,1,1)
closeBtn.MouseButton1Click:Connect(function()
    frame.Visible = false
end)

-- Botão Redimensionar
local resizeBtn = Instance.new("TextButton", frame)
resizeBtn.Size = UDim2.new(0, 30, 0, 30)
resizeBtn.Position = UDim2.new(1, -70, 0, 5)
resizeBtn.Text = "↔"
resizeBtn.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
resizeBtn.TextColor3 = Color3.new(1,1,1)
local expanded = true
resizeBtn.MouseButton1Click:Connect(function()
    expanded = not expanded
    frame.Size = expanded and UDim2.new(0, 400, 0, 350) or UDim2.new(0, 200, 0, 200)
end)

-- Funções
local flying = false
local flyPart = nil
local noclipActive = false
local espActive = false
local defaultSpeed = 16
local defaultJump = 50

function fly()
    if flying then return end
    flying = true
    flyPart = Instance.new("BodyVelocity")
    flyPart.Velocity = Vector3.new(0, 0, 0)
    flyPart.MaxForce = Vector3.new(100000, 100000, 100000)
    flyPart.Parent = player.Character.HumanoidRootPart

    game:GetService("RunService").RenderStepped:Connect(function()
        if flying then
            flyPart.Velocity = player:GetMouse().Hit.lookVector * 50
        end
    end)
end

function unfly()
    flying = false
    if flyPart then flyPart:Destroy() flyPart = nil end
end

function noclip()
    noclipActive = true
    game:GetService("RunService").Stepped:Connect(function()
        if noclipActive then
            for _, part in pairs(player.Character:GetDescendants()) do
                if part:IsA("BasePart") then part.CanCollide = false end
            end
        end
    end)
end

function clip()
    noclipActive = false
    for _, part in pairs(player.Character:GetDescendants()) do
        if part:IsA("BasePart") then part.CanCollide = true end
    end
end

function esp()
    espActive = true
    for _, plr in pairs(game.Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local billboard = Instance.new("BillboardGui", plr.Character.HumanoidRootPart)
            billboard.Name = "ESP"
            billboard.Size = UDim2.new(0, 100, 0, 40)
            billboard.AlwaysOnTop = true
            local label = Instance.new("TextLabel", billboard)
            label.Size = UDim2.new(1, 0, 1, 0)
            label.Text = plr.Name
            label.TextColor3 = Color3.new(1,1,1)
            label.BackgroundTransparency = 1
        end
    end
end

function unesp()
    for _, plr in pairs(game.Players:GetPlayers()) do
        if plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local espGui = plr.Character.HumanoidRootPart:FindFirstChild("ESP")
            if espGui then espGui:Destroy() end
        end
    end
end

-- Botões
local function createButton(text, posY, callback)
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(0, 180, 0, 30)
    btn.Position = UDim2.new(0, 10, 0, posY)
    btn.Text = text
    btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 14
    btn.MouseButton1Click:Connect(callback)
end

createButton("Fly", 50, fly)
createButton("Unfly", 85, unfly)
createButton("ESP", 120, esp)
createButton("UnESP", 155, unesp)
createButton("Noclip", 190, noclip)
createButton("Clip", 225, clip)

-- Sliders
local function createSlider(labelText, posY, defaultValue, onChange)
    local label = Instance.new("TextLabel", frame)
    label.Size = UDim2.new(0, 180, 0, 20)
    label.Position = UDim2.new(0, 10, 0, posY)
    label.Text = labelText
    label.TextColor3 = Color3.new(1,1,1)
    label.BackgroundTransparency = 1
    label.Font = Enum.Font.Gotham
    label.TextSize = 14

    local box = Instance.new("TextBox", frame)
    box.Size = UDim2.new(0, 180, 0, 30)
    box.Position = UDim2.new(0, 10, 0, posY + 25)
    box.Text = tostring(defaultValue)
    box.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    box.TextColor3 = Color3.new(1,1,1)
    box.Font = Enum.Font.Gotham
    box.TextSize = 14
    box.FocusLost:Connect(function()
        local val = tonumber(box.Text)
        if val then onChange(val) end
    end)
end

createSlider("Speed", 260, defaultSpeed, function(val)
    player.Character.Humanoid.WalkSpeed = val
end)

createSlider("JumpPower", 315, defaultJump, function(val)
    player.Character.Humanoid.JumpPower = val
end)
