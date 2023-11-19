local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local mobileInput = game:GetService("MobileInput")

local gui = Instance.new("ScreenGui")
gui.Name = "SCANNER BALL"
gui.Parent = player.PlayerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 100)
frame.Position = UDim2.new(0, 10, 0, 10)
frame.BackgroundColor3 = Color3.new(0, 0, 0)
frame.BackgroundTransparency = 0.5
frame.Parent = gui

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 30)
titleLabel.Text = "SCANNER BALL Controls"
titleLabel.Font = Enum.Font.SourceSansBold
titleLabel.TextSize = 18
titleLabel.TextColor3 = Color3.new(1, 1, 1)
titleLabel.Parent = frame

local autoParryToggle = Instance.new("TextLabel")
autoParryToggle.Size = UDim2.new(1, 0, 0, 20)
autoParryToggle.Position = UDim2.new(0, 0, 0, 30)
autoParryToggle.Text = "Auto Parry: ON"
autoParryToggle.Font = Enum.Font.SourceSans
autoParryToggle.TextSize = 16
autoParryToggle.TextColor3 = Color3.new(1, 1, 1)
autoParryToggle.Parent = frame

local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(1, 0, 0, 20)
toggleButton.Position = UDim2.new(0, 0, 0, 60)
toggleButton.Text = "Toggle Auto Parry"
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.TextSize = 16
toggleButton.Parent = frame

local parryPart = Instance.new("Part")
parryPart.Anchored, parryPart.CanCollide, parryPart.BrickColor, parryPart.Transparency = true, false, BrickColor.new("Bright red"), 0.5
parryPart.Parent = character

local autoParry = true
local maxAutoParryUses = 5
local autoParryUses = 0
local parryCooldown = 1

local function updateAutoParryLabel()
    autoParryToggle.Text = "Auto Parry: " .. (autoParry and "ON" or "OFF")
end

local function canUseAutoParry()
    return autoParry and autoParryUses < maxAutoParryUses
end

local function handleToggleAutoParry()
    if canUseAutoParry() then
        autoParry = not autoParry
        autoParryUses = autoParryUses + 1
        updateAutoParryLabel()
    else
        warn("Maximum Auto Parry uses reached.")
    end
end

local function onTouch(input)
    if input.UserInputType == Enum.UserInputType.Touch and canUseAutoParry() then
        for _, ball in pairs(game.Workspace:WaitForChild("Balls"):GetChildren()) do
            if ball:IsA("Ball") then
                local distance = (ball.Position - humanoid.Parent.Position).Magnitude
                if distance < 15 then
                    local reflectDirection = (parryPart.Position - ball.Position).unit
                    local reflectVelocity = reflectDirection * ball.Velocity.magnitude
                    ball.Velocity = reflectVelocity
                end
            end
        end
    end
end

local function handleAutoParry()
    while wait(parryCooldown) do
        if canUseAutoParry() then
            local closestBall = nil
            local closestDistance = math.huge

            for _, ball in pairs(game.Workspace:WaitForChild("Balls"):GetChildren()) do
                if ball:IsA("Ball") then
                    local distance = (ball.Position - humanoid.Parent.Position).Magnitude
                    if distance < closestDistance then
                        closestDistance, closestBall = distance, ball
                    end
                end
            end

            if closestBall then
                local reflectDirection = (parryPart.Position - closestBall.Position).unit
                local reflectVelocity = reflectDirection * closestBall.Velocity.magnitude
                closestBall.Velocity = reflectVelocity
                autoParryUses = autoParryUses + 1
                updateAutoParryLabel()
            end
        end
    end
end

toggleButton.MouseButton1Click:Connect(handleToggleAutoParry)
mobileInput.TouchTap:Connect(onTouch)
spawn(handleAutoParry)

updateAutoParryLabel()
