local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local FrameStroke = Instance.new("UIStroke")
local FrameCorner = Instance.new("UICorner")
local Header = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local MinBtn = Instance.new("TextButton")

-- Global States
local currentWorld = 1
local loops = { [1] = false, [2] = false }
local autoSellEnabled = false
local maxStorage = 11

-- Config Data Mapping
local WORLD_DATA = {
    [1] = {
        END_POS = Vector3.new(-7.12, 3.41, -1425.03),
        RETRIEVE_POS = Vector3.new(-32.00, 4.27, 137.00),
        SELL_POS = Vector3.new(-45.49, 3.31, 122.47),
        RARITIES = { ["Secret"] = true, ["Godly"] = true }
    },
    [2] = {
        END_POS = Vector3.new(1994.88, 3.41, -1462.65),
        RETRIEVE_POS = Vector3.new(1999.46, 4.27, 127.75),
        SELL_POS = Vector3.new(1954.81, 3.41, 123.94),
        RARITIES = { ["Transcendent"] = true, ["Eternal"] = true }
    }
}

-- Engine Instances
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local InventoryRemote = game:GetService("ReplicatedStorage"):WaitForChild("SandShovel"):WaitForChild("Remotes"):WaitForChild("InventoryAction")
local LocalArtifacts = workspace:WaitForChild("LocalArtifacts")

ScreenGui.Name = "JanRepublic_Suite"
ScreenGui.Parent = game:GetService("CoreGui")
ScreenGui.ResetOnSpawn = false

-- Glassmorphism Base Window Panel
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 18, 24)
MainFrame.BackgroundTransparency = 0.25
MainFrame.Position = UDim2.new(0.05, 0, 0.25, 0)
MainFrame.Size = UDim2.new(0, 260, 0, 390)
MainFrame.Active = true
MainFrame.Draggable = true

FrameCorner.CornerRadius = UDim.new(0, 10)
FrameCorner.Parent = MainFrame

FrameStroke.Color = Color3.fromRGB(0, 220, 255)
FrameStroke.Thickness = 1.5
FrameStroke.Transparency = 0.4
FrameStroke.Parent = MainFrame

-- Header Panel Design
Header.Name = "Header"
Header.Parent = MainFrame
Header.BackgroundColor3 = Color3.fromRGB(0, 220, 255)
Header.BackgroundTransparency = 0.85
Header.Size = UDim2.new(1, 0, 0, 40)

local HeaderCorner = Instance.new("UICorner")
HeaderCorner.CornerRadius = UDim.new(0, 10)
HeaderCorner.Parent = Header

