local players = game:GetService("Players")
local localPlayer = players.LocalPlayer
local camera = workspace.CurrentCamera

local function createESP(player)
    local box = Drawing.new("Square")
    box.Thickness = 2
    box.Color = Color3.new(1, 0, 0) -- Rojo
    box.Filled = false

    local function update()
        local character = player.Character
        local rootPart = character and character:FindFirstChild("HumanoidRootPart")
        if character and rootPart and player ~= localPlayer then
            local vector, onScreen = camera:WorldToViewportPoint(rootPart.Position)
            if onScreen then
                box.Size = Vector2.new(100, 100) -- Ajusta el tamaño del contorno
                box.Position = Vector2.new(vector.X - 50, vector.Y - 50) -- Centra el contorno
                box.Visible = true
            else
                box.Visible = false
            end
        else
            box.Visible = false
        end
    end

    game:GetService("RunService").RenderStepped:Connect(update)
end

for _, player in ipairs(players:GetPlayers()) do
    if player ~= localPlayer then
        createESP(player)
    end
end

players.PlayerAdded:Connect(function(player)
    createESP(player)
end) 
