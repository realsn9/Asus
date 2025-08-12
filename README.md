local Rayfield = loadstring(game: HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield: CreateWindow({
    Name = "Asus hub",
    Icon = nil,
    LoadingTitle = "Asus system",
    LoadingSubtitle = "by nut",
    Theme = "Default",
    ToggleUIKeybind = "K",

    DisableRayfieldPrompts = false,
    DisableBuildWarnings = false,

    ConfigurationSaving = {
        Enabled = true,
        FolderName = nil,
        FileName = "Aura Hub"
    },

    Discord = {
        Enabled = true,
        Invite = "JF2F2RANud",
        RememberJoins = true
    },

    KeySystem = true,
    KeySettings = {
        Title = "Aura Keys",
        Subtitle = "Key System",
        Note = "Para conseguir a key, entre no discord da Asus",
        FileName = "Key",
        SaveKey = true,
        GrabKeyFromSite = false,
        Key = {
            "asus",
            "Ore",
            "AsusHub"
        }
    }
})

local main = Window: CreateTab("main", 4483362458)
local system = Window: CreateTab("system", 4483362458)
local system = Window: CreateTab("sobre", 4483362458)

    --Função: Matar Jogador
local
function MatarJogador()
local player = game.Players.LocalPlayer
if player and player.Character and player.Character: FindFirstChild("Humanoid") then
player.Character.Humanoid.Health = 0
end
end

main: CreateButton({
        Name = "Matar Jogador",
        Callback = MatarJogador
    })

    --Toggle: Flash
main: CreateToggle({
        Name = "Flash",
        CurrentValue = false,
        Callback = function (Value)
        local player = game.Players.LocalPlayer
        if player and player.Character and player.Character: FindFirstChild("Humanoid") then
        player.Character.Humanoid.WalkSpeed = Value and 50 or 16
        end
        end
    })

    --Slider: JumpPower
main: CreateSlider({
        Name = "Pulo (JumpPower)",
        Range = {
            0,
            100
        },
        Increment = 1,
        Suffix = "",
        CurrentValue = 50,
        Callback = function (Value)
        local player = game.Players.LocalPlayer
        if player and player.Character and player.Character: FindFirstChild("Humanoid") then
        player.Character.Humanoid.JumpPower = Value
        end
        end
    })

    --Input: Nome do NPC
    main: CreateInput({
        Name = "Nome do NPC",
        PlaceholderText = "Digite um NPC...",
        RemoveTextAfterFocusLost = false,
        Callback = function (Text)
        --ação com texto digitado
        end
    })

    --Dropdown: Selecionar NPC
main: CreateDropdown({
        Name = "Selecionar NPC",
        Options = {
            "Npc 1",
            "Npc 2",
            "Npc 3"
        },
        CurrentOption = "Npc 1",
        Callback = function (Option)
        --ação com opção selecionada
        end
    })

    --Keybind: Exemplo
main: CreateKeybind({
        Name = "Atalho de Teclado",
        CurrentKeybind = "F",
        HoldToInteract = false,
        Callback = function (Key)
        --ação ao pressionar tecla
        end
    })

    --Parágrafo: Criador
main: CreateParagraph({
        Title = "Criador",
        Content = "Asus hub by nut"
    })

    --Mods: Fly com WASD + direção da câmera
local flying = false
local flySpeed = 50
local direction = Vector3.zero

local UIS = game: GetService("UserInputService")
local RunService = game: GetService("RunService")
local player = game.Players.LocalPlayer

UIS.InputBegan: Connect(function (input, gameProcessed) if gameProcessed then
    return end
    if input.KeyCode == Enum.KeyCode.W then direction += Vector3.new(0, 0, -1) end
    if input.KeyCode == Enum.KeyCode.S then direction += Vector3.new(0, 0, 1) end
    if input.KeyCode == Enum.KeyCode.A then direction += Vector3.new(-1, 0, 0) end
    if input.KeyCode == Enum.KeyCode.D then direction += Vector3.new(1, 0, 0) end
    if input.KeyCode == Enum.KeyCode.Space then direction += Vector3.new(0, 1, 0) end
    if input.KeyCode == Enum.KeyCode.LeftControl then direction += Vector3.new(0, -1, 0) end end)

UIS.InputEnded: Connect(function (input) if input.KeyCode == Enum.KeyCode.W then direction -= Vector3.new(0, 0, -1) end
    if input.KeyCode == Enum.KeyCode.S then direction -= Vector3.new(0, 0, 1) end
    if input.KeyCode == Enum.KeyCode.A then direction -= Vector3.new(-1, 0, 0) end
    if input.KeyCode == Enum.KeyCode.D then direction -= Vector3.new(1, 0, 0) end
    if input.KeyCode == Enum.KeyCode.Space then direction -= Vector3.new(0, 1, 0) end
    if input.KeyCode == Enum.KeyCode.LeftControl then direction -= Vector3.new(0, -1, 0) end end)

RunService.RenderStepped: Connect(function () if flying then local character = player.Character
    if character and character: FindFirstChild("HumanoidRootPart") then local hrp = character.HumanoidRootPart local cam = workspace.CurrentCamera local moveDirection = cam.CFrame: VectorToWorldSpace(direction) hrp.Velocity = moveDirection * flySpeed character: FindFirstChild("Humanoid").PlatformStand = true end
    else
        local character = player.Character
    if character and character: FindFirstChild("Humanoid") then character.Humanoid.PlatformStand = false end end end)

main: CreateToggle({
    Name = "Fly (WASD + Olhar)",
    CurrentValue = false,
    Callback = function (Value)
    flying = Value
    end
})

main: CreateSlider({
        Name = "Velocidade do Fly",
        Range = {
            10,
            200
        },
        Increment = 5,
        Suffix = " u/s",
        CurrentValue = 50,
        Callback = function (Value)
        flySpeed = Value
        end
    })

    --Mods: ESP
local
function ToggleESP(state)
for _, player in pairs(game.Players: GetPlayers()) do
    if player~ = game.Players.LocalPlayer and player.Character then
local highlight = player.Character: FindFirstChild("Highlight")
if state then
if not highlight then
highlight = Instance.new("Highlight")
highlight.FillColor = Color3.fromRGB(255, 0, 0)
highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
highlight.Parent = player.Character
end
else
if highlight then
highlight: Destroy()
end
end
end
end
end

main: CreateToggle({
        Name = "ESP",
        CurrentValue = false,
        Callback = function (Value)
        ToggleESP(Value)
        end
    })

    --Mods: Noclip
local noclip = false

game: GetService("RunService").Stepped: Connect(function () if noclip then local character = game.Players.LocalPlayer.Character
    if character then
    for _, part in pairs(character: GetDescendants()) do
        if part:
    IsA("BasePart") then part.CanCollide = false end end end end end)

main: CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Callback = function (Value)
    noclip = Value
    end
})

Rayfield: LoadConfiguration()
