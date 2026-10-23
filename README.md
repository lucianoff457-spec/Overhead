local function atualizarPlaca(player)
	local character = player.Character
	local head = character and character:FindFirstChild("Head")
	local billboard = head and head:FindFirstChild(NOME_GUI)

	-- Só LWdvks0 pode ver os dois ícones.
	if player.Name ~= USUARIO_ESPECIAL then
		if billboard then
			billboard:Destroy()
		end
		return
	end

	if not billboard then
		return
	end

	local badges = billboard:FindFirstChild("Emblemas")
	if not badges then
		return
	end

	local icones = { "🛡️", "👑" }

	for index, emoji in ipairs(icones) do
		local badge = badges:FindFirstChild("Emblema" .. index)
		local label = badge and badge:FindFirstChild("Emoji")

		if badge then
			badge.Visible = true
		end

		if label then
			label.Text = emoji
		end
	end
end

local function criarPlaca(player, character)
	local head = character:WaitForChild("Head", 10)
	if not head then
		return
	end

	local antiga = head:FindFirstChild(NOME_GUI)
	if antiga then
		antiga:Destroy()
	end

	-- Não cria placa para os outros jogadores.
	if player.Name ~= USUARIO_ESPECIAL then
		return
	end

	local billboard = Instance.new("BillboardGui")
	billboard.Name = NOME_GUI
	billboard.Adornee = head
	billboard.AlwaysOnTop = true
	billboard.Size = UDim2.fromOffset(76, 38)
	billboard.StudsOffset = Vector3.new(0, 3.5, 0)
	billboard.MaxDistance = 150
	billboard.Parent = head

	local badges = Instance.new("Frame")
	badges.Name = "Emblemas"
	badges.BackgroundTransparency = 1
	badges.Size = UDim2.fromScale(1, 1)
	badges.Parent = billboard

	local icones = { "🛡️", "👑" }

	for index, emoji in ipairs(icones) do
		local badge = Instance.new("Frame")
		badge.Name = "Emblema" .. index
		badge.Size = UDim2.fromOffset(34, 34)
		badge.Position = UDim2.fromOffset((index - 1) * 42, 2)
		badge.BackgroundColor3 = Color3.fromRGB(12, 14, 17)
		badge.BorderSizePixel = 0
		badge.Parent = badges

		local corner = Instance.new("UICorner")
		corner.CornerRadius = UDim.new(1, 0)
		corner.Parent = badge

		local label = Instance.new("TextLabel")
		label.Name = "Emoji"
		label.BackgroundTransparency = 1
		label.Size = UDim2.fromScale(1, 1)
		label.Font = Enum.Font.GothamBold
		label.Text = emoji
		label.TextScaled = true
		label.Parent = badge
	end

	atualizarPlaca(player)
end
