## 1. The +10 box

Script inside the first box — this is the script from the first lesson, every new
line is marked with `-- 🆕 НОВОЕ`. You only need to add those lines:

```lua
local part = script.Parent

-- 🆕 НОВОЕ — здесь запоминаем, получал ли игрок коины только что
local debounce = false

part.Touched:Connect(function(object)

	-- 🆕 НОВОЕ — если игрок только что получал коины, ничего не делаем
	if debounce then return end

	-- узнаем какой игрок наступил на коробку
	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	-- если на коробку наступил игрок, а не что-то другое
	if player then
		-- 🆕 НОВОЕ — запоминаем что игрок только что получил коины, чтобы не дать их еще раз
		debounce = true

		-- даем игроку 10 коинов
		player.Coins.Value = player.Coins.Value + 10
		print(player.Name .. " got 10 coins! Coins: " .. player.Coins.Value)

		-- 🆕 НОВОЕ — ждем 1 секунду прежде чем дать еще коины
		task.wait(1)
		-- 🆕 НОВОЕ — убираем дебаунс, чтобы игрок мог получить еще коины
		debounce = false
	end
end)
```

⸻

## 2. The 20-coin box

Script inside the second box — the same 5 new lines, marked with `-- 🆕 НОВОЕ`:

```lua
local part = script.Parent

-- 🆕 НОВОЕ — здесь запоминаем, купил ли игрок эту вещь только что
local debounce = false

part.Touched:Connect(function(object)

	-- 🆕 НОВОЕ — если игрок только что покупал, ничего не делаем
	if debounce then return end

	-- узнаем какой игрок наступил на коробку
	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	if player then
		-- 🆕 НОВОЕ — запоминаем что игрок только что купил эту вещь, чтобы не купил сразу еще раз
		debounce = true

		-- проверяем что у игрока хватает коинов
		if player.Coins.Value >= 20 then
			-- забираем 20 коинов
			player.Coins.Value = player.Coins.Value - 20
			print(player.Name .. " bought the box! Coins: " .. player.Coins.Value)
		else
			-- коинов не хватает, ничего не покупаем
			print(player.Name .. " doesn't have enough coins (needs 20). Coins: " .. player.Coins.Value)
		end

		-- 🆕 НОВОЕ — ждем 1 секунду прежде чем можно купить еще раз
		task.wait(1)
		-- 🆕 НОВОЕ — убираем дебаунс, теперь игрок может покупать еще раз
		debounce = false
	end
end)
```

⸻

## 3. The 50-coin box

Same as the 20-coin box, with the amount changed to 50. Again, only the
`-- 🆕 НОВОЕ` lines are new:

```lua
local part = script.Parent

-- 🆕 НОВОЕ — здесь запоминаем, купил ли игрок эту вещь только что
local debounce = false

part.Touched:Connect(function(object)

	-- 🆕 НОВОЕ — если игрок только что покупал, ничего не делаем
	if debounce then return end

	-- узнаем какой игрок наступил на коробку
	local player = game.Players:GetPlayerFromCharacter(object.Parent)
	if player then
		-- 🆕 НОВОЕ — запоминаем что игрок только что купил эту вещь, чтобы не купил сразу еще раз
		debounce = true

		-- проверяем что у игрока хватает коинов
		if player.Coins.Value >= 50 then
			-- забираем 50 коинов
			player.Coins.Value = player.Coins.Value - 50
			print(player.Name .. " bought the box! Coins: " .. player.Coins.Value)
		else
			-- коинов не хватает, ничего не покупаем
			print(player.Name .. " doesn't have enough coins (needs 50). Coins: " .. player.Coins.Value)
		end

		-- 🆕 НОВОЕ — ждем 1 секунду прежде чем можно купить еще раз
		task.wait(1)
		-- 🆕 НОВОЕ — убираем дебаунс, теперь игрок может покупать еще раз
		debounce = false
	end
end)
```
