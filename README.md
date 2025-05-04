local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer

repeat wait() until player:FindFirstChild("PlayerGui")

-- GUI
local gui = Instance.new("ScreenGui", player.PlayerGui)
gui.Name = "AutoClickGui"
gui.ResetOnSpawn = false

local button = Instance.new("TextButton", gui)
button.Size = UDim2.new(0, 160, 0, 40)
button.Position = UDim2.new(0, 10, 0, 10)
button.BackgroundColor3 = Color3.fromRGB(50, 150, 255)
button.Text = "Ativar AutoClick"
button.Font = Enum.Font.SourceSansBold
button.TextSize = 20
button.TextColor3 = Color3.new(1, 1, 1)

-- AUTO CLICK
local ativo = false
local loop

local function clicarMeioDaTela()
	local viewportSize = workspace.CurrentCamera.ViewportSize
	local x = viewportSize.X / 2
	local y = viewportSize.Y / 2

	VirtualInputManager:SendMouseButtonEvent(x, y, 0, true, game, 0)
	VirtualInputManager:SendMouseButtonEvent(x, y, 0, false, game, 0)
end

-- Toggle do botão
button.MouseButton1Click:Connect(function()
	ativo = not ativo
	button.Text = ativo and "Desativar AutoClick" or "Ativar AutoClick"
	button.BackgroundColor3 = ativo and Color3.fromRGB(200, 50, 50) or Color3.fromRGB(50, 150, 255)

	if ativo then
		loop = RunService.RenderStepped:Connect(function()
			clicarMeioDaTela()
		end)
	else
		if loop then
			loop:Disconnect()
			loop = nil
		end
	end
end)
