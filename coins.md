
# Example if

```lua
-- здесь запоминаем сколько у нас коинов
local coins = 100

-- проверяем что коинов хватает, чтобы купить вещь
if coins >= 50 then
    -- этот print сработает только если коинов 50 или больше
    print("You have enough coins to buy the item!")
end

-- а тут проверяем 500 коинов, и у нас их не хватает
if coins >= 500 then
    -- этот print не сработает, потому что коинов только 100
    print("You have enough coins to buy the item!")
end
```


# Example game

```lua
-- script.Parent — это коробка, внутри которой лежит наш скрипт
local box = script.Parent

-- Touched срабатывает каждый раз когда кто-то трогает коробку
box.Touched:Connect(function()

	-- этот текст появится в окне Output
	print("The box was touched!")

	-- красим коробку в зеленый цвет
	box.BrickColor = BrickColor.new("Lime green")

end)
```

