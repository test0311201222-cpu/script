--// SERVICES
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")
local Player = game.Players.LocalPlayer
local GuiParent = Player:WaitForChild("PlayerGui")

--// MAIN UI
local ScreenGui = Instance.new("ScreenGui", GuiParent)
ScreenGui.ResetOnSpawn = false

--// MAIN HUB FRAME
local Hub = Instance.new("Frame", ScreenGui)
Hub.Size = UDim2.fromOffset(420, 280)
Hub.Position = UDim2.new(0.35, 0, 0.25, 0)
Hub.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Hub.BackgroundTransparency = 0.15
Hub.BorderSizePixel = 0

Instance.new("UICorner", Hub).CornerRadius = UDim.new(0, 12)

local HubStroke = Instance.new("UIStroke", Hub)
HubStroke.Thickness = 2
HubStroke.Color = Color3.fromRGB(50, 120, 255)

--// DRAG SYSTEM
local dragging = false
local dragStart, startPos

Hub.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = Hub.Position
	end
end)

Hub.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = false
	end
end)

UIS.InputChanged:Connect(function(input)
	if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
		local delta = input.Position - dragStart
		Hub.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

--// HEADER
local Header = Instance.new("TextLabel", Hub)
Header.Size = UDim2.new(1, 0, 0, 35)
Header.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Header.Text = "My Hub"
Header.Font = Enum.Font.GothamSemibold
Header.TextSize = 18
Header.TextColor3 = Color3.new(1, 1, 1)

Instance.new("UICorner", Header).CornerRadius = UDim.new(0, 12)

local HeaderStroke = Instance.new("UIStroke", Header)
HeaderStroke.Color = Color3.fromRGB(60, 60, 60)

--// BUTTONS ON TOP-RIGHT
local function createTopButton(txt, xOffset)
	local Btn = Instance.new("TextButton", Header)
	Btn.Size = UDim2.fromOffset(35, 35)
	Btn.Position = UDim2.new(1, xOffset, 0, 0)
	Btn.BackgroundTransparency = 1
	Btn.Text = txt
	Btn.TextColor3 = Color3.fromRGB(200, 200, 200)
	Btn.TextSize = 18
	Btn.Font = Enum.Font.GothamBold
	return Btn
end

local MinBtn = createTopButton("-", -105)
local MaxBtn = createTopButton("+", -70)
local CloseBtn = createTopButton("×", -35)

--// CONTENT
local Content = Instance.new("Frame", Hub)
Content.Size = UDim2.new(1, -20, 1, -50)
Content.Position = UDim2.new(0, 10, 0, 45)
Content.BackgroundTransparency = 1

--// ESP TOGGLE
local ToggleFrame = Instance.new("Frame", Content)
ToggleFrame.Size = UDim2.new(1, 0, 0, 50)
ToggleFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
ToggleFrame.BorderSizePixel = 0

Instance.new("UICorner", ToggleFrame).CornerRadius = UDim.new(0, 10)

local ToggleName = Instance.new("TextLabel", ToggleFrame)
ToggleName.Size = UDim2.new(0.7, 0, 1, 0)
ToggleName.Position = UDim2.new(0.05, 0, 0, 0)
ToggleName.BackgroundTransparency = 1
ToggleName.Text = "ESP"
ToggleName.Font = Enum.Font.Gotham
ToggleName.TextColor3 = Color3.new(1, 1, 1)
ToggleName.TextSize = 17

local ToggleBtn = Instance.new("TextButton", ToggleFrame)
ToggleBtn.Size = UDim2.fromOffset(45, 22)
ToggleBtn.Position = UDim2.new(0.8, 0, 0.5, -11)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
ToggleBtn.Text = ""
ToggleBtn.BorderSizePixel = 0
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(0, 20)

local espEnabled = false

ToggleBtn.MouseButton1Click:Connect(function()
	espEnabled = not espEnabled
	local goal = {BackgroundColor3 = espEnabled and Color3.fromRGB(0, 170, 255) or Color3.fromRGB(70,70,70)}
	TweenService:Create(ToggleBtn, TweenInfo.new(0.2), goal):Play()
end)

--// MINIMIZED UI
local MiniFrame = Instance.new("TextButton", ScreenGui)
MiniFrame.Size = UDim2.fromOffset(120, 40)
MiniFrame.Position = UDim2.new(0.02, 0, 0.4, 0)
MiniFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MiniFrame.Text = "My Hub"
MiniFrame.TextColor3 = Color3.new(1,1,1)
MiniFrame.BackgroundTransparency = 0.3
MiniFrame.Font = Enum.Font.GothamBold
MiniFrame.TextSize = 16
MiniFrame.Visible = false

Instance.new("UICorner", MiniFrame).CornerRadius = UDim.new(0, 10)

--// MINIMIZE BUTTON ACTION
MinBtn.MouseButton1Click:Connect(function()
	Hub.Visible = false
	MiniFrame.Visible = true
end)

MiniFrame.MouseButton1Click:Connect(function()
	MiniFrame.Visible = false
	Hub.Visible = true
end)

--// MAXIMIZE BUTTON (ANIMATION)
local maximized = false
MaxBtn.MouseButton1Click:Connect(function()
	maximized = not maximized
	local goal = {}

	if maximized then
		goal = {Size = UDim2.fromOffset(550, 350)}
	else
		goal = {Size = UDim2.fromOffset(420, 280)}
	end

	TweenService:Create(Hub, TweenInfo.new(0.25, Enum.EasingStyle.Sine), goal):Play()
end)

--// CLOSE WITH CONFIRMATION
local ConfirmFrame = Instance.new("Frame", ScreenGui)
ConfirmFrame.Size = UDim2.fromOffset(300, 140)
ConfirmFrame.Position = UDim2.new(0.4, 0, 0.35, 0)
ConfirmFrame.BackgroundColor3 = Color3.fromRGB(20,20,20)
ConfirmFrame.Visible = false

Instance.new("UICorner", ConfirmFrame).CornerRadius = UDim.new(0, 12)

local ConfirmText = Instance.new("TextLabel", ConfirmFrame)
ConfirmText.Size = UDim2.new(1,0,0,50)
ConfirmText.Position = UDim2.new(0,0,0,10)
ConfirmText.BackgroundTransparency = 1
ConfirmText.Text = "Deseja fechar o hub?"
ConfirmText.Font = Enum.Font.Gotham
ConfirmText.TextSize = 18
ConfirmText.TextColor3 = Color3.new(1,1,1)

local Yes = Instance.new("TextButton", ConfirmFrame)
Yes.Size = UDim2.fromOffset(120, 40)
Yes.Position = UDim2.new(0.1,0,0.6,0)
Yes.BackgroundColor3 = Color3.fromRGB(50,50,50)
Yes.Text = "Sim"
Yes.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", Yes).CornerRadius = UDim.new(0,10)

local No = Instance.new("TextButton", ConfirmFrame)
No.Size = UDim2.fromOffset(120, 40)
No.Position = UDim2.new(0.55,0,0.6,0)
No.BackgroundColor3 = Color3.fromRGB(50,50,50)
No.Text = "Não"
No.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", No).CornerRadius = UDim.new(0,10)

CloseBtn.MouseButton1Click:Connect(function()
	ConfirmFrame.Visible = true
end)

Yes.MouseButton1Click:Connect(function()
	ScreenGui:Destroy()
end)

No.MouseButton1Click:Connect(function()
	ConfirmFrame.Visible = false
end)
