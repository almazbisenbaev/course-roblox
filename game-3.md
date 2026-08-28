# Update 3 — Show the Coins on screen (GUI)

Right now the coins only show up in the **Output** window with `print(...)`.
The player can't see them. Let's put the number on the screen.

Plan: build a small text label in `StarterGui`, then a `LocalScript` keeps its
text in sync with `player.Coins`.

⸻

## 1. Build the GUI (no code — just clicking in Studio)

In the **Explorer**:

1. Hover over `StarterGui` → click **+** → add a **ScreenGui**.
2. Hover over the new `ScreenGui` → click **+** → add a **TextLabel**.
3. Rename the TextLabel to **`CoinsLabel`** (the script looks for that exact name).

Now select `CoinsLabel` and set these in the **Properties** panel:

| Property | Value | Why |
| --- | --- | --- |
| `Size` | `{0, 200}, {0, 50}` | 200 x 50 pixels |
| `Position` | `{0, 20}, {0, 20}` | 20 pixels from the top-left corner |
| `Text` | `Coins: 0` | what you see before the script runs |
| `TextScaled` | ✅ on | text fills the label nicely |
| `BackgroundTransparency` | `0.5` | half see-through background |

Press **Play** — you should already see the label sitting in the corner.
It's just a picture though, the number never changes yet.

⸻

## 2. The script that updates the label

Add a **LocalScript** inside the `ScreenGui` (hover `ScreenGui` → **+** → **LocalScript**).

> ⚠️ It must be a **LocalScript**, not a Script. GUI belongs to one player,
> so it runs on that player's computer.

```lua
local player = game.Players.LocalPlayer
local label = script.Parent.CoinsLabel

-- Ждем пока сервер создаст коины
local coins = player:WaitForChild("Coins")

-- Показываем текущее значение (пока выйдет 0)
label.Text = "Coins: " .. coins.Value

-- Обновляем текст каждый раз когда у игрока меняются коины
coins.Changed:Connect(function(newValue)
	label.Text = "Coins: " .. newValue
end)
```

That's the whole thing. Press **Play** and jump on the green box — the number
on screen goes up.

⸻

## 3. How it works

| Line | What it does |
| --- | --- |
| `game.Players.LocalPlayer` | "the player sitting at this computer" — only works in a LocalScript |
| `script.Parent.CoinsLabel` | the script is inside the ScreenGui, so its parent is the ScreenGui, and the label is inside it |
| `player:WaitForChild("Coins")` | the ServerScriptService script creates `Coins` a moment after joining — this waits for it instead of erroring |
| `label.Text = "Coins: " .. coins.Value` | sets the text once, at the start |
| `coins.Changed:Connect(...)` | runs the function **every time** the number changes; `newValue` is the new amount |

The `..` joins text together: `"Coins: " .. 30` becomes `"Coins: 30"`.

⸻

## Common mistakes

- **Nothing shows up** → the script is a `Script` instead of a `LocalScript`.
- **`CoinsLabel is not a valid member of ScreenGui`** → the TextLabel is named
  something else (like `TextLabel`), or it's not inside the ScreenGui.
- **The label never updates** → you wrote `coins.Value.Changed` instead of
  `coins.Changed`.

⸻

## Future update plans

- Make the label flash green when coins go up, red when they go down.
- Add a coin icon (`ImageLabel`) next to the number.
- Show a short "Not enough coins!" message when a purchase fails.
- Use `UIListLayout` once there is more than one thing to display.
