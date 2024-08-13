## Coinflip

`CoinflipService.lua`

```lua
local url = `https://{domain}/v1/ah/random/coinflip?a={session.Player2[1]}&b={session.Player2[5]}`
local success, response = pcall(function()
    return HttpService:GetAsync(url, true)
end)
if (not success) then
    Service.Client.RoundChanged:FireAll("end", Sessions[round_id], 4)
    Sessions[round_id] = nil
    StartedSessions[round_id] = nil
    return
end
response = HttpService:JSONDecode(response)

local winningColour = session[`Player{response.result}`][2]
local winningPlayer = (session.Player1[2] == winningColour) and session.Player1[1] or session.Player2[1]
local winningString = winningPlayer == session.Player1[1] and "Player1" or "Player2"
local looserString = winningPlayer == session.Player1[1] and "Player2" or "Player1"
session._HASH = response.hash
```

[`call-coinflip-bot.js`](https://github.com/Noxirity/ArcadeHavenAPI/blob/main/endpoints/v1/ah/games/call-coinflip-bot.js)
[`coinflip.js`](https://github.com/Noxirity/ArcadeHavenAPI/blob/main/endpoints/v1/ah/random/coinflip.js)

`settings.bot_win_chance` is only defined during Coinflip events.