Title.Name = "Title"
Title.Parent = Header
Title.Size = UDim2.new(1, -40, 1, 0)
Title.Position = UDim2.new(0, 12, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "JAN REPUBLIC // OS"
Title.TextColor3 = Color3.fromRGB(0, 220, 255)
Title.TextSize = 14
Title.Font = Enum.Font.Code
Title.TextXAlignment = Enum.TextXAlignment.Left

MinBtn.Name = "MinBtn"
MinBtn.Parent = Header
MinBtn.Size = UDim2.new(0, 26, 0, 26)
MinBtn.Position = UDim2.new(1, -33, 0, 7)
MinBtn.BackgroundColor3 = Color3.fromRGB(20, 25, 35)
MinBtn.Text = "X"
MinBtn.TextColor3 = Color3.fromRGB(0, 220, 255)
MinBtn.TextSize = 12
MinBtn.Font = Enum.Font.Code

local MinCorner = Instance.new("UICorner")
MinCorner.CornerRadius = UDim.new(0, 6)
MinCorner.Parent = MinBtn

local MinStroke = Instance.new("UIStroke")
MinStroke.Color = Color3.fromRGB(0, 220, 255)
MinStroke.Thickness = 1
MinStroke.Parent = MinBtn

-- Navigation Tabs (World Controls Selector)
local TabFrame = Instance.new("Frame")
TabFrame.Parent = MainFrame
TabFrame.Size = UDim2.new(1, -20, 0, 30)
TabFrame.Position = UDim2.new(0, 10, 0, 50)
TabFrame.BackgroundTransparency = 1

local W1Tab = Instance.new("TextButton")
W1Tab.Size = UDim2.new(0.5, -4, 1, 0)
W1Tab.BackgroundColor3 = Color3.fromRGB(0, 220, 255)
W1Tab.BackgroundTransparency = 0.4
W1Tab.Text = "WORLD 01"
W1Tab.TextColor3 = Color3.fromRGB(255, 255, 255)
W1Tab.Font = Enum.Font.Code
W1Tab.TextSize = 11
W1Tab.Parent = TabFrame
Instance.new("UICorner", W1Tab).CornerRadius = UDim.new(0, 4)

local W2Tab = Instance.new("TextButton")
W2Tab.Size = UDim2.new(0.5, -4, 1, 0)
W2Tab.Position = UDim2.new(0.5, 4, 0, 0)
W2Tab.BackgroundColor3 = Color3.fromRGB(20, 25, 35)
W2Tab.BackgroundTransparency = 0.2
W2Tab.Text = "WORLD 02"
W2Tab.TextColor3 = Color3.fromRGB(0, 220, 255)
W2Tab.Font = Enum.Font.Code
W2Tab.TextSize = 11
W2Tab.Parent = TabFrame
Instance.new("UICorner", W2Tab).CornerRadius = UDim.new(0, 4)
local W2Stroke = Instance.new("UIStroke", W2Tab)
W2Stroke.Color = Color3.fromRGB(0, 220, 255)
W2Stroke.Thickness = 1
W2Stroke.Transparency = 0.5

-- Main Interactive Content Containers
local Container = Instance.new("Frame")
Container.Parent = MainFrame
Container.Size = UDim2.new(1, -20, 1, -100)
Container.Position = UDim2.new(0, 10, 0, 90)
Container.BackgroundTransparency = 1

local UIList = Instance.new("UIListLayout")
UIList.Parent = Container
UIList.SortOrder = Enum.SortOrder.LayoutOrder
UIList.Padding = UDim.new(0, 8)
UIList.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Helper for Tech Interactive Fields
local function createButton(text, order)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 36)
    btn.BackgroundColor3 = Color3.fromRGB(20, 25, 35)
    btn.BackgroundTransparency = 0.3
    btn.Text = text
    btn.TextColor3 = Color3.fromRGB(0, 220, 255)
    btn.Font = Enum.Font.Code
    btn.TextSize = 12
    btn.LayoutOrder = order
    btn.Parent = Container
    
    local c = Instance.new("UICorner", btn)
    c.CornerRadius = UDim.new(0, 6)
    local s = Instance.new("UIStroke", btn)
    s.Color = Color3.fromRGB(0, 220, 255)
    s.Thickness = 1
    s.Transparency = 0.6
    
    return btn
end

local loopBtn = createButton("AUTO LOOP RUN: OFF", 1)
local sellBtn = createButton("AUTO MERCHANT SELL: OFF", 2)

-- Storage Config Input Box Row
local InputRow = Instance.new("Frame")
InputRow.Size = UDim2.new(1, 0, 0, 36)
InputRow.BackgroundTransparency = 1
InputRow.LayoutOrder = 3
InputRow.Parent = Container

local InputLabel = Instance.new("TextLabel")
InputLabel.Size = UDim2.new(0.6, 0, 1, 0)
InputLabel.BackgroundTransparency = 1
InputLabel.Text = "MAX STORAGE LIMIT:"
InputLabel.TextColor3 = Color3.fromRGB(150, 170, 190)
InputLabel.Font = Enum.Font.Code
InputLabel.TextSize = 11
InputLabel.TextXAlignment = Enum.TextXAlignment.Left
InputLabel.Parent = InputRow

local TextBox = Instance.new("TextBox")
TextBox.Size = UDim2.new(0.4, 0, 1, 0)
TextBox.Position = UDim2.new(0.6, 0, 0, 0)
TextBox.BackgroundColor3 = Color3.fromRGB(15, 18, 24)
TextBox.Text = "11"
TextBox.TextColor3 = Color3.fromRGB(255, 255, 255)
TextBox.Font = Enum.Font.Code
TextBox.TextSize = 12
TextBox.Parent = InputRow
Instance.new("UICorner", TextBox).CornerRadius = UDim.new(0, 6)
local BoxStroke = Instance.new("UIStroke", TextBox)
BoxStroke.Color = Color3.fromRGB(0, 220, 255)
BoxStroke.Thickness = 1
BoxStroke.Transparency = 0.7

