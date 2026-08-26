# Update 2 — Debounce (stop the box from firing many times per touch)

Problem: `Touched` fires many times while you stand on a box, so one jump can give you
coins over and over (or charge you over and over).

Fix: a **debounce** — a simple `true/false` flag that locks the script for 1 second
after it runs.

Below are the **complete scripts** for all three boxes, ready to copy.
Every new line is marked with a `-- NEW` comment above it.

⸻

## 1. The +10 box

Script inside the first box:

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

		player.Coins.Value = player.Coins.Value + 10
		print(player.Name .. " got 10 coins! Coins: " .. player.Coins.Value)

		-- NEW
		task.wait(1)
		-- NEW
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

⸻

## What the 4 new pieces do

| New line | Where it goes | What it does |
| --- | --- | --- |
| `local debounce = false` | after `local part = script.Parent` | creates the lock, unlocked at start |
| `if debounce then return end` | first line inside `Touched` | if it's locked, stop right here |
| `debounce = true` | first line inside `if player then` | lock it |
| `task.wait(1)` + `debounce = false` | last lines inside `if player then` | wait 1 second, then unlock |

Note: the `debounce` variable is **per box**, not per player — while one player is on
the cooldown, the box ignores everyone. That's fine for now; a per-player version is a
later update.
