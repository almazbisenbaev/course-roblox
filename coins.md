
# Example if

```lua
local coins = 100

if coins >= 50 then
    print("You have enough coins to buy the item!")
end

if coins >= 50 then
    print("You have enough coins to buy the item!")
end
```


# Example game

```lua
local box = script.Parent

box.Touched:Connect(function()

	print("The box was touched!")

	box.BrickColor = BrickColor.new("Lime green")

end)
```

