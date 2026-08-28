## 1. The +10 box

Script inside the first box:

```lua
local part = script.Parent
local debounce = false

part.Touched:Connect(function(object)

	-- если игрок уже получал коины
	if debounce then return end

	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	if player then
		-- указываем что юзер уже получил коины, чтобы не давать их еще раз
		debounce = true

		player.Coins.Value = player.Coins.Value + 10
		print(player.Name .. " got 10 coins! Coins: " .. player.Coins.Value)

		-- ждем 1 секунду прежде чем дать еще коины
		task.wait(1)
		-- убираем дебаунс, чтобы игрок мог получить еще коины
		debounce = false
	end
end)
```

⸻

## 2. The 20-coin box

Script inside the second box:

```lua
local part = script.Parent

-- NEW
local debounce = false

part.Touched:Connect(function(object)
	-- NEW
	if debounce then return end

	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	if player then
		-- NEW
		debounce = true

		if player.Coins.Value >= 20 then
			player.Coins.Value = player.Coins.Value - 20
			print(player.Name .. " bought the box! Coins: " .. player.Coins.Value)
		else
			print(player.Name .. " doesn't have enough coins (needs 20). Coins: " .. player.Coins.Value)
		end

		-- NEW
		task.wait(1)
		-- NEW
		debounce = false
	end
end)
```

⸻

## 3. The 50-coin box

Same as the 20-coin box, with the amount changed to 50:

```lua
local part = script.Parent

-- NEW
local debounce = false

part.Touched:Connect(function(object)
	-- NEW
	if debounce then return end

	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	if player then
		-- NEW
		debounce = true

		if player.Coins.Value >= 50 then
			player.Coins.Value = player.Coins.Value - 50
			print(player.Name .. " bought the box! Coins: " .. player.Coins.Value)
		else
			print(player.Name .. " doesn't have enough coins (needs 50). Coins: " .. player.Coins.Value)
		end

		-- NEW
		task.wait(1)
		-- NEW
		debounce = false
	end
end)
```
