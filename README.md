-- LocalScript em StarterGui
local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")
local humanoid = char:WaitForChild("Humanoid")

-- Base inicial (SET atualiza esse valor)
local basePosition = hrp.Position
local flying = false
local flySpeed = 300
local flyHeight = 7

-- Atualizar quando respawnar
player.CharacterAdded:Connect(function(newChar)
	char = newChar
	hrp = char:WaitForChild("HumanoidRootPart")
	humanoid = char:WaitForChild("Humanoid")
	basePosition = hrp.Position
	flying = false
end)

-- Criar GUI principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SpiderDogHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = player:WaitForChild("PlayerGui")

----------------- MENU 1 (🚀 TELEGUIADO) -----------------
local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 220, 0, 110)
Frame.Position = UDim2.new(0.5, -110, 0.3, 0)
Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 12)
UICorner.Parent = Frame

local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0.8, 0, 0, 26)
ToggleButton.Position = UDim2.new(0.5, 0, 0, 25)
ToggleButton.AnchorPoint = Vector2.new(0.5, 0)
ToggleButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
ToggleButton.Text = "🚀 teleguiado(OFF/ON)"
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.Font = Enum.Font.Gotham
ToggleButton.TextSize = 12
ToggleButton.Parent = Frame

local UICorner2 = Instance.new("UICorner")
UICorner2.CornerRadius = UDim.new(0, 8)
UICorner2.Parent = ToggleButton

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(0.8, 0, 0, 26)
Title.Position = UDim2.new(0.5, 0, 0, 55)
Title.AnchorPoint = Vector2.new(0.5, 0)
Title.BackgroundTransparency = 1
Title.Text = "🕸️ Spider Cat Hub"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16
Title.Parent = Frame

-- Funções do Teleguiado
local function stopFlying()
	flying = false
	if hrp then
		for _, v in pairs(hrp:GetChildren()) do
			if v:IsA("BodyMover") or v:IsA("BodyGyro") then
				v:Destroy()
			end
		end
		hrp.CFrame = CFrame.new(hrp.Position.X, basePosition.Y, hrp.Position.Z)
	end
end

local function startFlying()
	if basePosition and not flying then
		flying = true
		local bv = Instance.new("BodyVelocity")
		bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
		bv.Parent = hrp

		local bg = Instance.new("BodyGyro")
		bg.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
		bg.P = 1e5
		bg.Parent = hrp

		task.spawn(function()
			while flying and hrp and hrp.Parent do
				local targetPos = Vector3.new(basePosition.X, basePosition.Y + flyHeight, basePosition.Z)
				local diff = targetPos - hrp.Position

				if hrp.Position.Y <= basePosition.Y then
					hrp.CFrame = CFrame.new(hrp.Position.X, basePosition.Y, hrp.Position.Z)
				end

				if diff.Magnitude <= 0.3 then
					bv.Velocity = Vector3.zero
				else
					bv.Velocity = diff.Unit * flySpeed
				end

				bg.CFrame = CFrame.new(hrp.Position, targetPos)
				game:GetService("RunService").Heartbeat:Wait()
			end
			stopFlying()
		end)
	end
end

ToggleButton.MouseButton1Click:Connect(function()
	if flying then
		stopFlying()
	else
		startFlying()
	end
end)

----------------- MENU 2 (🕸️ BIEL) -----------------
local FrameBiel = Instance.new("Frame")
FrameBiel.Size = UDim2.new(0, 160, 0, 180)
FrameBiel.Position = UDim2.new(0.5, -80, 0.65, 0)
FrameBiel.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
FrameBiel.BorderSizePixel = 0
FrameBiel.Active = true
FrameBiel.Parent = ScreenGui

local UICornerBiel = Instance.new("UICorner")
UICornerBiel.CornerRadius = UDim.new(0, 12)
UICornerBiel.Parent = FrameBiel

-- Título 🕸️ BIEL
local BielTitleFrame = Instance.new("TextButton")
BielTitleFrame.Size = UDim2.new(1, 0, 0, 30)
BielTitleFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
BielTitleFrame.Text = "🕸️ BIEL"
BielTitleFrame.TextColor3 = Color3.fromRGB(255, 255, 255)
BielTitleFrame.Font = Enum.Font.GothamBold
BielTitleFrame.TextSize = 14
BielTitleFrame.AutoButtonColor = false
BielTitleFrame.Parent = FrameBiel

