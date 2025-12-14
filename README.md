local ReplicatedStorage = game:GetService("ReplicatedStorage")
local morphEvent = ReplicatedStorage:WaitForChild("MorphEvent")
local abilityEvent = ReplicatedStorage:WaitForChild("AbilityEvent")

-- IDs de outfits (REEMPLAZA)
local morphs = {
	Stuart = 1234567890,
	Caduceus = 2345678901,
	JohnDoe = 3456789012
}

-- Sonidos (REEMPLAZA)
local sounds = {
	Stuart = "rbxassetid://9123456789",
	Caduceus = "rbxassetid://9234567890",
	JohnDoe = "rbxassetid://9345678901"
}

morphEvent.OnServerEvent:Connect(function(player, morph)
	local char = player.Character
	if not char then return end
	local humanoid = char:FindFirstChildOfClass("Humanoid")
	if not humanoid then return end

	local descId = morphs[morph]
	if not descId then return end

	local description = game.Players:GetHumanoidDescriptionFromOutfitId(descId)
	humanoid:ApplyDescription(description)
end)

abilityEvent.OnServerEvent:Connect(function(player, morph)
	local char = player.Character
	if not char then return end
	local humanoid = char:FindFirstChildOfClass("Humanoid")
	if not humanoid then return end

	-- 🔊 Sonido
	local root = char:FindFirstChild("HumanoidRootPart")
	if root and sounds[morph] then
		local sound = Instance.new("Sound")
		sound.SoundId = sounds[morph]
		sound.Volume = 1
		sound.Parent = root
		sound:Play()
		game.Debris:AddItem(sound, 5)
	end

	-- ⚡ Habilidades
	if morph == "Stuart" then
		local oldSpeed = humanoid.WalkSpeed
		humanoid.WalkSpeed = 40
		task.delay(5, f
