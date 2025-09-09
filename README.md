-- Shadow com botão circular para abrir/fechar
-- LocalScript

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Criando ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ShadowScriptsBETA"
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Frame principal (com efeito rainbow)
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 250, 0, 150)
frame.Position = UDim2.new(0.35, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Visible = false -- começa fechado
frame.Parent = screenGui

-- UICorner para arredondar
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 15)
corner.Parent = frame

-- Efeito Rainbow no Frame
task.spawn(function()
	while true do
		for hue = 0, 255 do
			if frame.Visible then
				frame.BackgroundColor3 = Color3.fromHSV(hue/255, 1, 1)
			end
			task.wait(0.05)
		end
	end
end)

-- Título
local title = Instance.new("TextLabel")
title.Text = "Shadow Float"
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundTransparency = 1
title.TextScaled = true
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.Parent = frame

-- Botão Tween
local tweenBtn = Instance.new("TextButton")
tweenBtn.Text = "Float"
tweenBtn.Size = UDim2.new(0.8, 0, 0, 50)
tweenBtn.Position = UDim2.new(0.1, 0, 0.5, 0)
tweenBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
tweenBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
tweenBtn.TextScaled = true
tweenBtn.Font = Enum.Font.GothamBold
tweenBtn.Parent = frame

local btnCorner = Instance.new("UICorner")
btnCorner.CornerRadius = UDim.new(0, 10)
btnCorner.Parent = tweenBtn

-- Fly Tween Script
local flying = false

tweenBtn.MouseButton1Click:Connect(function()
	flying = not flying
	if flying then
		-- Ativa voo (um pouco mais rápido que antes)
		local bodyVelocity = Instance.new("BodyVelocity")
		bodyVelocity.Velocity = Vector3.new(0, 8, 0)
		bodyVelocity.MaxForce = Vector3.new(0, math.huge, 0)
		bodyVelocity.Parent = humanoidRootPart
	else
		-- Desativa voo
		for _, v in pairs(humanoidRootPart:GetChildren()) do
			if v:IsA("BodyVelocity") then
				v:Destroy()
			end
		end
	end
end)

-- Botão circular para abrir/fechar
local circleBtn = Instance.new("TextButton")
circleBtn.Size = UDim2.new(0, 50, 0, 50)
circleBtn.Position = UDim2.new(0.05, 0, 0.5, 0)
circleBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
circleBtn.Text = ""
circleBtn.Active = true
circleBtn.Draggable = true
circleBtn.Parent = screenGui

local circleCorner = Instance.new("UICorner")
circleCorner.CornerRadius = UDim.new(1, 0) -- deixa redondo
circleCorner.Parent = circleBtn

-- Rainbow no círculo
task.spawn(function()
	while true do
		for hue = 0, 255 do
			circleBtn.BackgroundColor3 = Color3.fromHSV(hue/255, 1, 1)
			task.wait(0.05)
		end
	end
end)

-- Clicar no círculo abre/fecha o Gui
circleBtn.MouseButton1Click:Connect(function()
	frame.Visible = not frame.Visible
end)
