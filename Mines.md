## Mines

`MinesService.lua`

```lua
local gameData = {Mines = {}, Checked = {}}
local minesSet = 0
for i = 1, 25 do
    gameData.Mines[NumberToLetter(i)] = false
    gameData.Checked[NumberToLetter(i)] = false end
repeat
    local randomLetter = NumberToLetter(Random.new():NextInteger(1, 25))
    if (not gameData.Mines[randomLetter]) then
        gameData.Mines[randomLetter] = true
        minesSet += 1
    end task.wait()
until minesSet >= totalMines
```

`NumberToLetter(...)`

```lua
if number < 1 or number > 25 then
    return nil
end

local letter = string.char(number + 64)
return letter
```

`CheckCell(...)`

```lua
local checkedCells = gameData.Checked
if (gameData.Mines[cell] == nil) then return end
if (checkedCells[cell] == true) then return end

local isBomb = gameData.Mines[cell]
if (not isBomb) then
    gameData.Checked[cell] = true
    local total_uncovered = 0
    local total_mines = 0
    for _, checked in pairs(gameData.Checked) do if (checked) then total_uncovered += 1 end end
    for _, is_mine in pairs(gameData.Mines) do if (is_mine) then total_mines += 1 end end
    local amount = math.floor(CalculateMinesPayout(total_mines, total_uncovered, 25, gameData.BetAmount))

    if (amount >= gameSettings.Mines.MaximumWin) then
        return "cashout", table.pack(self:Cashout(player))[2]
    end

    return "safe", amount
else
    ActiveGames[player] = nil
    return "bomb", gameData.Mines
end
```