local UICornerTitle = Instance.new("UICorner")
UICornerTitle.CornerRadius = UDim.new(0, 6)
UICornerTitle.Parent = BielTitleFrame

-- Conteúdo
local ContentFrame = Instance.new("Frame")
ContentFrame.Size = UDim2.new(1, 0, 1, -30)
ContentFrame.Position = UDim2.new(0, 0, 0, 30)
ContentFrame.BackgroundTransparency = 1
ContentFrame.Parent = FrameBiel

local function createBielButton(name, text, yPos, color)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(0.85, 0, 0, 26)
	btn.Position = UDim2.new(0.5, 0, 0, yPos)
	btn.AnchorPoint = Vector2.new(0.5, 0)
	btn.BackgroundColor3 = color
	btn.Text = text
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.Font = Enum.Font.Gotham
	btn.TextSize = 12
	btn.Name = name
	btn.Parent = ContentFrame

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 8)
	corner.Parent = btn

	return btn
end

local painelBtn = createBielButton("Painel", "PAINEL 🕷️", 10, Color3.fromRGB(200, 0, 0))
local setBtn = createBielButton("Set", "SET 👹", 45, Color3.fromRGB(0, 100, 255))
local velBtn = createBielButton("Velocidade", "VELOCIDADE ⚡", 80, Color3.fromRGB(0, 200, 0))
local vel2Btn = createBielButton("Velocidade2", "VELOCIDADE 2 ⚡", 115, Color3.fromRGB(0, 200, 0))
local tpBtn = createBielButton("TP", "TELEPORTE 🌀", 150, Color3.fromRGB(150, 0, 200))

-- Abrir/fechar painel
local aberto = true
BielTitleFrame.MouseButton1Click:Connect(function()
	aberto = not aberto
	ContentFrame.Visible = aberto
	if aberto then
		FrameBiel.Size = UDim2.new(0, 160, 0, 180)
	else
		FrameBiel.Size = UDim2.new(0, 160, 0, 30)
	end
end)

-- Arrastar menu 🕸️ BIEL
local UserInputService = game:GetService("UserInputService")
local dragging, dragInput, dragStart, startPos

local function update(input)
	local delta = input.Position - dragStart
	FrameBiel.Position = UDim2.new(
		startPos.X.Scale,
		startPos.X.Offset + delta.X,
		startPos.Y.Scale,
		startPos.Y.Offset + delta.Y
	)
end

BielTitleFrame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = FrameBiel.Position
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

BielTitleFrame.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
		dragInput = input
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		update(input)
	end
end)

----------------- FUNÇÕES DOS BOTÕES -----------------
setBtn.MouseButton1Click:Connect(function()
	basePosition = hrp.Position
	print("📍 Base salva em: " .. tostring(basePosition))
end)

velBtn.MouseButton1Click:Connect(function()
	flySpeed = 150
end)

vel2Btn.MouseButton1Click:Connect(function()
	flySpeed = humanoid.WalkSpeed * 10
end)

-- TELEPORTE: chão ↕ +5 metros (loop por 2 segundos, bem rápido)
tpBtn.MouseButton1Click:Connect(function()
	if basePosition then
		local ground = Vector3.new(basePosition.X, basePosition.Y, basePosition.Z)
		local sky = ground + Vector3.new(0, 5, 0)

		-- Garantir controle do HRP
		local bv = Instance.new("BodyVelocity")
		bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
		bv.Velocity = Vector3.zero
		bv.Parent = hrp

		task.spawn(function()
			local startTime = tick()
			local toggle = true

			while tick() - startTime < 2 do -- dura 2 segundos
				if toggle then
					hrp.CFrame = CFrame.new(sky)
				else
					hrp.CFrame = CFrame.new(ground)
				end
				toggle = not toggle
				task.wait(0.05) -- ⚡ super rápido
			end

			-- garantir que para no chão
			hrp.CFrame = CFrame.new(ground)
			bv:Destroy()
		end)
	end
end)
