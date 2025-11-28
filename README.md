--// SERVICES
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local guiParent = player:WaitForChild("PlayerGui")

--// MAIN GUI
local ScreenGui = Instance.new("ScreenGui", guiParent)
ScreenGui.IgnoreGuiInset = true
ScreenGui.ResetOnSpawn = false

--// MAIN FRAME (DRAGGABLE HUB)
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.fromOffset(550, 350)
MainFrame.Position = UDim2.new(0.3, 0, 0.25, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 0

Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)

local Stroke = Instance.new("UIStroke", MainFrame)
Stroke.Thickness = 1
Stroke.Color = Color3.fromRGB(60, 60, 60)

--// DRAG FUNCTION
local dragging = false
local dragStart, startPos

MainFrame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = MainFrame.Position
	end
end)

MainFrame.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = false
	end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
	if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
		local delta = input.Position - dragStart
		MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

--// SIDEBAR
local SideBar = Instance.new("Frame", MainFrame)
SideBar.Size = UDim2.fromOffset(140, 350)
SideBar.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
SideBar.BorderSizePixel = 0

Instance.new("UICorner", SideBar).CornerRadius = UDim.new(0, 12)

local SideStroke = Instance.new("UIStroke", SideBar)
SideStroke.Color = Color3.fromRGB(50, 50, 50)
SideStroke.Thickness = 1

--// TAB BUTTON CREATOR
local function createTabButton(name, order)
	local Button = Instance.new("TextButton", SideBar)
	Button.Size = UDim2.new(1, -20, 0, 40)
	Button.Position = UDim2.new(0, 10, 0, 20 + (order - 1)*45)
	Button.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
	Button.Text = name
	Button.Font = Enum.Font.GothamSemibold
	Button.TextSize = 16
	Button.TextColor3 = Color3.fromRGB(200, 200, 200)
	Button.BorderSizePixel = 0
	
	local corner = Instance.new("UICorner", Button)
	corner.CornerRadius = UDim.new(0, 6)

	return Button
end

local tabButtons = {
	Main = createTabButton("Main", 1),
	Visual = createTabButton("Visual", 2),
	Player = createTabButton("Player", 3),
}

--// MAIN CONTENT AREA
local Content = Instance.new("Frame", MainFrame)
Content.Size = UDim2.new(1, -150, 1, -20)
Content.Position = UDim2.new(0, 150, 0, 10)
Content.BackgroundTransparency = 1

--// TOGGLE FUNCTION CREATOR
local function createToggle(text, order)
	local Frame = Instance.new("Frame", Content)
	Frame.Size = UDim2.new(1, -20, 0, 45)
	Frame.Position = UDim2.new(0, 10, 0, 10 + (order - 1)*55)
	Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
	Frame.BorderSizePixel = 0

	Instance.new("UICorner", Frame).CornerRadius = UDim.new(0, 8)

	local Label = Instance.new("TextLabel", Frame)
	Label.Size = UDim2.new(0.7, 0, 1, 0)
	Label.Position = UDim2.new(0.05, 0, 0, 0)
	Label.Text = text
	Label.Font = Enum.Font.Gotham
	Label.TextColor3 = Color3.fromRGB(220, 220, 220)
	Label.TextSize = 15
	Label.BackgroundTransparency = 1

	local ToggleBtn = Instance.new("TextButton", Frame)
	ToggleBtn.Size = UDim2.fromOffset(45, 22)
	ToggleBtn.Position = UDim2.new(0.85, 0, 0.5, -11)
	ToggleBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
	ToggleBtn.Text = ""
	ToggleBtn.BorderSizePixel = 0

	local corner = Instance.new("UICorner", ToggleBtn)
	corner.CornerRadius = UDim.new(0, 20)

	local state = false

	ToggleBtn.MouseButton1Click:Connect(function()
		state = not state
		local goal = {BackgroundColor3 = state and Color3.fromRGB(0, 170, 255) or Color3.fromRGB(60, 60, 60)}
		TweenService:Create(ToggleBtn, TweenInfo.new(0.18), goal):Play()
	end)

	return Frame
end

--// EXAMPLE TOGGLES (You can change names later)
createToggle("Show Safe Zone", 1)
createToggle("Kill Aura", 2)
createToggle("ESP", 3)

