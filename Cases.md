## Cases

`RandomSelect(...)`

```lua
local nf = require(game.ReplicatedStorage.NumberFormatter)

function weightedRandomSelect(dictionary, player, dataService)
    local totalWeight = 0
    local items = dictionary.Items
    for _, item in ipairs(items) do
        totalWeight += item[2]
    end

    local weightTable = {}

    for _, item in ipairs(items) do
        local weight = item[2]
        local numToAdd = math.floor(weight / totalWeight * 100)
        local remainder = weight - numToAdd / 100 * totalWeight
        if Random.new():NextNumber() < remainder / totalWeight then
            numToAdd = numToAdd + 1
        end
        for i = 1, numToAdd do
            table.insert(weightTable, item)
        end
	end

	local result = weightTable[Random.new():NextInteger(1, #weightTable)]
	if (player) then
		dataService:RegisterCaseOpening(dictionary["Case Name"])
	end

	if (dictionary.Currency == "cash" and player) then
		-- chat announcement system
		-- "💸 Roblox just won..."
	end

    return result
end

return weightedRandomSelect
```
