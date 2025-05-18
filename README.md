-- NatHub | Dead Rails (0.3.1) UI por ChatGPT
local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local TweenService = game:GetService("TweenService")

-- Criar GUI principal
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "NatHubUI"
ScreenGui.ResetOnSpawn = false

-- Janela principal
local mainFrame = Instance.new("Frame", ScreenGui)
mainFrame.Size = UDim2.new(0, 320, 0, 240)
mainFrame.Position = UDim2.new(0.5, -160, 0.5, -120)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = true
mainFrame.Active = true
mainFrame.Draggable = true

-- Cantos arredondados
local UICorner = Instance.new("UICorner", mainFrame)
UICorner.CornerRadius = UDim.new(0, 10)

-- Título
local title = Instance.new("TextLabel", mainFrame)
title.Size = UDim2.new(1, -60, 0, 30)
title.Position = UDim2.new(0, 10, 0, 0)
title.Text = "NatHub | Dead Rails (0.3.1)"
title.TextColor3 = Color3.new(1, 1, 1)
title.BackgroundTransparency = 1
title.TextXAlignment = Enum.TextXAlignment.Left
title.Font = Enum.Font.GothamBold
title.TextSize = 14

-- Botão Fechar
local closeButton = Instance.new("TextButton", mainFrame)
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -30, 0, 0)
closeButton.Text = "X"
closeButton.TextColor3 = Color3.new(1, 1, 1)
closeButton.BackgroundTransparency = 1
closeButton.Font = Enum.Font.GothamBold
closeButton.TextSize = 14
closeButton.MouseButton1Click:Connect(function()
	mainFrame.Visible = false
end)

-- Botão Minimizar
local minimizeButton = Instance.new("TextButton", mainFrame)
minimizeButton.Size = UDim2.new(0, 30, 0, 30)
minimizeButton.Position = UDim2.new(1, -60, 0, 0)
minimizeButton.Text = "-"
minimizeButton.TextColor3 = Color3.new(1, 1, 1)
minimizeButton.BackgroundTransparency = 1
minimizeButton.Font = Enum.Font.GothamBold
minimizeButton.TextSize = 14
minimizeButton.MouseButton1Click:Connect(function()
	for _, v in pairs(mainFrame:GetChildren()) do
		if v:IsA("TextButton") or v:IsA("TextLabel") or v:IsA("ScrollingFrame") then
			if v ~= title and v ~= closeButton and v ~= minimizeButton then
				v.Visible = not v.Visible
			end
		end
	end
end)

-- ScrollFrame para botões
local menuFrame = Instance.new("ScrollingFrame", mainFrame)
menuFrame.Size = UDim2.new(1, -20, 1, -40)
menuFrame.Position = UDim2.new(0, 10, 0, 40)
menuFrame.CanvasSize = UDim2.new(0, 0, 0, 300)
menuFrame.BackgroundTransparency = 1
menuFrame.BorderSizePixel = 0
menuFrame.ScrollBarThickness = 4

-- Função para criar botões
local function createButton(name, callback)
	local btn = Instance.new("TextButton", menuFrame)
	btn.Size = UDim2.new(1, -10, 0, 35)
	btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	btn.Text = name
	btn.TextColor3 = Color3.new(1, 1, 1)
	btn.Font = Enum.Font.Gotham
	btn.TextSize = 14
	btn.AutoButtonColor = true

	local corner = Instance.new("UICorner", btn)
	corner.CornerRadius = UDim.new(0, 8)

	btn.MouseButton1Click:Connect(callback)
end

-- Funções do Hub
createButton("Auto Win", function()
	local args = {[1] = true}
	game:GetService("ReplicatedStorage"):WaitForChild("RemoteEvents"):WaitForChild("Win"):FireServer(unpack(args))
end)

createButton("Coletar Ouro", function()
	for _, v in pairs(workspace:GetDescendants()) do
		if v:IsA("ProximityPrompt") and v.Parent.Name:lower():find("gold") then
			fireproximityprompt(v)
		end
	end
end)

createButton("Teleport: Estação", function()
	player.Character:MoveTo(Vector3.new(0, 10, 0)) -- substitua pelas coords reais
end)

createButton("Teleport: Trem", function()
	player.Character:MoveTo(Vector3.new(100, 10, 100))
end)

createButton("Teleport: Base", function()
	player.Character:MoveTo(Vector3.new(-200, 10, 50))
end)

-- Final
print("✅ NatHub carregado com sucesso.")
