# gens
Gens for a gens server (problem)

options:
    gens_amount_cap: 25
    drops_time: 5 seconds
command /test:
    trigger:
        set {gens::placed::cap::%player's uuid%} to 25

on join:
    if {placed::gens::amount::%player's uuid%} isn't set:
        set {placed::gens::amount::%player's uuid%} to {@gens_amount_cap}

on tab complete of "/gengive":
    if player has permission "gen.give":
        set tab completions for position 2 to"wheat","pumpkin","melon","coal" 
command /gengive <player> <text>:
    trigger:
        if player has permission "gen.give":
            if arg-2 is "wheat":
			          give arg-1 hay block, named "&eWheat Gen"
            if arg-2 is "pumpkin":
                give arg-1, pumpkin, block, named "&6Pumpkin Gen")
            if arg-2 is "coal":
                give arg-1, coal block, named "&8Coal Gen")
            if arg-2 is "melon":
                give arg-1, melon block, named "&2Melon Gen")
if event-block is Melon block
send action bar "&a%{placed::gens::amount::%player's uuid%}%&f/&a%{gens::placed::cap::%player's uuid%}%" to player
set event-block to air
every {@drops_time}:
    loop all players:
        loop {placed::gens::%loop-player's uuid%::}:
            gensDrops(loop-value-2, wheat) if block at loop-value-2 contains hay bale
            gensDrops(loop-value-2, pumpkin pie) if block at loop-value-2 contains pumpkin block
            gensDrops(loop-value-2, coal) if block at loop-value-2 contains coal block
            gensDrops(loop-value-2, melon slice) if block at loop-value-2 contains melon block
on join:
    while player is online:
        updateBoard(player)
        wait 1 seconds
on disconnect:
    clear scoreboard of player
function updateBoard(P: player):

    set title of {_P}'s scoreboard to "&aCrystalGN"
    set line 15 of {_P}'s scoreboard to "Gens Limit: %{placed::gens::amount::%{_P}'s uuid%}%/%{gens::placed::cap::%{_P}'s uuid%}%"

on right click: 
    if {placed::gens::%player's uuid%::} contains location of event-block:
        if block at event-block = hay bale:
            gensUpgrade(player, location of event-block, pumpkin, 500)
        else if block at event-block = pumpkin:
            gensUpgrade(player, location of event-block, melon, 1000)
        else if block at event-block = melon:
            gensUpgrade(player, location of event-block, coal, 2500)
        else if block at event-block = coal:

function gensUpgrade(P: player, L: location, B: itemtype, N: int):
    if {_P}'s balance >= {_N}:
        set block at {_L} to {_B}
        remove {_N} from {_P}'s balance
    else:
        send "&cYou dont have enough money!" to {_P}
        stop
