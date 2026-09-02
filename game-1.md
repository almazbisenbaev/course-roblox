Green box → jump on it → +10 Coins
Blue box → jump on it → costs 20 Coins




1. Give every player a Coins value

Script inside ServerScriptService:

```lua
game.Players.PlayerAdded:Connect(function(player)
	-- когда игрок заходит в игру, делаем ему счетчик коинов
	local coins = Instance.new("IntValue")
	coins.Name = "Coins"
	-- в начале коинов ноль
	coins.Value = 0
	-- кладем счетчик внутрь игрока
	coins.Parent = player

	print(player.Name .. " joined. Coins: " .. coins.Value)
end)
```

⸻

2. The +10 box

Script inside the first box:

```lua
local part = script.Parent
part.Touched:Connect(function(object)
	-- узнаем какой игрок наступил на коробку
	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	-- если на коробку наступил игрок, а не что-то другое
	if player then
		-- даем игроку 10 коинов
		player.Coins.Value = player.Coins.Value + 10
		print(player.Name .. " got 10 coins! Coins: " .. player.Coins.Value)
	end
end)
```

⸻

3. The 20-coin box

Script inside the second box:

```lua
local part = script.Parent
part.Touched:Connect(function(object)
	-- узнаем какой игрок наступил на коробку
	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	if player then
		-- проверяем что у игрока хватает коинов
		if player.Coins.Value >= 20 then
			-- забираем 20 коинов
			player.Coins.Value = player.Coins.Value - 20
			print(player.Name .. " bought the box! Coins: " .. player.Coins.Value)
		else
			-- коинов не хватает, ничего не покупаем
			print(player.Name .. " doesn't have enough coins (needs 20). Coins: " .. player.Coins.Value)
		end
	end
end)
```

⸻

Future update plans

- Replace magic numbers with variables (e.g. `local reward = 10`).
- Add a visual/sound effect on earning or spending coins.
- Make a box a one-time purchase (attribute or disabled state).
- Move the shared Touched-handling logic into a ModuleScript once there are more boxes.
