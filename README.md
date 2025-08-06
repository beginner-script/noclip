local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local RunService = game:GetService("RunService")

-- GUI 생성
local screenGui = Instance.new("ScreenGui", game.CoreGui)
screenGui.ResetOnSpawn = false
screenGui.Name = "NoclipGui"

local button = Instance.new("TextButton", screenGui)
button.Size = UDim2.new(0, 80, 0, 30)
button.Position = UDim2.new(0, 10, 0, 10)
button.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
button.TextColor3 = Color3.fromRGB(0, 0, 0)
button.Text = "Noclip"
button.Font = Enum.Font.SourceSansBold
button.TextSize = 16

-- 노클립 상태
local noclipEnabled = false
local connection

-- 노클립 On/Off 함수
local function toggleNoclip()
	noclipEnabled = not noclipEnabled
	if noclipEnabled then
		connection = RunService.Stepped:Connect(function()
			for _, part in ipairs(player.Character:GetDescendants()) do
				if part:IsA("BasePart") then
					part.CanCollide = false
				end
			end
		end)
		button.Text = "Noclip: ON"
	else
		if connection then
			connection:Disconnect()
		end
		for _, part in ipairs(player.Character:GetDescendants()) do
			if part:IsA("BasePart") then
				part.CanCollide = true
			end
		end
		button.Text = "Noclip: OFF"
	end
end

-- 버튼 클릭 시 토글
button.MouseButton1Click:Connect(toggleNoclip)

-- 캐릭터 리스폰 시 갱신
player.CharacterAdded:Connect(function(newChar)
	char = newChar
	if noclipEnabled and connection then
		connection:Disconnect()
	end
end)
