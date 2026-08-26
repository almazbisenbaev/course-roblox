Green box → jump on it → +10 Coins
Blue box → jump on it → costs 20 Coins
Red box → jump on it → costs 50 Coins




1. Give every player a Coins value

Script inside ServerScriptService:

```lua
game.Players.PlayerAdded:Connect(function(player)
	local coins = Instance.new("IntValue")
	coins.Name = "Coins"
	coins.Value = 0
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
	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	if player then
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
	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	if player then
		if player.Coins.Value >= 20 then
			player.Coins.Value = player.Coins.Value - 20
			print(player.Name .. " bought the box! Coins: " .. player.Coins.Value)
		else
			print(player.Name .. " doesn't have enough coins (needs 20). Coins: " .. player.Coins.Value)
		end
	end
end)
```

⸻

4. The 50-coin box

Same as the 20-coin box, with the amount changed to 50:

```lua
local part = script.Parent
part.Touched:Connect(function(object)
	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	if player then
		if player.Coins.Value >= 50 then
			player.Coins.Value = player.Coins.Value - 50
			print(player.Name .. " bought the box! Coins: " .. player.Coins.Value)
		else
			print(player.Name .. " doesn't have enough coins (needs 50). Coins: " .. player.Coins.Value)
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