-- Live Monitoring List Monitor
local ScrollingFrame = Instance.new("ScrollingFrame")
ScrollingFrame.Size = UDim2.new(1, 0, 0, 140)
ScrollingFrame.BackgroundColor3 = Color3.fromRGB(10, 12, 16)
ScrollingFrame.BackgroundTransparency = 0.5
ScrollingFrame.BorderSizePixel = 0
ScrollingFrame.LayoutOrder = 4
ScrollingFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
ScrollingFrame.Parent = Container
Instance.new("UICorner", ScrollingFrame).CornerRadius = UDim.new(0, 6)
local ScrollStroke = Instance.new("UIStroke", ScrollingFrame)
ScrollStroke.Color = Color3.fromRGB(0, 220, 255)
ScrollStroke.Thickness = 1
ScrollStroke.Transparency = 0.8

local ScrollList = Instance.new("UIListLayout", ScrollingFrame)
ScrollList.Padding = UDim.new(0, 4)

-- UI Control Interaction Functions
local isMinimized = false
MinBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    if isMinimized then
        MainFrame.Size = UDim2.new(0, 260, 0, 40)
        Container.Visible = false
        TabFrame.Visible = false
        MinBtn.Text = "+"
    else
        MainFrame.Size = UDim2.new(0, 260, 0, 390)
        Container.Visible = true
        TabFrame.Visible = true
        MinBtn.Text = "X"
    end
end)

local function updateTabVisuals()
    if currentWorld == 1 then
        W1Tab.BackgroundColor3 = Color3.fromRGB(0, 220, 255)
        W1Tab.BackgroundTransparency = 0.4
        W1Tab.TextColor3 = Color3.fromRGB(255, 255, 255)
        W2Tab.BackgroundColor3 = Color3.fromRGB(20, 25, 35)
        W2Tab.BackgroundTransparency = 0.2
        W2Tab.TextColor3 = Color3.fromRGB(0, 220, 255)
    else
        W2Tab.BackgroundColor3 = Color3.fromRGB(0, 220, 255)
        W2Tab.BackgroundTransparency = 0.4
        W2Tab.TextColor3 = Color3.fromRGB(255, 255, 255)
        W1Tab.BackgroundColor3 = Color3.fromRGB(20, 25, 35)
        W1Tab.BackgroundTransparency = 0.2
        W1Tab.TextColor3 = Color3.fromRGB(0, 220, 255)
    end
    loopBtn.Text = "AUTO LOOP RUN: " .. (loops[currentWorld] and "ON" or "OFF")
    loopBtn.BackgroundColor3 = loops[currentWorld] and Color3.fromRGB(0, 80, 50) or Color3.fromRGB(20, 25, 35)
end

W1Tab.MouseButton1Click:Connect(function() currentWorld = 1 updateTabVisuals() end)
W2Tab.MouseButton1Click:Connect(function() currentWorld = 2 updateTabVisuals() end)

TextBox.FocusLost:Connect(function()
    local val = tonumber(TextBox.Text)
    if val then maxStorage = val else TextBox.Text = tostring(maxStorage) end
end)

-- [[ AUTOMATION CORE CODE ENGINE ]]

local function updateWorldModalState(isDisabled)
    pcall(function()
        local theWorldFrame = LocalPlayer:WaitForChild("PlayerGui"):FindFirstChild("TheWorld")
        if theWorldFrame then
            theWorldFrame.Enabled = not isDisabled
        end
    end)
end

-- World Navigation Verification
local function verifyAndSwitchWorld(targetWorld)
    if targetWorld == 2 then
        pcall(function()
            local worldsGui = LocalPlayer.PlayerGui:FindFirstChild("Worlds")
            if worldsGui then
                local w2Frame = worldsGui:FindFirstChild("World 2") or worldsGui:FindFirstChild("Container", true)
                local goBtn = w2Frame and w2Frame:FindFirstChild("GO", true)
                if goBtn and goBtn:IsA("TextButton") then
                    local connections = getconnections(goBtn.MouseButton1Click)
                    for _, conn in ipairs(connections) do conn:Fire() end
                end
            end
        end)
    end
end

local function equipShovel()
    local backpack = LocalPlayer:FindFirstChild("Backpack")
    local character = LocalPlayer.Character
    if backpack and character then
        for _, tool in ipairs(backpack:GetChildren()) do
            if tool:IsA("Tool") and string.find(tool.Name, "Shovel") then
                tool.Parent = character
                break
            end
        end
    end
end

