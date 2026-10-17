local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")


local targetTextureId = 130742397

local function applyTexture(instance)
	-- Меняем только у Part и MeshPart
	if instance:IsA("Part") or instance:IsA("MeshPart") then
		local textureId = "rbxassetid://" .. tostring(targetTextureId)
		
	
		if not instance:IsA("BasePart") then return end

	
		instance.TopTexture = textureId
		instance.BottomTexture = textureId
		instance.FrontTexture = textureId
		instance.BackTexture = textureId
		instance.LeftTexture = textureId
		instance.RightTexture = textureId

		
		if instance:IsA("MeshPart") and instance:FindFirstChild("Mesh") then
			local mesh = instance.Mesh
			mesh.TextureId = textureId
		end
	end
end

local function traverseAndApply(root)
	for _, child in ipairs(root:GetDescendants()) do
		applyTexture(child)
	end
	
	applyTexture(root)
end

local workspaceParts = workspace:GetDescendants()
for _, part in ipairs(workspaceParts) do
	applyTexture(part)
end

traverseAndApply(character)

player.CharacterAdded:Connect(function(newChar)
	character = newChar
	humanoidRootPart = character:WaitForChild("HumanoidRootPart")
	traverseAndApply(character)
end)
