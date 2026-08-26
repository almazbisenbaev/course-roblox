# Update 4 — Save the Coins (DataStore)

Right now the coins disappear the moment a player leaves. Let's save them, so the
number is still there next time they join.

Everything in this update happens in **one script**: the one in `ServerScriptService`
from the first lesson.

⸻

## 1. Turn on saving in Studio (do this first!)

DataStores are off by default in Studio. Nothing below will work until you switch
them on:

**Home** tab → **Game Settings** → **Security** → turn on
**Enable Studio Access to API Services** → **Save**.

> If you forget this step you'll see `403: Studio access to APIs is not allowed`
> in the Output window.

Also note: the game must be **published** (File → Publish to Roblox) for a
DataStore to exist at all.

⸻

## 2. The full script

Script inside `ServerScriptService` — every new line is marked with `-- 🆕 NEW`:

```lua
-- 🆕 NEW
local DataStoreService = game:GetService("DataStoreService")
-- 🆕 NEW
local coinsStore = DataStoreService:GetDataStore("PlayerCoins")

game.Players.PlayerAdded:Connect(function(player)
	local coins = Instance.new("IntValue")
	coins.Name = "Coins"
	coins.Value = 0
	coins.Parent = player

	-- 🆕 NEW — try to load the saved amount
	local ok, savedValue = pcall(function()
		return coinsStore:GetAsync("Player_" .. player.UserId)
	end)

	-- 🆕 NEW — if it loaded, use it
	if ok and savedValue then
		coins.Value = savedValue
	end

	print(player.Name .. " joined. Coins: " .. coins.Value)
end)

-- 🆕 NEW — save when the player leaves
game.Players.PlayerRemoving:Connect(function(player)
	pcall(function()
		coinsStore:SetAsync("Player_" .. player.UserId, player.Coins.Value)
	end)

	print(player.Name .. " left. Saved coins: " .. player.Coins.Value)
end)
```

Press **Play**, collect some coins, press **Stop**, then **Play** again — the
number should still be there.

⸻

## 3. How it works

| Line | What it does |
| --- | --- |
| `game:GetService("DataStoreService")` | asks Roblox for the saving system |
| `GetDataStore("PlayerCoins")` | opens one "box" to store things in — the name is yours to pick |
| `"Player_" .. player.UserId` | the **key**: every player needs their own slot. `UserId` is a number Roblox gives each account, and it never changes (unlike the name) |
| `GetAsync(key)` | reads what's saved. Returns `nil` if it's a brand new player |
| `SetAsync(key, value)` | writes the value |
| `PlayerRemoving` | fires just before a player leaves — the last chance to save |

### What is `pcall`?

`pcall` means **protected call**. Saving talks to Roblox's servers over the
internet, and the internet sometimes fails. Without `pcall`, one failed save
throws an error and stops the whole script. With it, the game just carries on.

```lua
local ok, savedValue = pcall(function()
	return coinsStore:GetAsync("Player_" .. player.UserId)
end)
```

- `ok` is `true` if it worked, `false` if it failed.
- `savedValue` is the number that came back.

That's why the check is `if ok and savedValue then` — load the number only if the
request **worked** *and* there actually **was** something saved.

⸻

## 4. One more safety net

When you press **Stop** in Studio, the server can shut down before
`PlayerRemoving` finishes saving. Add this at the very bottom of the script:

```lua
-- 🆕 NEW — save everyone if the server shuts down
game:BindToClose(function()
	for _, player in ipairs(game.Players:GetPlayers()) do
		pcall(function()
			coinsStore:SetAsync("Player_" .. player.UserId, player.Coins.Value)
		end)
	end
end)
```

`BindToClose` runs right before the server closes, and Roblox waits a few seconds
for it to finish. The loop goes through everyone still in the game and saves them.

⸻

## Common mistakes

- **`403: Studio access to APIs is not allowed`** → step 1 wasn't done.
- **Coins are always 0 on rejoin** → the save probably errored. Change
  `pcall(function() ... end)` in `PlayerRemoving` to grab the error and print it:
  ```lua
  local ok, err = pcall(function()
  	coinsStore:SetAsync("Player_" .. player.UserId, player.Coins.Value)
  end)
  if not ok then
  	print("Save failed: " .. err)
  end
  ```
- **Testing with two Studio windows** → both save to the same key and the last one
  to leave wins. Test one player at a time for now.

⸻

## Does the GUI still work?

Yes. The label from the last lesson uses `coins.Changed`, and loading the saved
value **changes** `coins.Value` — so the number on screen updates by itself. No
changes needed there.

⸻

## Future update plans

- Replace magic numbers with variables (e.g. `local reward = 10`).
- Add a visual/sound effect on earning or spending coins.
- Make a box a one-time purchase (attribute or disabled state).
- Move the shared Touched-handling logic into a ModuleScript once there are more boxes.
- Save more than one thing per player (coins + items) by storing a table.