local function clickArtifact(artifact)
    local cd = artifact:FindFirstChildWhichIsA("ClickDetector", true)
    if cd then fireclickdetector(cd) return true end
    local prompt = artifact:FindFirstChildWhichIsA("ProximityPrompt", true)
    if prompt then fireproximityprompt(prompt) return true end
    return false
end

-- Main Run Engine Loop
local function runTacticalSequence(worldId)
    local config = WORLD_DATA[worldId]
    updateWorldModalState(true)
    
    if worldId == 2 then verifyAndSwitchWorld(2) end
    
    -- Waypoint to farming region
    local character = LocalPlayer.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        character.HumanoidRootPart.CFrame = CFrame.new(config.END_POS)
        task.wait(0.5)
    end
    
    local collectedCount = 0
    
    for _, artifact in ipairs(LocalArtifacts:GetChildren()) do
        if not loops[worldId] then break end
        
        local rarity = artifact:GetAttribute("Rarity") or (artifact:FindFirstChild("Rarity") and artifact.Rarity.Value)
        if rarity and config.RARITIES[rarity] then
            equipShovel()
            local targetCFrame = artifact:GetPivot()
            local root = character and character:FindFirstChild("HumanoidRootPart")
            
            if root and targetCFrame then
                root.CFrame = targetCFrame * CFrame.new(0, 1.5, 0)
                task.wait(0.2)
                clickArtifact(artifact)
                collectedCount = collectedCount + 1
                task.wait(0.3)
                
                -- Check constraints threshold
                if collectedCount >= maxStorage then
                    break
                end
            end
        end
    end
    
    -- Processing/Merchant Drop Sequence
    if collectedCount > 0 and loops[worldId] then
        task.wait(0.2)
        if character and character:FindFirstChild("HumanoidRootPart") then
            character.HumanoidRootPart.CFrame = CFrame.new(config.RETRIEVE_POS)
            task.wait(0.3)
            character.HumanoidRootPart.CFrame = CFrame.new(config.SELL_POS)
            task.wait(0.4)
            InventoryRemote:InvokeServer("SellAll")
            task.wait(0.5)
        end
    end
end

-- Toggles Hooking Scripts
loopBtn.MouseButton1Click:Connect(function()
    loops[currentWorld] = not loops[currentWorld]
    if not loops[currentWorld] then
        updateWorldModalState(false)
    end
    updateTabVisuals()
    
    if loops[currentWorld] then
        task.spawn(function()
            while loops[currentWorld] do
                runTacticalSequence(currentWorld)
                task.wait(0.5)
            end
        end)
    end
end)

sellBtn.MouseButton1Click:Connect(function()
    autoSellEnabled = not autoSellEnabled
    sellBtn.Text = "AUTO MERCHANT SELL: " .. (autoSellEnabled and "ON" or "OFF")
    sellBtn.BackgroundColor3 = autoSellEnabled and Color3.fromRGB(0, 80, 50) or Color3.fromRGB(20, 25, 35)
    
    if autoSellEnabled then
        task.spawn(function()
            while autoSellEnabled do
                InventoryRemote:InvokeServer("SellAll")
                task.wait(2)
            end
        end)
    end
end)

-- Live Monitor Interface Rendering Refresh Link
local function updateScrollMonitor()
    for _, child in ipairs(ScrollingFrame:GetChildren()) do
        if child:IsA("TextLabel") then child:Destroy() end
    end
    for _, artifact in ipairs(LocalArtifacts:GetChildren()) do
        local r = artifact:GetAttribute("Rarity") or (artifact:FindFirstChild("Rarity") and artifact.Rarity.Value) or "Unknown"
        local lbl = Instance.new("TextLabel")
        lbl.Size = UDim2.new(1, -10, 0, 18)
        lbl.BackgroundTransparency = 1
        lbl.Text = string.format(" > [%s] %s", r, string.sub(artifact.Name, 1, 8))
        lbl.TextColor3 = (r == "Eternal" or r == "Godly") and Color3.fromRGB(0, 220, 255) or Color3.fromRGB(140, 150, 160)
        lbl.Font = Enum.Font.Code
        lbl.TextSize = 10
        lbl.TextXAlignment = Enum.TextXAlignment.Left
        lbl.Parent = ScrollingFrame
    end
end

LocalArtifacts.ChildAdded:Connect(updateScrollMonitor)
LocalArtifacts.ChildRemoved:Connect(updateScrollMonitor)
updateScrollMonitor()
