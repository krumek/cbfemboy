-- Counter Blox CHH Script (Femboy Edition)
-- Author: xAI Grok 3
-- Version: 1.1.0
-- Last Updated: August 16, 2025, 06:00 PM CEST
-- License: MIT

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local StarterGui = game:GetService("StarterGui")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer
local Workspace = game:GetService("Workspace")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "FemboyCHH"
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.Enabled = true

-- Notification System
local function notifyMsg(msg, duration)
    pcall(function()
        StarterGui:SetCore("SendNotification", {
            Title = "Femboy CHH",
            Text = msg,
            Duration = duration or 3
        })
    end)
end

notifyMsg("Femboy CHH загружен! 💙", 3)

-- GUI Creation (Dark Pink Color)
local GuiFrame = Instance.new("Frame")
GuiFrame.Size = UDim2.new(0, 250, 0, 300)
GuiFrame.Position = UDim2.new(0.5, -125, 0.5, -150)
GuiFrame.BackgroundColor3 = Color3.fromRGB(199, 21, 133) -- Dark pink
GuiFrame.BorderSizePixel = 0
GuiFrame.BackgroundTransparency = 0.2
GuiFrame.Parent = ScreenGui
GuiFrame.Visible = true
GuiFrame.Active = true
GuiFrame.Draggable = true

local UiCorner = Instance.new("UICorner")
UiCorner.CornerRadius = UDim.new(0, 15)
UiCorner.Parent = GuiFrame

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 50)
Title.BackgroundTransparency = 1
Title.Text = "Femboy CHH Menu 💕"
Title.TextColor3 = Color3.fromRGB(255, 255, 255) -- White text for contrast
Title.Font = Enum.Font.GothamBold
Title.TextSize = 18
Title.Parent = GuiFrame

local ButtonLayout = Instance.new("UIListLayout")
ButtonLayout.Padding = UDim.new(0, 10)
ButtonLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
ButtonLayout.Parent = GuiFrame

local function createButton(text, action)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(0, 200, 0, 45)
    Button.BackgroundColor3 = Color3.fromRGB(219, 112, 147) -- Pale pink for buttons
    Button.Text = text
    Button.TextColor3 = Color3.fromRGB(255, 245, 238) -- Soft pink text
    Button.Font = Enum.Font.Gotham
    Button.TextSize = 14
    Button.Parent = GuiFrame
    Button.MouseButton1Click:Connect(action)
    local ButtonCorner = Instance.new("UICorner")
    ButtonCorner.CornerRadius = UDim.new(0, 10)
    ButtonCorner.Parent = Button
    return Button
end

-- State Variables
local isAimbotOn = false
local isESPOn = false

-- Custom Binding
local customBindKey = Enum.KeyCode.RightControl
local bindButton = createButton("Bind GUI Key (Current: RightControl)", function()
    notifyMsg("Press a key to bind...", 5)
    local connection
    connection = UserInputService.InputBegan:Connect(function(input)
        if input.KeyCode ~= Enum.KeyCode.Unknown then
            customBindKey = input.KeyCode
            notifyMsg("GUI Key bound to " .. input.KeyCode.Name, 3)
            bindButton.Text = "Bind GUI Key (Current: " .. input.KeyCode.Name .. ")"
            connection:Disconnect()
        end
    end)
end)

-- Toggle GUI
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == customBindKey then
        guiVisible = not guiVisible
        GuiFrame.Visible = guiVisible
        notifyMsg("Femboy Menu: " .. (guiVisible and "Открыт 💙" or "Закрыт"), 2)
    end
end)

-- Aimbot Function
local function toggleAimbot()
    isAimbotOn = not isAimbotOn
    notifyMsg("Aimbot: " .. (isAimbotOn and "Включён 💕" or "Выключён"), 2)
    if isAimbotOn then
        RunService.RenderStepped:Connect(function()
            if isAimbotOn then
                local target = nil
                local minDistance = math.huge
                for _, player in ipairs(Players:GetPlayers()) do
                    if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") and player.Team ~= LocalPlayer.Team then
                        local head = player.Character.Head
                        local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)
                        local distance = (Vector2.new(screenPos.X, screenPos.Y) - Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)).Magnitude
                        if onScreen and distance < minDistance then
                            minDistance = distance
                            target = head
                        end
                    end
                end
                if target then
                    Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Position)
                end
            end
        end)
    end
end

-- ESP Function
local espDrawings = {}
local function toggleESP()
    isESPOn = not isESPOn
    notifyMsg("ESP: " .. (isESPOn and "Включён 💙" or "Выключён"), 2)
    if not isESPOn then
        for _, drawing in pairs(espDrawings) do
            drawing:Remove()
        end
        espDrawings = {}
    else
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") and player.Team ~= LocalPlayer.Team then
                local box = Drawing.new("Square")
                box.Thickness = 2
                box.Color = Color3.fromRGB(135, 206, 235) -- Голубой контур
                box.Filled = false
                espDrawings[player] = box
                player.CharacterRemoving:Connect(function()
                    if espDrawings[player] then
                        espDrawings[player]:Remove()
                        espDrawings[player] = nil
                    end
                end)
            end
        end
    end
end

-- Update ESP
RunService.RenderStepped:Connect(function()
    if isESPOn then
        for player, box in pairs(espDrawings) do
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                local root = player.Character.HumanoidRootPart
                local headPos, onScreen = Camera:WorldToViewportPoint(root.Position + Vector3.new(0, 3, 0))
                local footPos = Camera:WorldToViewportPoint(root.Position - Vector3.new(0, 3, 0))
                if onScreen then
                    box.Visible = true
                    box.Size = Vector2.new(2000 / headPos.Z, footPos.Y - headPos.Y)
                    box.Position = Vector2.new(headPos.X - box.Size.X / 2, headPos.Y)
                else
                    box.Visible = false
                end
            end
        end
    end
end)

-- Buttons
createButton("Toggle Aimbot 💙", toggleAimbot)
createButton("Toggle ESP 💕", toggleESP)
createButton("Close Menu", function() GuiFrame.Visible = false; notifyMsg("Femboy Menu закрыт 💙", 2) end)

-- Respawn Fix for ESP
LocalPlayer.CharacterAdded:Connect(function()
    if isESPOn then toggleESP(); toggleESP() end -- Reset ESP on respawn
end)

Players.PlayerAdded:Connect(function(player)
    if isESPOn and player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") and player.Team ~= LocalPlayer.Team then
        local box = Drawing.new("Square")
        box.Thickness = 2
        box.Color = Color3.fromRGB(135, 206, 235)
        box.Filled = false
        espDrawings[player] = box
        player.CharacterRemoving:Connect(function()
            if espDrawings[player] then
                espDrawings[player]:Remove()
                espDrawings[player] = nil
            end
        end)
    end
end)

-- Skin Changer
local skinButton = createButton("Skin Changer", function()
    notifyMsg("Skin Changer: Coming soon! 💙", 3)
end)

-- Arm Change
local armButton = createButton("Change Arms", function()
    notifyMsg("Arm Change: Coming soon! 💕", 3)
end)

-- Character Model Change (Femboy/Boykisser)
local modelButton = createButton("Femboy/Boykisser Model", function()
    notifyMsg("Model Change: Coming soon! 💙", 3)
end)

-- Initial Setup
end
