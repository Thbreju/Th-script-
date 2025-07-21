local bannedPlayers = {}

remote.OnServerEvent:Connect(function(player, command, targetName)
	if not admins[player.Name] then return end

	if command == "Ban" and targetName then
		bannedPlayers[targetName] = true
		local target = game.Players:FindFirstChild(targetName)
		if target then
			target:Kick("Você foi banido pelo Th Hub.")
		end
		return
	end

	local target = game.Players:FindFirstChild(targetName)
	if not target or not target.Character then return end
	local humanoid = target.Character:FindFirstChildOfClass("Humanoid")

	if command == "Kill" and humanoid then
		humanoid.Health = 0

	elseif command == "Speed" and humanoid then
		humanoid.WalkSpeed = 100

	elseif command == "ResetSpeed" and humanoid then
		humanoid.WalkSpeed = 16

	elseif command == "Respawn" then
		target:LoadCharacter()

	elseif command == "Fly" then
		local bodyVelocity = Instance.new("BodyVelocity")
		bodyVelocity.Velocity = Vector3.new(0, 50, 0)
		bodyVelocity.MaxForce = Vector3.new(0, 50000, 0)
		bodyVelocity.Name = "ThHubFly"
		bodyVelocity.Parent = target.Character.PrimaryPart

	elseif command == "Unfly" then
		local fly = target.Character.PrimaryPart:FindFirstChild("ThHubFly")
		if fly then fly:Destroy() end

	elseif command == "Invisible" then
		for _, part in pairs(target.Character:GetChildren()) do
			if part:IsA("BasePart") then
				part.Transparency = 1
			end
		end

	elseif command == "Visible" then
		for _, part in pairs(target.Character:GetChildren()) do
			if part:IsA("BasePart") then
				part.Transparency = 0
			end
		end

	elseif command == "TeleportTo" then
		if target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
			local tpPart = target.Character.HumanoidRootPart
			player.Character:MoveTo(tpPart.Position + Vector3.new(2, 0, 2))
		end
	end

	print(player.Name .. " executou o comando: " .. command .. " em " .. targetName)
end)
