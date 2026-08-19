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

local part = script.Parent
part.Touched:Connect(function(object)
	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	if player then
		player.Coins.Value = player.Coins.Value + 10
		print(player.Name .. " got 10 coins!")
	end
end)

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

local part = script.Parent
part.Touched:Connect(function(object)
	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	if player then
		if player.Coins.Value >= 20 then
			player.Coins.Value = player.Coins.Value - 20
			print(player.Name .. " bought the box!")
		end
	end
end)

The important part is:

if player.Coins.Value >= 20 then

Check whether they have enough.

Then:

player.Coins.Value = player.Coins.Value - 20

Take the coins.

⸻

4. The 50-coin box

Same thing, just change 20 to 50:

local part = script.Parent
part.Touched:Connect(function(object)
	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	if player then
		if player.Coins.Value >= 50 then
			player.Coins.Value = player.Coins.Value - 50
			print(player.Name .. " bought the box!")
		end
	end
end)

And that’s already a complete little economy system.

One thing you’ll notice when testing: Touched fires a lot. A player’s legs, arms, etc. can touch the box multiple times, so the +10 box could accidentally give 20, 30, 40 coins from one jump.

That’s actually the next useful thing to learn: a debounce/cooldown. It would make this game behave properly without adding much code.