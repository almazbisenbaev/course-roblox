Apply these 4 changes to the +10 box script, the 20-coin box script, and the 50-coin box script (all three).

1. After `local part = script.Parent`, add:

```lua
local debounce = false
```

2. As the first line inside `part.Touched:Connect(function(object)`, add:

```lua
if debounce then return end
```

3. As the first line inside `if player then`, add:

```lua
debounce = true
```

4. As the last lines inside `if player then`, after the `print(...)` line, add:

```lua
task.wait(1)
debounce = false
```
