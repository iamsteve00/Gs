
-- Creating the Main Interface
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")

ScreenGui.Name = "CustomScript"
ScreenGui.Parent = game.CoreGui

Frame.Name = "MainFrame"
Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(255, 182, 193) -- Theme color
Frame.Position = UDim2.new(0.5, -150, 0.5, -100)
Frame.Size = UDim2.new(0, 300, 0, 200)

-- Navigation Buttons
local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Parent = Frame
MinimizeButton.Text = "_"
MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
MinimizeButton.Position = UDim2.new(1, -40, 0, 10)

local CloseButton = Instance.new("TextButton")
CloseButton.Parent = Frame
CloseButton.Text = "X"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -40, 0, 50)

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Creating the Minimized Box
local MiniFrame = Instance.new("Frame")
MiniFrame.Parent = ScreenGui
MiniFrame.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
MiniFrame.Size = UDim2.new(0, 50, 0, 50)
MiniFrame.Position = UDim2.new(0, 20, 0, 20)
MiniFrame.Visible = false
MiniFrame.Active = true
MiniFrame.Draggable = true

local RestoreButton = Instance.new("TextButton")
RestoreButton.Parent = MiniFrame
RestoreButton.Text = "☰"
RestoreButton.Size = UDim2.new(1, 0, 1, 0)

MinimizeButton.MouseButton1Click:Connect(function()
    Frame.Visible = false
    MiniFrame.Visible = true
end)

RestoreButton.MouseButton1Click:Connect(function()
    Frame.Visible = true
    MiniFrame.Visible = false
end)

-- Creating Tabs
local function createTabButton(name, text, position)
    local tabButton = Instance.new("TextButton")
    tabButton.Name = name
    tabButton.Parent = Frame
    tabButton.Text = text
    tabButton.Size = UDim2.new(0, 80, 0, 30)
    tabButton.Position = position
    return tabButton
end

local TrollTab = createTabButton("TrollTab", "Troll", UDim2.new(0, 10, 0, 10))
local PlayerTab = createTabButton("PlayerTab", "Player", UDim2.new(0, 100, 0, 10))
local OthersTab = createTabButton("OthersTab", "Others", UDim2.new(0, 190, 0, 10))

-- Function to create "Back" buttons
local function createBackButton(parent)
    local BackButton = Instance.new("TextButton")
    BackButton.Parent = parent
    BackButton.Text = "Back"
    BackButton.Size = UDim2.new(0, 80, 0, 30)
    BackButton.Position = UDim2.new(0, 10, 1, -40)
    BackButton.MouseButton1Click:Connect(function()
        parent.Visible = false
        Frame.Visible = true
    end)
end

-- Creating Tabs
local function createTabFrame()
    local tabFrame = Instance.new("Frame")
    tabFrame.Parent = ScreenGui
    tabFrame.Size = UDim2.new(0, 300, 0, 250)
    tabFrame.Position = UDim2.new(0.5, -150, 0.5, -100)
    tabFrame.Visible = false
    createBackButton(tabFrame)
    return tabFrame
end

local TrollFrame = createTabFrame()
local MapFrame = createTabFrame()
local OthersFrame = createTabFrame()
local PlayerFrame = createTabFrame()

-- Showing Tabs when clicking
TrollTab.MouseButton1Click:Connect(function()
    Frame.Visible = false
    TrollFrame.Visible = true
end)

PlayerTab.MouseButton1Click:Connect(function()
    Frame.Visible = false
    PlayerFrame.Visible = true
end)

OthersTab.MouseButton1Click:Connect(function()
    Frame.Visible = false
    OthersFrame.Visible = true
end)

-- Move Objects Feature
local MoveObjectButton = Instance.new("TextButton")
MoveObjectButton.Parent = MapFrame
MoveObjectButton.Text = "Move Objects"
MoveObjectButton.Size = UDim2.new(0, 120, 0, 30)
MoveObjectButton.Position = UDim2.new(0, 140, 0, 50)

MoveObjectButton.MouseButton1Click:Connect(function()
    for _, obj in ipairs(game.Workspace:GetChildren()) do
        if obj:IsA("Model") and obj.PrimaryPart then
            obj:SetPrimaryPartCFrame(obj:GetPrimaryPartCFrame() * CFrame.new(math.random(-20, 20), 0, math.random(-20, 20)))
        end
    end
end)

-- Function to display a temporary message on players' screens
local function showTemporaryMessage(messageText, duration)
    for _, player in ipairs(game.Players:GetPlayers()) do
        local playerGui = player:FindFirstChildOfClass("PlayerGui")
        if playerGui then
            local messageLabel = Instance.new("TextLabel")
            messageLabel.Parent = playerGui
            messageLabel.Text = messageText
            messageLabel.Size = UDim2.new(0, 400, 0, 50)
            messageLabel.Position = UDim2.new(0.5, -200, 0.1, 0) -- Position adjusted
            messageLabel.BackgroundTransparency = 0.5
            messageLabel.TextScaled = true

            -- Timer to remove the message after the specified duration
            task.delay(duration, function()
                messageLabel:Destroy()
            end)
        end
    end
end

-- Calling the function when executing the script
showTemporaryMessage("Script executed successfully!", 3)
