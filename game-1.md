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

Now when a player joins, Roblox creates:

Player
└── Coins
    └── Value: 0

So you can do:

`player.Coins.Value`

to get their coins.

And:

`player.Coins.Value = player.Coins.Value + 10`

to give them 10.

⸻

2. The +10 box

Put a Script inside the first box:

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

There’s one important difference from your original code.

You had:

local parent = object.Parent
local humanoid = parent:FindFirstChild("Humanoid")

That’s good for figuring out “did a character touch me?”

But now we want to know which Player owns that character, so we use:

game.Players:GetPlayerFromCharacter(object.Parent)

That gives us the actual Player.

⸻

3. The 20-coin box

Put this in the second box:

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

The important part is:

`if player.Coins.Value >= 20 then`

Check whether they have enough.

Then:

`player.Coins.Value = player.Coins.Value - 20`

Take the coins.

⸻

4. The 50-coin box

Same thing, just change 20 to 50:

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

And that’s already a complete little economy system.

One thing you’ll notice when testing: Touched fires a lot. A player’s legs, arms, etc. can touch the box multiple times, so the +10 box could accidentally give 20, 30, 40 coins from one jump.

That’s actually the next useful thing to learn: a debounce/cooldown. It would make this game behave properly without adding much code.

⸻

Future update plans

1. Debounce the boxes

   Touched can fire multiple times per jump (once per limb). Add a simple flag per box so it only triggers once, then waits before allowing another trigger:

   ```lua
   local part = script.Parent
   local debounce = false

   part.Touched:Connect(function(object)
       if debounce then return end
       local player = game.Players:GetPlayerFromCharacter(object.Parent)
       if player then
           debounce = true
           player.Coins.Value = player.Coins.Value + 10
           print(player.Name .. " got 10 coins! Coins: " .. player.Coins.Value)
           task.wait(1)
           debounce = false
       end
   end)
   ```

2. Show the Coins value in a GUI

   Right now Coins only shows up in the output/print statements. Add a ScreenGui with a TextLabel that reads `player.Coins:GetPropertyChangedSignal("Value")` (or `.Changed`) so players can see their balance live, without opening the Explorer.

3. Other gradual enhancements

   - Save Coins between sessions with DataStoreService, so players don’t lose progress when they leave.
   - Turn the box costs/rewards into variables (e.g. `local reward = 10`) instead of magic numbers, so they’re easier to tune.
   - Add a small visual/sound effect (e.g. the box briefly changes color or plays a sound) when a player earns or spends coins, as feedback beyond the print statements.
   - Prevent a box from being “bought” more than once if it’s meant to be a one-time purchase, using an attribute or a destroyed/disabled state.
   - Wrap the repeated Touched-handling logic (get player, check/adjust coins, print) into a shared ModuleScript function once you have more than a couple of boxes, to avoid copy-pasting the same script into every box.