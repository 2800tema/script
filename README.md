local api = loadstring(game:HttpGet("https://raw.githubusercontent.com/kometa-anon/kometa/main/api/api.lua"))()
local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/kometa-anon/kometa/main/ui/finity.lua"))()
local bssapi = loadstring(game:HttpGet("https://raw.githubusercontent.com/kometa-anon/kometa/main/api/bssapi.lua"))()

if not isfolder("kometa") then makefolder("kometa") end
if isfile('kometa.txt') == false then (syn and syn.request or http_request)({ Url = "http://127.0.0.1:6463/rpc?v=1",Method = "POST",Headers = {["Content-Type"] = "application/json",["Origin"] = "https://discord.com"},Body = game:GetService("HttpService"):JSONEncode({cmd = "INVITE_BROWSER",args = {code = "2a5gVpcpzv"},nonce = game:GetService("HttpService"):GenerateGUID(false)}),writefile('kometa.txt', "discord")})end
if _G.windowname then game.Players.LocalPlayer:Kick('kocmoc detected.') end

-- Script temporary variables

local playerstatsevent = game:GetService("ReplicatedStorage").Events.RetrievePlayerStats
local statstable = playerstatsevent:InvokeServer()
local monsterspawners = game:GetService("Workspace").MonsterSpawners
local rarename
local ToysFolder = game:GetService('Workspace').Toys
function rtsg() tab = game.ReplicatedStorage.Events.RetrievePlayerStats:InvokeServer() return tab end
function maskequip(mask) local ohString1 = "Equip" local ohTable2 = { ["Mute"] = false, ["Type"] = mask, ["Category"] = "Accessory"} game:GetService("ReplicatedStorage").Events.ItemPackageEvent:InvokeServer(ohString1, ohTable2) end
local lasttouched = nil
local done = true
local hi = false

-- Script tables

local temptable = {
    version = "1.6.3",
    MondoCollectTokens = false,
    blackfield = "Ant Field",
    LastFieldColor = 'White',
    redfields = {},
    bluefields = {},
    whitefields = {},
    shouldiconvertballoonnow = false,
    balloondetected = false,
    puffshroomdetected = false,
    magnitude = 70,
    size = nil,
    WebSocket = nil,
    running = false,
    configname = "",
    globalconfigname = "",
    GlobalConfigDescription = '',
    tokenpath = game:GetService("Workspace").Collectibles,
    started = {
        vicious = false,
        mondo = false,
        windy = false,
        ant = false,
        monsters = false
    },
    detected = {
        vicious = false,
        windy = false
    },
    tokensfarm = false,
	bestblacklistever = {  -- actually if u have at least 10 iq you can bypass it
	437282664
	},
    converting = false,
    honeystart = 0,
    grib = nil,
    gribpos = CFrame.new(0,0,0),
    honeycurrent = statstable.Totals.Honey,
    dead = false,
    float = false,
    pepsigodmode = false,
    pepsiautodig = false,
    alpha = false,
    beta = false,
    myhiveis = false,
    invis = false,
    windy = nil,
    collecting = {
        tickets = false,
        rares = false,
        snowflake = false,
        dispensers = false
    },
    sprouts = {
        detected = false,
        coords 
    },
    cache = {
        autofarm = false,
        killmondo = false,
        vicious = false,
        windy = false
    },
    allplanters = {},
    planters = {
        planter = {},
        cframe = {},
        activeplanters = {
            type = {},
            id = {}
        }
    },
    monstertypes = {"Ladybug", "Rhino", "Spider", "Scorpion", "Mantis", "Werewolf"},
    ["stopapypa"] = function(path, part)
        local Closest
        for i,v in next, path:GetChildren() do
            if v.Name ~= "PlanterBulb" then
                if Closest == nil then
                    Closest = v.Soil
                else
                    if (part.Position - v.Soil.Position).magnitude < (Closest.Position - part.Position).magnitude then
                        Closest = v.Soil
                    end
                end
            end
        end
        return Closest
    end,
    coconuts = {},
    crosshairs = {},
    FuzzyBombs = {},
    crosshair = false,
    coconut = false,
    fuzzy = false,
    act = 0,
    ['touchedfunction'] = function(v)
        if lasttouched ~= v then
            if v.Parent.Name == "FlowerZones" then
                if v:FindFirstChild("ColorGroup") then
                    if tostring(v.ColorGroup.Value) == "Red" then
                        maskequip("Demon Mask")
                    elseif tostring(v.ColorGroup.Value) == "Blue" then
                        maskequip("Diamond Mask")
                    end
                else
                    maskequip("Gummy Mask")
                end
                lasttouched = v
            end
        end
    end,
    stats = {
        runningfor = 0,
        killedvicious = 0,
        killedwindy = 0;
        farmedsprouts = 0,
        farmedants = 0,
        sincelasthoneymaking = 0,
    },
    oldtool = rtsg()["EquippedCollector"],
    ['gacf'] = function(part, st)
        coordd = CFrame.new(part.Position.X, part.Position.Y+st, part.Position.Z)
        return coordd
    end,
    ['feed'] = function(x, y, type, amount)
        if not amount then
            amount = 1
        end
        local bo = tonumber(x)
        local ba = tonumber(y)
        local be = type
        local br = tonumber(amount)

        game:GetService("ReplicatedStorage").Events.ConstructHiveCellFromEgg:InvokeServer(bo, ba, be, br)
    end,
    foundpopstar = false,
    item_names = {}
}


local planterst = {}
local planterstindexed = {}

if temptable.honeystart == 0 then temptable.honeystart = statstable.Totals.Honey end

for i,v in next, game:GetService("Workspace").MonsterSpawners:GetDescendants() do if v.Name == "TimerAttachment" then v.Name = "Attachment" end end
for i,v in next, game:GetService("Workspace").MonsterSpawners:GetChildren() do if v.Name == "RoseBush" then v.Name = "ScorpionBush" elseif v.Name == "RoseBush2" then v.Name = "ScorpionBush2" end end
for i,v in next, game:GetService("Workspace").FlowerZones:GetChildren() do if v:FindFirstChild("ColorGroup") then if v:FindFirstChild("ColorGroup").Value == "Red" then table.insert(temptable.redfields, v.Name) elseif v:FindFirstChild("ColorGroup").Value == "Blue" then table.insert(temptable.bluefields, v.Name) end else table.insert(temptable.whitefields, v.Name) end end
local flowertable = {}
for _,z in next, game:GetService("Workspace").Flowers:GetChildren() do table.insert(flowertable, z.Position) end
local masktable = {}
for _,v in next, game:GetService("ReplicatedStorage").Accessories:GetChildren() do if string.match(v.Name, "Mask") then table.insert(masktable, v.Name) end end
local collectorstable = {}
for _,v in next, getupvalues(require(game:GetService("ReplicatedStorage").Collectors).Exists) do for e,r in next, v do table.insert(collectorstable, e) end end
local beestable = {}
for _,v in next, game:GetService("ReplicatedStorage").BeeModels:GetChildren() do table.insert(beestable, v.Name..' Bee') end
local fieldstable = {}
for _,v in next, game:GetService("Workspace").FlowerZones:GetChildren() do table.insert(fieldstable, v.Name) end
local toystable = {}
for _,v in next, game:GetService("Workspace").Toys:GetChildren() do table.insert(toystable, v.Name) end
local spawnerstable = {}
for _,v in next, game:GetService("Workspace").MonsterSpawners:GetChildren() do table.insert(spawnerstable, v.Name) end
local accesoriestable = {}
for _,v in next, game:GetService("ReplicatedStorage").Accessories:GetChildren() do if v.Name ~= "UpdateMeter" then table.insert(accesoriestable, v.Name) end end
for i,v in pairs(getupvalues(require(game:GetService("ReplicatedStorage").PlanterTypes).GetTypes)) do for e,z in pairs(v) do table.insert(temptable.allplanters, e) end end
for v,_ in next, rtsg().Eggs do table.insert(temptable.item_names, v) end
table.sort(fieldstable)
table.sort(accesoriestable)
table.sort(toystable)
table.sort(spawnerstable)
table.sort(temptable.item_names)
table.sort(masktable)
table.sort(temptable.allplanters)
table.sort(collectorstable)
table.sort(beestable)

for i,v in pairs(getupvalues(require(game:GetService("ReplicatedStorage").PlanterTypes).GetTypes)) do for e,z in pairs(v) do planterst[e] = z table.insert(planterstindexed, z) end end
table.sort(planterst)
for i,v in pairs(planterst) do planterst[i] = {} end

-- float pad

local floatpad = Instance.new("Part", game:GetService("Workspace"))
floatpad.CanCollide = false
floatpad.Anchored = true
floatpad.Transparency = 1
floatpad.Name = "FloatPad"

-- cococrab

local cocopad = Instance.new("Part", game:GetService("Workspace"))
cocopad.Name = "Coconut Part"
cocopad.Anchored = true
cocopad.Transparency = 1
cocopad.Size = Vector3.new(135, 1, 100)
cocopad.CanCollide = false
cocopad.Position = Vector3.new(-265.52117919922, 105.91863250732, 480.86791992188)

local popfolder = Instance.new("Folder", game:GetService("Workspace").Particles)
popfolder.Name = "PopStars"

-- antfarm

local antpart = Instance.new("Part", workspace)
antpart.Name = "Ant Autofarm Part"
antpart.Position = Vector3.new(96, 47, 553)
antpart.Anchored = true
antpart.Size = Vector3.new(128, 1, 50)
antpart.Transparency = 1
antpart.CanCollide = false

-- config

local kometa = {
    rares = {},
    priority = {},
    bestfields = {
        red = "Pepper Patch",
        white = "Coconut Field",
        blue = "Stump Field"
    },
    blacklistedfields = {},
    killerkometa = {},
    bltokens = {},
    toggles = {
        autofarm = false,
        farmclosestleaf = false,
        farmbubbles = false,
        autodig = false,
        collectorsteal = false,
        farmrares = false,
        farmtickets = false,
        rgbui = false,
        farmflower = false,
        farmfuzzy = false,
        farmcoco = false,
        farmflame = false,
        farmclouds = false,
        killmondo = false,
        killvicious = false,
        loopspeed = false,
        loopjump = false,
        autoquest = false,
        autoboosters = false,
        autodispense = false,
        clock = false,
        freeantpass = false,
        honeystorm = false,
        autodoquest = false,
        disableseperators = false,
        npctoggle = false,
        loopfarmspeed = false,
        mobquests = false,
        traincrab = false,
        tainsnail = false,
        avoidmobs = false,
        farmsprouts = false,
        enabletokenblacklisting = false,
        farmballoons = false,
        farmsnowflakes = false,
        collectgingerbreads = false,
        collectcrosshairs = false,
        farmpuffshrooms = false,
        tptonpc = false,
        donotfarmtokens = false,
        convertballoons = false,
        autostockings = false,
        autosamovar = false,
        autoonettart = false,
        autocandles = false,
        autofeast = false,
        autoplanters = false,
        autokillmobs = false,
        autoant = false,
        killwindy = false,
        godmode = false,
        disablerender = false,
        bloatfarm = false,
        autodonate = false,
        donotdonatedrop = false,
        instantconverters = false,
        autospawnsprout = false,
        faceballoons = false,
        faceflames = false,
        visualnight = false,
        convertminutestoggle = false,
        randomizespeed = false,
        farmglitchedtokens = false,
        freerobopass = false,
        automasks = false,
        donotconvert = false
    },
    vars = {
        field = "Ant Field",
        donatit = {"Treat", 1},
        convertat = 100,
        farmspeed = 60,
        prefer = "Tokens",
        walkspeed = 70,
        jumppower = 70,
        npcprefer = "All Quests",
        farmtype = "Walk",
        monstertimer = 3,
        convertminutes = 45
    },
    dispensesettings = {
        blub = false,
        straw = false,
        treat = false,
        coconut = false,
        glue = false,
        rj = false,
        white = false,
        red = false,
        blue = false
    },
    AutoUseSettings = {
        ['Glitter'] = false,
        ['Blue Extract'] = false,
        ['Red Extract'] = false,
        ['Glue'] = false,
        ['Oil'] = false,
        ['Enzymes'] = false,
        ['Tropical Drink'] = false,
        ['Purple Potion'] = false,
        ['Super Smoothie'] = false
    },
    beessettings = {
        general = {
            x = 1,
            y = 1,
            amount = 1
        },
        usb = "",
        usbtoggle = false,
        ugb = false,
        foodtype = "Treat",
        af = false,
        mutation = "Convert Amount",
        umb = false
    },
    webhooking = {
        convert = false,
        vicious = false,
        windy = false,
        playeradded = false,
        connectionlost = false
    },
    planterssettings = {
        {
            enabled = false,
            Type = require(game:GetService("ReplicatedStorage").PlanterTypes).INVENTORY_ORDER[1],
            growth = 1,
            field = fieldstable[1]
        },
        {
            enabled = false,
            Type = require(game:GetService("ReplicatedStorage").PlanterTypes).INVENTORY_ORDER[1],
            growth = 1,
            field = fieldstable[2]
        },
        {
            enabled = false,
            Type = require(game:GetService("ReplicatedStorage").PlanterTypes).INVENTORY_ORDER[1],
            growth = 1,
            field = fieldstable[3]
        }
    }
}

local kometawebhook = {
    webhook = '',
}

local defaultkometa = kometa
local defaultkometawebhook = kometawebhook


-- websocket

-- if syn then
--     if pcall(function() syn.websocket.connect("ws://api.kometa.ga:8888/") end) then
--         temptable.WebSocket = syn.websocket.connect("ws://api.kometa.ga:8888/")
--     end
-- elseif Krnl then
--     if Krnl.Websocket.connect("ws://api.kometa.ga:8888/") then
--         temptable.WebSocket = Krnl.Websocket.connect("ws://api.kometa.ga:8888/")
--     end
-- end

if syn then
    pcall(function()
        temptable.WebSocket = syn.websocket.connect("ws://api.kometa.ga:8888/")
    end)
elseif Krnl then
    pcall(function()
        temptable.WebSocket = Krnl.WebSocket.connect("ws://api.kometa.ga:8888/")
    end)
elseif identifyexecutor() and identifyexecutor() == 'ScriptWare' then
    pcall(function()
        temptable.WebSocket = WebSocket.connect("ws://api.kometa.ga:8888/")
    end)
end

if temptable.WebSocket then
    if syn or Krnl or (identifyexecutor() and identifyexecutor() == 'ScriptWare') then
        temptable.WebSocket.OnMessage:Connect(function(msg)
            local Data = game:GetService("HttpService"):JSONDecode(msg)
            if Data.hwid == game:GetService("RbxAnalyticsService"):GetClientId() then
                if Data.statusCode ~= '606' then
                    if Data.action == 'ConfigSave' then
                        if Data.statusCode == "601" then
                            api.notify("kometa", Data.message, 5)
                            setclipboard(Data.id)
                        elseif Data.statusCode == "602" then
                            api.notify('kometa', Data.error)
                        end
                    elseif Data.action == 'ConfigLoad' then
                        if Data.statusCode == "601" then
                            api.notify("kometa", Data.message, 5)
                            local Config = Data.cfg
                            kometa = game:service'HttpService':JSONDecode(Config)
                        else
                            api.notify('kometa', Data.error)
                        end
                    end
                else
                    api.notify('kometa', Data.error, 5)
                end
            end
            if Data.action == 'Notification' and Data.message then
                task.spawn(function() 
                    local oldColor = game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.ServerMessage.BackgroundColor3
                    game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.ServerMessage.BackgroundColor3 = Color3.fromRGB(164, 84, 255)
                    game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.ServerMessage.Visible = true 
                    game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.ServerMessage.TextBox.Text = Data.message
                    task.wait(120)
                    game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.ServerMessage.Visible = false
                    game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.ServerMessage.BackgroundColor3 = oldColor
                end)
            end
            if Data.action == 'DirectNotification' and Data.message then
                if Data.hwid == game:GetService("RbxAnalyticsService"):GetClientId() then
                    task.spawn(function() 
                        local oldColor = game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.ServerMessage.BackgroundColor3
                        game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.ServerMessage.BackgroundColor3 = Color3.fromRGB(164, 84, 255)
                        game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.ServerMessage.Visible = true 
                        game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.ServerMessage.TextBox.Text = Data.message
                        task.wait(120)
                        game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.ServerMessage.Visible = false
                        game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.ServerMessage.BackgroundColor3 = oldColor
                    end)
                end
            end
        end)
    end
end


-- functions

function findFieldWithRay(pos, dir)
    local ray = Ray.new(pos, dir)
    local part, position = workspace:FindPartOnRayWithWhitelist(ray, {game:GetService('Workspace').FlowerZones})
    if part then
        return part
    end
end

function statsget() local StatCache = require(game.ReplicatedStorage.ClientStatCache) local stats = StatCache:Get() return stats end
function farm(trying, important)
    if not IsToken(trying) then return end
    if kometa.toggles.faceballoons and findballoon() ~= nil and findballoon():FindFirstChild("BalloonRoot") then api.humanoidrootpart().CFrame = CFrame.lookAt(api.humanoidrootpart().Position, Vector3.new(findballoon().BalloonRoot.Position.X, api.humanoidrootpart().Position.Y, findballoon().BalloonRoot.Position.Z)) end
    if kometa.toggles.faceflames and findclosestflame() ~= nil then api.humanoidrootpart().CFrame = CFrame.lookAt(api.humanoidrootpart().Position, Vector3.new(findclosestflame().Position.X, api.humanoidrootpart().Position.Y, findclosestflame().Position.Z)) end
    if important and kometa.toggles.bloatfarm and temptable.foundpopstar then temptable.float = true api.teleport(CFrame.new(trying.CFrame.Position) * CFrame.Angles(0, math.rad(180), 0)) end
    -- if kometa.toggles.loopfarmspeed then game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = kometa.vars.farmspeed end
    if kometa.toggles.randomizespeed then api.humanoid().WalkSpeed = math.random(30, 70) end
    api.humanoid().AutoRotate = false
    api.humanoid():MoveTo(trying.Position) 
    repeat task.wait() until (trying.Position-api.humanoidrootpart().Position).magnitude <= 5 or not IsToken(trying) or not temptable.running
    api.humanoid().AutoRotate = true
end

function farmold(trying, important)
    if kometa.toggles.faceballoons and findballoon() then api.humanoidrootpart().CFrame = CFrame.lookAt(api.humanoidrootpart().Position, Vector3.new(findballoon().BalloonRoot.Position.X, api.humanoidrootpart().Position.Y, findballoon().BalloonRoot.Position.Z)) end
    if kometa.toggles.faceflames and findclosestflame() then api.humanoidrootpart().CFrame = CFrame.lookAt(api.humanoidrootpart().Position, Vector3.new(findclosestflame().Position.X, api.humanoidrootpart().Position.Y, findclosestflame().Position.Z)) end
    -- if kometa.toggles.loopfarmspeed then game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = kometa.vars.farmspeed end
    if kometa.toggles.randomizespeed then api.humanoid().WalkSpeed = math.random(30, 70) end
    api.humanoid().AutoRotate = false
    api.humanoid():MoveTo(trying.Position) 
    repeat task.wait() until (trying.Position-api.humanoidrootpart().Position).magnitude <= 5 or not IsToken(trying) or not temptable.running
    api.humanoid().AutoRotate = true
end

function farmtickets(v)
    if kometa.toggles.farmtickets then 
        if v.CFrame.YVector.Y == 1 and v.Transparency == 0 and v ~= nil and v.Parent ~= nil then 
            decal = v:FindFirstChildOfClass("Decal") 
            if decal.Texture ~= "1674871631" and decal.Texture ~= "rbxassetid://1674871631" then return end
            temptable.collecting.tickets = true
            temptable.float = true
            local reenablespeed = kometa.toggles.loopspeed
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(v.Position) * CFrame.new(math.random(20, 30), -3, math.random(-10, 10)) 
            repeat 
                task.wait() 
                api.humanoid().WalkSpeed = 25
                api.walkTo(v.Position)
            until not v.Parent or v.CFrame.YVector.Y ~= 1 or not v.Transparency == 0 or (v.Position-api.humanoidrootpart().Position).Magnitude > 50
            temptable.collecting.tickets = false
            if temptable.float then temptable.float = false end
            kometa.toggles.loopspeed = reenablespeed
            task.wait(math.random(1, 5)/10)
        end
    end 
end

function farmsnowflakes(v)
    if kometa.toggles.farmsnowflakes then
        temptable.collecting.snowflake = true
        local SnowflakePosition = v.Position
        api.teleport(CFrame.new(SnowflakePosition))
        temptable.float = true
        repeat
            task.wait()
        until not v.Parent or v.CFrame.YVector.Y ~= 1 
        if temptable.float then temptable.float = false end
        task.wait(1)
        temptable.collecting.snowflake = false
    end
end

function farmrares(v)
    if kometa.toggles.farmrares then 
        if v.CFrame.YVector.Y == 1 and v.Transparency == 0 and v ~= nil and v.Parent ~= nil then
            decal = v:FindFirstChildOfClass("Decal") 
            if not table.find(kometa.rares, string.split(decal.Texture, 'rbxassetid://')[2]) then return end
            temptable.collecting.rares = true
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(v.Position) * CFrame.new(math.random(20, 30), -3, math.random(-10, 10)) 
            temptable.float = true
            local reenablespeed = kometa.toggles.loopspeed
            repeat 
                task.wait() 
                api.humanoid().WalkSpeed = 25
                api.walkTo(v.Position)
            until not v.Parent or v.CFrame.YVector.Y ~= 1 or not v.Transparency == 0 or (v.Position-api.humanoidrootpart().Position).Magnitude > 50
            temptable.collecting.rares = false
            kometa.toggles.loopspeed = reenablespeed
            if temptable.float then temptable.float = false end
            task.wait(math.random(1, 5)/10)
        end 
    end
end

function farmcombattokens(v, pos, type)
    if type == 'crab' then
        if v.CFrame.YVector.Y == 1 and v.Transparency == 0 and v ~= nil and v.Parent ~= nil then
            if (v.Position - pos.Position).Magnitude < 50 then
                repeat
                    task.wait()
                    api.walkTo(v.Position)
                until not v.Parent or v.CFrame.YVector.Y ~= 1 or not v
                api.teleport(pos)
            end
        end
    elseif type == 'snail' then
        if v.CFrame.YVector.Y == 1 and v.Transparency == 0 and v ~= nil and v.Parent ~= nil then
            if (v.Position - pos.Position).Magnitude < 50 then
                repeat
                    task.wait()
                    api.walkTo(v.Position)
                until not v.Parent or v.CFrame.YVector.Y ~= 1 or not v
                api.teleport(pos)
            end
        end
    elseif type == 'mondo' then
        if temptable.MondoCollectTokens then return end
        if v.CFrame.YVector.Y == 1 and v.Transparency == 0 and v ~= nil and v.Parent ~= nil then
            if (v.Position - pos.Position).Magnitude < 25 then
                repeat
                    task.wait()
                    api.tweenNoDelay(0.5, v.CFrame)
                until not v.Parent or v.CFrame.YVector.Y ~= 1 or not v
                api.teleport(pos)
            end
        end
    end
end

function disableall()
    if kometa.toggles.autofarm and not temptable.converting then
        temptable.cache.autofarm = true
        kometa.toggles.autofarm = false
    end
    if kometa.toggles.killmondo and not temptable.started.mondo then
        kometa.toggles.killmondo = false
        temptable.cache.killmondo = true
    end
    if kometa.toggles.killvicious and not temptable.started.vicious then
        kometa.toggles.killvicious = false
        temptable.cache.vicious = true
    end
    if kometa.toggles.killwindy and not temptable.started.windy then
        kometa.toggles.killwindy = false
        temptable.cache.windy = true
    end
end

function enableall()
    if temptable.cache.autofarm then
        kometa.toggles.autofarm = true
        temptable.cache.autofarm = false
    end
    if temptable.cache.killmondo then
        kometa.toggles.killmondo = true
        temptable.cache.killmondo = false
    end
    if temptable.cache.vicious then
        kometa.toggles.killvicious = true
        temptable.cache.vicious = false
    end
    if temptable.cache.windy then
        kometa.toggles.killwindy = true
        temptable.cache.windy = false
    end
end

function gettoken(v3)
    if not v3 then
        v3 = fieldposition
    end
    task.wait()
    if kometa.toggles.bloatfarm and temptable.foundpopstar then return end
    for e,r in next, game:GetService("Workspace").Collectibles:GetChildren() do
        itb = false
        if r:FindFirstChildOfClass("Decal") and kometa.toggles.enabletokenblacklisting then
            if api.findvalue(kometa.bltokens, string.split(r:FindFirstChildOfClass("Decal").Texture, 'rbxassetid://')[2]) then
                itb = true
            end
        end
        if tonumber((r.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude) <= temptable.magnitude/1.2 and not itb and (v3-r.Position).magnitude <= temptable.magnitude then
            farm(r)
        end
    end
end

function getlinktoken(v3)
    if not v3 then
        v3 = fieldposition
    end
    task.wait()
    if kometa.toggles.bloatfarm and temptable.foundpopstar then return end
    for e,r in next, game:GetService("Workspace").Collectibles:GetChildren() do
        if r:FindFirstChildOfClass("Decal") and (r:FindFirstChildOfClass("Decal").Texture == '1629547638' or r:FindFirstChildOfClass("Decal").Texture == 'rbxassetid://1629547638') then
            if tonumber((r.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude) <= temptable.magnitude/1.2 and (v3-r.Position).magnitude <= temptable.magnitude then
                farm(r)
                break
            end
        end
    end
end

function makesprinklers()
    if (game:GetService("Workspace").FlowerZones[kometa.vars.field].Position - api.humanoidrootpart().Position).magnitude < temptable.magnitude then
        local Sprinkler = rtsg().EquippedSprinkler
        local Amount = 1
        if Sprinkler == "Basic Sprinkler" or Sprinkler == "The Supreme Saturator" then
            Amount = 1
        elseif Sprinkler == "Silver Soakers" then
            Amount = 2
        elseif Sprinkler == "Golden Gushers" then
            Amount = 3
        elseif Sprinkler == "Diamond Drenchers" then
            Amount = 4
        end
        local FieldSelected = game:GetService("Workspace").FlowerZones[kometa.vars.field]
        local FieldEdge1 = FieldSelected.Position + Vector3.new((FieldSelected.Size.X / 2) - 24, 0, (FieldSelected.Size.Z / 2) - 24)
        local FieldEdge2 = FieldSelected.Position - Vector3.new((FieldSelected.Size.X / 2) - 24, 0, (FieldSelected.Size.Z / 2) - 24)
        local FieldBounds = {
            [1] = Vector3.new(FieldEdge1.X - FieldEdge1.X % 0.000000000000001, FieldSelected.Position.Y, FieldSelected.Position.Z),
            [2] = Vector3.new(FieldSelected.Position.X, FieldSelected.Position.Y, FieldEdge1.Z - FieldEdge1.Z % 0.000000000000001),
            [3] = Vector3.new(FieldEdge2.X - FieldEdge2.X % 0.000000000000001, FieldSelected.Position.Y, FieldSelected.Position.Z),
            [4] = Vector3.new(FieldSelected.Position.X, FieldSelected.Position.Y, FieldEdge2.Z - FieldEdge2.Z % 0.000000000000001),
        }
        local JumpPower = api.humanoid().JumpPower
        for Index = 1, Amount do
            if not kometa.toggles.autofarm then
                break
            end
            api.humanoid().JumpPower = 70
            if Amount == 1 then
                api.teleport(CFrame.new(FieldSelected.Position))
                task.wait(.1)
                api.humanoid().Jump = true
            elseif Amount ~= 2 then
                api.teleport(CFrame.new(FieldBounds[Index]))
                task.wait(.1)
                api.humanoid().Jump = true
            else
                if Index == 1 then
                    api.teleport(CFrame.new(FieldBounds[1]))
                    task.wait(.1)
                    api.humanoid().Jump = true
                else
                    api.teleport(CFrame.new(FieldBounds[3]))
                    task.wait(.1)
                    api.humanoid().Jump = true
                end
            end
            task.wait(0.2)
            game.ReplicatedStorage.Events.PlayerActivesCommand:FireServer({["Name"] = "Sprinkler Builder"})
            api.humanoid().JumpPower = JumpPower
            task.wait(1.3)
        end
    else
        sprinkler = rtsg().EquippedSprinkler
        e = 1
        if sprinkler == "Basic Sprinkler" or sprinkler == "The Supreme Saturator" then
            e = 1
        elseif sprinkler == "Silver Soakers" then
            e = 2
        elseif sprinkler == "Golden Gushers" then
            e = 3
        elseif sprinkler == "Diamond Drenchers" then
            e = 4
        end
        for i = 1, e do
            k = api.humanoid().JumpPower
            if e ~= 1 then api.humanoid().JumpPower = 70 api.humanoid().Jump = true task.wait(.2) end
            game.ReplicatedStorage.Events.PlayerActivesCommand:FireServer({["Name"] = "Sprinkler Builder"})
            if e ~= 1 then api.humanoid().JumpPower = k task.wait(1) end
        end
    end
end

function killmobs()
    for i,v in pairs(game:GetService("Workspace").MonsterSpawners:GetChildren()) do
        if v:FindFirstChild("Territory") then
            if v.Name ~= "Commando Chick" and v.Name ~= "CoconutCrab" and v.Name ~= "StumpSnail" and v.Name ~= "TunnelBear" and v.Name ~= "King Beetle Cave" and not v.Name:match("CaveMonster") and not v:FindFirstChild("TimerLabel", true).Visible then
                if v.Name:match("Werewolf") then
                    monsterpart = game:GetService("Workspace").Territories.WerewolfPlateau.w
                elseif v.Name:match("Mushroom") then
                    monsterpart = game:GetService("Workspace").Territories.MushroomZone.Part
                else
                    monsterpart = v.Territory.Value
                end
                api.humanoidrootpart().CFrame = monsterpart.CFrame
                repeat api.humanoidrootpart().CFrame = monsterpart.CFrame avoidmob() task.wait(1) until v:FindFirstChild("TimerLabel", true).Visible or api.humanoid().Health == 0
                for i = 1, 4 do gettoken(monsterpart.Position) end
            end
        end
    end
end

function IsToken(token)
    if not token then
        return false
    end
    if not token.Parent then return false end
    if token then
        if kometa.toggles.farmballoons and findclosestballoon() then
            if (findclosestballoon().BalloonRoot.Position - api.humanoidrootpart().Position).magnitude >= 30 then
                return false
            end
        end
        if token.Orientation.Z ~= 0 and token.Name == "C" then
            return false
        end
        if token.Name == "C" then
            if token:FindFirstChild("FrontDecal") then
            else
                return false
            end
        end
        if not token.Name == "C" and not token.Name == "Bubble" then
            return false
        end
        if not token:IsA("Part") then
            return false
        end
        return true
    else
        return false
    end
end

function check(ok)
    if not ok then
        return false
    end
    if not ok.Parent then return false end
    return true
end

function findvalue(table, value)
    for i,v in pairs(table) do
        for e,r in pairs(v) do
            if r == value then
                return i
            end
        end
    end
end

function getplanters()
    for i,v in pairs(debug.getupvalues(require(game:GetService("ReplicatedStorage").LocalPlanters).LoadPlanter)[4]) do 
        planterst[v.Type] = {}
        if v.IsMine then
            if planterst[v.Type][1] == nil then
                planterst[v.Type] = {v.Type, v.ActorID, v.Pos, v.GrowthPercent}
            end
        end
    end
end

function findclosestballoon()
    local root = game:GetService("Players").LocalPlayer.Character.HumanoidRootPart
    if root == nil then return end
    local studs = math.huge
    local part;
    for _, obj in next, game:GetService("Workspace").Balloons.FieldBalloons:GetChildren() do
        if obj:FindFirstChild("BalloonRoot") and obj:FindFirstChild("PlayerName") then
            if obj:FindFirstChild("PlayerName").Value == game.Players.LocalPlayer.Name then
                local distance = (root.Position - obj.BalloonRoot.Position).Magnitude
                if distance < studs then
                    studs = distance
                    part = obj
                end
            end
        end
    end
    return part
end

function findballoon()
    for _, obj in next, game:GetService("Workspace").Balloons.FieldBalloons:GetChildren() do
        if obj:FindFirstChild("BalloonRoot") and obj:FindFirstChild("PlayerName") then
            if obj:FindFirstChild("PlayerName").Value == game.Players.LocalPlayer.Name then
                part = obj
                break
            end
        end
    end
    return part
end

function findclosestflame()
    local root = game:GetService("Players").LocalPlayer.Character.HumanoidRootPart
    if root == nil then return end
    local studs = math.huge
    local part;
    for _, obj in next, game:GetService("Workspace").PlayerFlames:GetChildren() do
        local distance = (root.Position - obj.Position).Magnitude
        if distance < studs then
            studs = distance
            part = obj
        end
    end
    return part
end

function farmant()
    antpart.CanCollide = true
    temptable.started.ant = true
    anttable = {left = true, right = false}
    temptable.oldtool = rtsg()['EquippedCollector']
    game.ReplicatedStorage.Events.ItemPackageEvent:InvokeServer("Equip",{["Mute"] = true,["Type"] = "Spark Staff",["Category"] = "Collector"})
    game.ReplicatedStorage.Events.ToyEvent:FireServer("Ant Challenge")
    kometa.toggles.autodig = true
    acl = CFrame.new(127, 48, 547)
    acr = CFrame.new(65, 48, 534)
    task.wait(1)
    game.ReplicatedStorage.Events.PlayerActivesCommand:FireServer({["Name"] = "Sprinkler Builder"})
    api.humanoidrootpart().CFrame = api.humanoidrootpart().CFrame + Vector3.new(0, 15, 0)
    task.wait(3)
    repeat
        task.wait()
        for i,v in next, game.Workspace.Toys["Ant Challenge"].Obstacles:GetChildren() do
            if v:FindFirstChild("Root") then
                if (v.Root.Position-api.humanoidrootpart().Position).magnitude <= 40 and anttable.left then
                    api.humanoidrootpart().CFrame = acr
                    anttable.left = false anttable.right = true
                    wait(.1)
                elseif (v.Root.Position-api.humanoidrootpart().Position).magnitude <= 40 and anttable.right then
                    api.humanoidrootpart().CFrame = acl
                    anttable.left = true anttable.right = false
                    wait(.1)
                end
            end
        end
    until game:GetService("Workspace").Toys["Ant Challenge"].Busy.Value == false
    task.wait(1)
    game.ReplicatedStorage.Events.ItemPackageEvent:InvokeServer("Equip",{["Mute"] = true,["Type"] = temptable.oldtool,["Category"] = "Collector"})
    temptable.started.ant = false
    antpart.CanCollide = false
    temptable.stats.farmedants = temptable.stats.farmedants + 1
end

function collectplanters()
    getplanters()
    -- for i,v in pairs(planterst) do
    --     for e,r in pairs(kometa.planterssettings) do
    --         if r.Type == i then
    --             if r.enabled then
    --                 if v[1] == nil then continue end
    --                 if v[1] ~= r.Type then continue end
    --                 if r.growth <= v[4] then
    --                     if v[3] and r.safepuffs then
    --                         continue
    --                     end
    --                     api.teleport(CFrame.new(v[5]))
    --                     game:GetService("ReplicatedStorage").Events.PlanterModelCollect:FireServer(v[2])
    --                     task.wait(2)
    --                     -- game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = v.." Planter"})
    --                     for i = 1, 5 do gettoken(CFrame.new(v[5]).Position) end
    --                     task.wait(2)
    --                     planterst[v[1]] = {}
    --                 end
    --             end
    --         end
    --     end
    -- end
    for i,v in pairs(planterst) do
        if v[1] == nil then continue end
        planterToCollect = nil
        if kometa.planterssettings[1].Type == v[1] then 
            planterToCollect = kometa.planterssettings[1]
        elseif kometa.planterssettings[2].Type == v[1] then 
            planterToCollect = kometa.planterssettings[2]
        elseif kometa.planterssettings[3].Type == v[1] then 
            planterToCollect = kometa.planterssettings[3]
        end
        if planterToCollect == nil then continue end
        -- print(v[1], v[4], planterToCollect.growth, planterToCollect.growth <= v[4])
        if planterToCollect.enabled and planterToCollect.growth <= v[4] then
            api.teleport(CFrame.new(v[3]))
            game:GetService("ReplicatedStorage").Events.PlanterModelCollect:FireServer(v[2])
            task.wait(1)
            -- game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = v.." Planter"})
            for i = 1, 5 do gettoken(CFrame.new(v[3]).Position) end
            task.wait(1)
            planterst[v[1]] = {}
        end
    end
end

function plantplanters()
    -- getplanters()
    -- for i,v in pairs(kometa.planterssettings) do
    --     if v.enabled then
    --         if planterst[i] then
    --             continue
    --         end
    --         api.teleport(game:GetService("Workspace").FlowerZones:FindFirstChild(v.field).CFrame)
    --         task.wait(2)
    --         game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = v.type.." Planter"})
    --         task.wait(1)
    --     end
    -- end
    for i,v in pairs(kometa.planterssettings) do
        if v.enabled then
            if planterst[v.Type][1] ~= nil then continue end
            -- if planterst[v.Type][1] == v.Type then continue end
            api.teleport(game:GetService("Workspace").FlowerZones:FindFirstChild(v.field).CFrame)
            task.wait(2)
            api.teleport(game:GetService("Workspace").FlowerZones:FindFirstChild(v.field).CFrame)
            game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = v.Type.." Planter"})
            task.wait(1)
        end
        -- if kometa.planterssettings[i].enabled then
        --     if v ~= nil then continue end
        --     api.teleport(game:GetService("Workspace").FlowerZones:FindFirstChild(v.field).CFrame)
        --     task.wait(2)
        --     game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = kometa.planterssettings[i].Type.." Planter"})
        --     task.wait(1)
        -- end
    end
end

function getprioritytokens(v3)
    if not v3 then
        v3 = fieldposition
    end
    task.wait()
    if kometa.toggles.bloatfarm and temptable.foundpopstar then return end
    for e,r in next, game:GetService("Workspace").Collectibles:GetChildren() do
        if r:FindFirstChildOfClass("Decal") and api.findvalue(kometa.priority, string.split(r:FindFirstChildOfClass("Decal").Texture, 'rbxassetid://')[2]) then
            if tonumber((r.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude) <= temptable.magnitude/1.2 and (v3-r.Position).magnitude <= temptable.magnitude then
                farm(r)
            end
        end
    end
end

function gethiveballoon()
    task.wait()
    result = false
    for i,hive in next, game:GetService("Workspace").Honeycombs:GetChildren() do
        task.wait()
        if hive:FindFirstChild("Owner") and hive:FindFirstChild("SpawnPos") then
            if tostring(hive.Owner.Value) == game.Players.LocalPlayer.Name then
                for e,balloon in next, game:GetService("Workspace").Balloons.HiveBalloons:GetChildren() do
                    task.wait()
                    if balloon:FindFirstChild("BalloonRoot") then
                        if (balloon.BalloonRoot.Position-hive.SpawnPos.Value.Position).magnitude < 15 then
                            result = true
                            break
                        end
                    end
                end
            end
        end
    end
    return result
end

function converthoney()
    task.wait(0)
    if temptable.converting then
        if game.Players.LocalPlayer.PlayerGui.ScreenGui.ActivateButton.TextBox.Text ~= "Stop Making Honey" and game.Players.LocalPlayer.PlayerGui.ScreenGui.ActivateButton.BackgroundColor3 ~= Color3.new(201, 39, 28) or (game:GetService("Players").LocalPlayer.SpawnPos.Value.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude > 10 then
            api.tween(1, game:GetService("Players").LocalPlayer.SpawnPos.Value * CFrame.fromEulerAnglesXYZ(0, 110, 0) + Vector3.new(0, 0, 9))
            task.wait(.9)
            if game.Players.LocalPlayer.PlayerGui.ScreenGui.ActivateButton.TextBox.Text ~= "Stop Making Honey" and game.Players.LocalPlayer.PlayerGui.ScreenGui.ActivateButton.BackgroundColor3 ~= Color3.new(201, 39, 28) or (game:GetService("Players").LocalPlayer.SpawnPos.Value.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude > 10 then game:GetService("ReplicatedStorage").Events.PlayerHiveCommand:FireServer("ToggleHoneyMaking") end
            task.wait(.1)
        end
    end
end

function closestleaf()
    for i,v in next, game.Workspace.Flowers:GetChildren() do
        if temptable.running == false and tonumber((v.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude) < temptable.magnitude/1.4 then
            farmold(v)
            break
        end
    end
end

function getbubble()
    for i,v in next, game.workspace.Particles:GetChildren() do
        if string.find(v.Name, "Bubble") and temptable.running == false and tonumber((v.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude) < temptable.magnitude/1.4 then
            if temptable.foundpopstar and kometa.toggles.bloatfarm then
                farm(v, true)
            else
                farm(v)
            end
            break
        end
    end
end

function getballoons()
    for i,v in next, game:GetService("Workspace").Balloons.FieldBalloons:GetChildren() do
        if v:FindFirstChild("BalloonRoot") and v:FindFirstChild("PlayerName") then
            if v:FindFirstChild("PlayerName").Value == game.Players.LocalPlayer.Name then
                if tonumber((v.BalloonRoot.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude) < temptable.magnitude/1.4 then
                    api.walkTo(v.BalloonRoot.Position)
                end
            end
        end
    end
end

function getflower()
    flowerrrr = flowertable[math.random(#flowertable)]
    if tonumber((flowerrrr-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude) <= temptable.magnitude/1.4 and tonumber((flowerrrr-fieldposition).magnitude) <= temptable.magnitude/1.4 then 
        if temptable.running == false then 
            -- if kometa.toggles.loopfarmspeed then game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = kometa.vars.farmspeed end
            if kometa.toggles.randomizespeed then api.humanoid().WalkSpeed = math.random(30, 70) end
            api.walkTo(flowerrrr) 
        end 
    end
end

function getcloud()
    for i,v in next, game:GetService("Workspace").Clouds:GetChildren() do
        e = v:FindFirstChild("Plane")
        if e and tonumber((e.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude) < temptable.magnitude/1.4 then
            api.walkTo(e.Position)
        end
    end
end

function getcoco(v)
    if temptable.coconut then repeat task.wait() until not temptable.coconut end
    temptable.coconut = true
    -- api.tween(.1, v.CFrame)
    -- repeat task.wait() api.walkTo(v.Position) until not v.Parent
    -- task.wait(.1)
    repeat
        task.wait()
            temptable.float = true
        api.tweenNoDelay(0.1, v.CFrame)
    until not v.Parent
    if temptable.float then temptable.float = false end
    temptable.coconut = false
    table.remove(temptable.coconuts, table.find(temptable.coconuts, v))
end

function getfuzzy(v)
    if temptable.fuzzy then repeat task.wait() until not temptable.fuzzy end
    if not v:FindFirstChild("Plane") then return end
    local FuzzyPlane = v:FindFirstChild("Plane")
    temptable.fuzzy = true
    repeat
        task.wait()
        api.tweenNoDelay(0.1, CFrame.new(FuzzyPlane.Position.X, FuzzyPlane.Position.Y+3, FuzzyPlane.Position.Z))
    until not v.Parent or not FuzzyPlane or not v
    temptable.fuzzy = false
    table.remove(temptable.FuzzyBombs, table.find(temptable.FuzzyBombs, v))
end

function getflame()
    for i,v in next, game:GetService("Workspace").PlayerFlames:GetChildren() do
        if tonumber((v.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude) < temptable.magnitude/1.4 and not rtsg().Modifiers.FlameHeat then
            if kometa.toggles.faceballoons and findballoon() then api.humanoidrootpart().CFrame = CFrame.lookAt(api.humanoidrootpart().Position, Vector3.new(findballoon().BalloonRoot.Position.X, api.humanoidrootpart().Position.Y, findballoon().BalloonRoot.Position.Z)) end
            if kometa.toggles.faceflames and findclosestflame() then api.humanoidrootpart().CFrame = CFrame.lookAt(api.humanoidrootpart().Position, Vector3.new(findclosestflame().Position.X, api.humanoidrootpart().Position.Y, findclosestflame().Position.Z)) end
            -- if kometa.toggles.loopfarmspeed then game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = kometa.vars.farmspeed end
            if kometa.toggles.randomizespeed then api.humanoid().WalkSpeed = math.random(30, 70) end
            repeat 
                api.humanoid().AutoRotate = false
                api.humanoid():MoveTo(v.Position) 
                task.wait()
            until rtsg().ModifierCaches.Value.FlameHeat['_'] >= 0.9 or not v or not v.Parent or (not v:WaitForChild('PF').Enabled and not v:WaitForChild('PS').Enabled) 
            api.humanoid().AutoRotate = true
            break
        end
    end
end

function getglitchtoken()
    for i,v in pairs(game:GetService("Workspace").Camera.DupedTokens:GetChildren()) do
        if tonumber((v.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude) < temptable.magnitude/1.4 then
            if kometa.toggles.faceballoons and findballoon() then api.humanoidrootpart().CFrame = CFrame.lookAt(api.humanoidrootpart().Position, Vector3.new(findballoon().BalloonRoot.Position.X, api.humanoidrootpart().Position.Y, findballoon().BalloonRoot.Position.Z)) end
            if kometa.toggles.faceflames and findclosestflame() then api.humanoidrootpart().CFrame = CFrame.lookAt(api.humanoidrootpart().Position, Vector3.new(findclosestflame().Position.X, api.humanoidrootpart().Position.Y, findclosestflame().Position.Z)) end
            if kometa.toggles.randomizespeed then api.humanoid().WalkSpeed = math.random(30, 70) end
            repeat
                api.humanoid().AutoRotate = false
                api.humanoid():MoveTo(v.Position)
                task.wait()
            until not v or not v.Parent
            api.humanoid().AutoRotate = true
            break
        end
    end
end

function avoidmob()
    for i,v in next, game:GetService("Workspace").Monsters:GetChildren() do
        if v:FindFirstChild("Head") then
            if (v.Head.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude < 35 and api.humanoid():GetState() ~= Enum.HumanoidStateType.Freefall then
                game.Players.LocalPlayer.Character.Humanoid.Jump = true
            end
        end
    end
end

function getcrosshairs(v)
    if v.BrickColor ~= BrickColor.new("Lime green") and v.BrickColor ~= BrickColor.new("Flint") then
    if temptable.crosshair then repeat task.wait() until not temptable.crosshair end
    temptable.crosshair = true
    -- api.walkTo(v.Position)
    repeat 
        task.wait() 
        api.walkTo(v.Position)
    until not v.Parent or v.BrickColor == BrickColor.new("Forest green") or v.BrickColor == BrickColor.new("Royal purple")
    task.wait(.1)
    temptable.crosshair = false
    table.remove(temptable.crosshairs, table.find(temptable.crosshairs, v))
    else
        table.remove(temptable.crosshairs, table.find(temptable.crosshairs, v))
    end
end

function makequests()
    pcall(function()
    for i,v in next, game:GetService("Workspace").NPCs:GetChildren() do
        if v.Name ~= "Ant Challenge Info" and v.Name ~= "Bubble Bee Man 2" and v.Name ~= "Wind Shrine" and v.Name ~= "Gummy Bear" then if v:FindFirstChild("Platform") then if v.Platform:FindFirstChild("AlertPos") then if v.Platform.AlertPos:FindFirstChild("AlertGui") then if v.Platform.AlertPos.AlertGui:FindFirstChild("ImageLabel") then
            image = v.Platform.AlertPos.AlertGui.ImageLabel
            button = game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.ActivateButton.MouseButton1Click
            if image.ImageTransparency == 0 then
                if kometa.toggles.tptonpc then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(v.Platform.Position.X, v.Platform.Position.Y+3, v.Platform.Position.Z)
                    task.wait(1)
                else
                    api.tween(2,CFrame.new(v.Platform.Position.X, v.Platform.Position.Y+3, v.Platform.Position.Z))
                    task.wait(3)
                end
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(v.Platform.Position.X, v.Platform.Position.Y+3, v.Platform.Position.Z)
                task.wait(.1)
                VirtualPressButton('E')
                task.wait(8)
                if image.ImageTransparency == 0 then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(v.Platform.Position.X, v.Platform.Position.Y+3, v.Platform.Position.Z)
                    task.wait(.1)
                    VirtualPressButton('E')
                end
                task.wait(2)
            end
        end     
    end end end end end end)
end

function VirtualPressButton(Button)
    game:GetService('VirtualInputManager'):SendKeyEvent(true, Button, false, nil)
end

function CheckToyCooldown(Toy)
    return (os.time() - (rtsg().ToyTimes[Toy] or math.huge) + 10 ) > game:GetService("Workspace").Toys[Toy].Cooldown.Value or false
end

function UseDispensers()
    if kometa.toggles.honeystorm and CheckToyCooldown('Honeystorm') then
        -- repeat api.teleport(CFrame.new(ToysFolder['Honeystorm'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Honeystorm') 
        api.teleport(CFrame.new(ToysFolder['Honeystorm'].Platform.CFrame.Position) * CFrame.new(0,3,0))
        game.ReplicatedStorage.Events.ToyEvent:FireServer("Honeystorm")
    end
    if kometa.toggles.autospawnsprout and CheckToyCooldown('Sprout Summoner') then
        api.teleport(CFrame.new(ToysFolder['Sprout Summoner'].Platform.CFrame.Position) * CFrame.new(0,3,0))
        game.ReplicatedStorage.Events.ToyEvent:FireServer("Sprout Summoner")
    end
    if kometa.toggles.autodispense then
        if kometa.dispensesettings.rj and CheckToyCooldown('Free Royal Jelly Dispenser') then
            api.teleport(CFrame.new(ToysFolder['Free Royal Jelly Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0))
            -- repeat api.teleport(CFrame.new(ToysFolder['Free Royal Jelly Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Free Royal Jelly Dispenser')
            game.ReplicatedStorage.Events.ToyEvent:FireServer("Free Royal Jelly Dispenser")
        end
        if kometa.dispensesettings.blub and CheckToyCooldown('Blueberry Dispenser') then
            api.teleport(CFrame.new(ToysFolder['Blueberry Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0))
            -- repeat api.teleport(CFrame.new(ToysFolder['Blueberry Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Blueberry Dispenser') 
            game.ReplicatedStorage.Events.ToyEvent:FireServer("Blueberry Dispenser")
        end
        if kometa.dispensesettings.straw and CheckToyCooldown('Strawberry Dispenser') then
            api.teleport(CFrame.new(ToysFolder['Strawberry Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0))
            -- repeat api.teleport(CFrame.new(ToysFolder['Strawberry Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Strawberry Dispenser') 
            game.ReplicatedStorage.Events.ToyEvent:FireServer("Strawberry Dispenser")
        end
        if kometa.dispensesettings.treat and CheckToyCooldown('Treat Dispenser') then
            api.teleport(CFrame.new(ToysFolder['Treat Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0))
            -- repeat api.teleport(CFrame.new(ToysFolder['Treat Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Treat Dispenser') 
            game.ReplicatedStorage.Events.ToyEvent:FireServer("Treat Dispenser")
        end
        if kometa.dispensesettings.coconut and CheckToyCooldown('Coconut Dispenser') then
            api.teleport(CFrame.new(ToysFolder['Coconut Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0))
            -- repeat api.teleport(CFrame.new(ToysFolder['Coconut Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Coconut Dispenser') 
            game.ReplicatedStorage.Events.ToyEvent:FireServer("Coconut Dispenser")
        end
        if kometa.dispensesettings.glue and CheckToyCooldown('Glue Dispenser') then
            api.teleport(CFrame.new(ToysFolder['Glue Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0))
            -- repeat api.teleport(CFrame.new(ToysFolder['Glue Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Glue Dispenser') 
            game.ReplicatedStorage.Events.ToyEvent:FireServer("Glue Dispenser")
        end
    end
    if kometa.toggles.autoboosters then
        if kometa.dispensesettings.white and CheckToyCooldown('Field Booster') then
            api.teleport(CFrame.new(ToysFolder['Field Booster'].Platform.CFrame.Position) * CFrame.new(0,3,0))
            -- repeat api.teleport(CFrame.new(ToysFolder['Field Booster'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Field Booster') 
            game.ReplicatedStorage.Events.ToyEvent:FireServer("Field Booster")
        end
        if kometa.dispensesettings.red and CheckToyCooldown('Red Field Booster') then
            api.teleport(CFrame.new(ToysFolder['Red Field Booster'].Platform.CFrame.Position) * CFrame.new(0,3,0))
            -- repeat api.teleport(CFrame.new(ToysFolder['Red Field Booster'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Red Field Booster') 
            game.ReplicatedStorage.Events.ToyEvent:FireServer("Red Field Booster")
        end
        if kometa.dispensesettings.blue and CheckToyCooldown('Blue Field Booster') then
            api.teleport(CFrame.new(ToysFolder['Blue Field Booster'].Platform.CFrame.Position) * CFrame.new(0,3,0))
            -- repeat api.teleport(CFrame.new(ToysFolder['Blue Field Booster'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Blue Field Booster') 
            game.ReplicatedStorage.Events.ToyEvent:FireServer("Blue Field Booster")
        end
    end
    if kometa.toggles.clock and CheckToyCooldown('Wealth Clock') then
        api.teleport(CFrame.new(ToysFolder['Wealth Clock'].Platform.CFrame.Position) * CFrame.new(0,3,0))
        -- repeat api.teleport(CFrame.new(ToysFolder['Wealth Clock'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Wealth Clock') 
        game.ReplicatedStorage.Events.ToyEvent:FireServer("Wealth Clock")
    end
    if kometa.toggles.freeantpass and CheckToyCooldown('Free Ant Pass Dispenser') and rtsg().Eggs.AntPass < 10 then
        api.teleport(CFrame.new(ToysFolder['Free Ant Pass Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0))
        -- repeat api.teleport(CFrame.new(ToysFolder['Free Ant Pass Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Free Ant Pass Dispenser') 
        game.ReplicatedStorage.Events.ToyEvent:FireServer("Free Ant Pass Dispenser")
    end
    if kometa.toggles.freerobopass and CheckToyCooldown('Free Robo Pass Dispenser') and rtsg().Eggs.RoboPass < 10 then
        api.teleport(CFrame.new(ToysFolder['Free Robo Pass Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0))
        -- repeat api.teleport(CFrame.new(ToysFolder['Free Robo Pass Dispenser'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Free Robo Pass Dispenser') 
        game.ReplicatedStorage.Events.ToyEvent:FireServer("Free Robo Pass Dispenser")
    end
    if kometa.toggles.autosamovar and CheckToyCooldown("Samovar") and game:GetService("Workspace").Toys['Samovar']:FindFirstChild("ModelAfter") then
        -- repeat api.teleport(CFrame.new(ToysFolder['Samovar'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Samovar')
        api.teleport(CFrame.new(ToysFolder['Samovar'].Platform.CFrame.Position) * CFrame.new(0,3,0))
        game.ReplicatedStorage.Events.ToyEvent:FireServer("Samovar")
        platformm = game:GetService("Workspace").Toys.Samovar.Platform
        task.wait(2)
        for i,v in pairs(game.Workspace.Collectibles:GetChildren()) do
            if (v.Position-platformm.Position).magnitude < 25 and v.CFrame.YVector.Y == 1 then
                repeat task.wait() api.humanoidrootpart().CFrame = CFrame.new(v.CFrame.Position) until not v or not v.Parent
            end
        end 
    end
    if kometa.toggles.autostockings and CheckToyCooldown("Stockings") and game:GetService("Workspace").Toys['Stockings']:FindFirstChild("ModelAfter") then
        -- repeat api.teleport(CFrame.new(ToysFolder['Stockings'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Stockings')
        api.teleport(CFrame.new(ToysFolder['Stockings'].Platform.CFrame.Position) * CFrame.new(0,3,0))
        game.ReplicatedStorage.Events.ToyEvent:FireServer("Stockings")
    end
    if kometa.toggles.autoonettart and CheckToyCooldown("Onett's Lid Art") and game:GetService("Workspace").Toys["Onett's Lid Art"]:FindFirstChild("ModelAfter") then
        -- repeat api.teleport(CFrame.new(ToysFolder["Onett's Lid Art"].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown("Onett's Lid Art")
        api.teleport(CFrame.new(ToysFolder["Onett's Lid Art"].Platform.CFrame.Position) * CFrame.new(0,3,0))
        game.ReplicatedStorage.Events.ToyEvent:FireServer("Onett's Lid Art")
    end
    if kometa.toggles.autocandles and CheckToyCooldown("Honeyday Candles") and game:GetService("Workspace").Toys['Honeyday Candles']:FindFirstChild("ModelAfter") then
        -- repeat api.teleport(CFrame.new(ToysFolder['Honeyday Candles'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Honeyday Candles')
        api.teleport(CFrame.new(ToysFolder['Honeyday Candles'].Platform.CFrame.Position) * CFrame.new(0,3,0))
        game.ReplicatedStorage.Events.ToyEvent:FireServer("Honeyday Candles")
    end
    if kometa.toggles.autofeast and CheckToyCooldown('Beesmas Feast') and game:GetService("Workspace").Toys['Beesmas Feast']:FindFirstChild("ModelAfter") then
        -- repeat api.teleport(CFrame.new(ToysFolder['Beesmas Feast'].Platform.CFrame.Position) * CFrame.new(0,3,0)) task.wait() VirtualPressButton('E') until not CheckToyCooldown('Beesmas Feast')
        api.teleport(CFrame.new(ToysFolder['Beesmas Feast'].Platform.CFrame.Position) * CFrame.new(0,3,0))
        game.ReplicatedStorage.Events.ToyEvent:FireServer("Beesmas Feast")
    end
end

local ui = library.new(true, "kometa в„пёЏ | v"..temptable.version)
ui.ChangeToggleKey(Enum.KeyCode.Semicolon)

local hometab = ui:Category("Home")
local farmtab = ui:Category("Farming")
local planterstab = ui:Category("Planters")
local combtab = ui:Category("Combat")
local statstab = ui:Category("Statistics")
local wayptab = ui:Category("Waypoints")
local hivetab = ui:Category("Hive")
local misctab = ui:Category("Misc")
local extrtab = ui:Category("Extra")
local Configs_Category = ui:Category("Configs")
local setttab = ui:Category("Settings")

local main = hometab:Sector("Main")
main:Cheat("Label", "Thanks you for using our script!")
main:Cheat("Label", "Another official fork of kocmoc by notweuz")
main:Cheat("Label", "Script by mrdevl, .anon, cryptozen and notweuz")
main:Cheat("Label", "Script version: "..temptable.version)
--information:Cheat("Button", "Discord Invite", function() setclipboard("https://discord.gg/2a5gVpcpzv") end)
--information:Cheat("Button", "Donation", function() setclipboard("https://qiwi.com/n/W33UZ") end)
local signs = hometab:Sector("Signs")
signs:Cheat("Label", "вљ пёЏ - Not Safe Function")
signs:Cheat("Label", "вљ™ - Configurable Function")
signs:Cheat("Label", "вЏі - Update Function Preview")
local links = hometab:Sector("Links")
links:Cheat("Button", "Discord Server", function() setclipboard("https://discord.gg/2a5gVpcpzv") end, {text=""})
links:Cheat("Button", "Token IDs", function() setclipboard("https://pastebin.com/raw/wtHBD3ij") end, {text=""})

local farm = farmtab:Sector("Farming")
farm:Cheat("Dropdown", "Field", function(Option) temptable.tokensfarm = false kometa.vars.field = Option end, {options=fieldstable})
farm:Cheat("Slider", "Convert at:", function(Value) kometa.vars.convertat = Value end, {min = 0, max = 100, suffix = "%", default = 100})
farm:Cheat("Checkbox", "Autofarm", function(State) kometa.toggles.autofarm = State end)
farm:Cheat("Checkbox", "Auto Sprinkler вЏі", function(State) kometa.toggles.autosprinkler = State end)
farm:Cheat("Checkbox", "Auto Dig", function(State) kometa.toggles.autodig = State end)
farm:Cheat("Checkbox", "Auto Equip Field Masks", function(State) kometa.toggles.automasks = State end)
farm:Cheat("Checkbox", "Farm Bubbles", function(State) kometa.toggles.farmbubbles = State end)
farm:Cheat("Checkbox", "Bubble Bloat Helper", function(State) kometa.toggles.bloatfarm = State end)
farm:Cheat("Checkbox", "Farm Flames", function(State) kometa.toggles.farmflame = State end)
farm:Cheat("Checkbox", "Farm Coconuts & Shower", function(State) kometa.toggles.farmcoco = State end)
farm:Cheat("Checkbox", "Farm Precise Crosshairs", function(State) kometa.toggles.collectcrosshairs = State end)
farm:Cheat("Checkbox", "Farm Fuzzy Bombs", function(State) kometa.toggles.farmfuzzy = State end)
farm:Cheat("Checkbox", "Farm Glitched Tokens", function(State) kometa.toggles.farmglitchedtokens = State end)
farm:Cheat("Checkbox", "Farm Under Balloons", function(State) kometa.toggles.farmballoons = State end)
farm:Cheat("Checkbox", "Farm Under Clouds", function(State) kometa.toggles.farmclouds = State end)
farm:Cheat("Checkbox", "Face Flames", function(State) kometa.toggles.faceflames = State end)
farm:Cheat("Checkbox", "Face Balloons", function(State) kometa.toggles.faceballoons = State end)
farm:Cheat("Checkbox", "Auto Accept/Confirm Quests вљ™", function(State) kometa.toggles.autoquest = State end)
farm:Cheat("Checkbox", "Auto Do Quests вљ™", function(State) kometa.toggles.autodoquest = State end)
local farmsecond = farmtab:Sector("Farming")
farmsecond:Cheat("Checkbox", "Auto Dispenser вљ™", function(State) kometa.toggles.autodispense = State end)
farmsecond:Cheat("Checkbox", "Auto Field Boosters вљ™", function(State) kometa.toggles.autoboosters = State end)
farmsecond:Cheat("Checkbox", "Auto Wealth Clock", function(State) kometa.toggles.clock = State end)
farmsecond:Cheat("Checkbox", "Auto Free Antpasses", function(State) kometa.toggles.freeantpass = State end)
farmsecond:Cheat("Checkbox", "Auto Free Robo Passes", function(State) kometa.toggles.freerobopass = State end)
farmsecond:Cheat("Checkbox", "Auto Special Sprout Summoner", function(State) kometa.toggles.autospawnsprout = State end)
farmsecond:Cheat("Checkbox", "Auto Honeystorm", function(State) kometa.toggles.honeystorm = State end)
-- farmsecond:Cheat("Checkbox", "Auto Gingerbread Bears", function(State) kometa.toggles.collectgingerbreads = State end) РћРЅРµС‚С‚ РіРµР№
farmsecond:Cheat("Checkbox", "Auto Samovar", function(State) kometa.toggles.autosamovar = State end)
farmsecond:Cheat("Checkbox", "Auto Stockings", function(State) kometa.toggles.autostockings = State end)
farmsecond:Cheat("Checkbox", "Auto Honey Candles", function(State) kometa.toggles.autocandles = State end)
farmsecond:Cheat("Checkbox", "Auto Beesmas Feast", function(State) kometa.toggles.autofeast = State end)
farmsecond:Cheat("Checkbox", "Auto Onett's Lid Art", function(State) kometa.toggles.autoonettart = State end)
farmsecond:Cheat("Checkbox", "Farm Snowflakes", function(State) kometa.toggles.farmsnowflakes = State end)
farmsecond:Cheat("Checkbox", "Farm Sprouts", function(State) kometa.toggles.farmsprouts = State end)
farmsecond:Cheat("Checkbox", "Farm Puffshrooms", function(State) kometa.toggles.farmpuffshrooms = State end)
farmsecond:Cheat("Checkbox", "Farm Tickets вљ пёЏ", function(State) kometa.toggles.farmtickets = State end)
farmsecond:Cheat("Checkbox", "Teleport To Rares вљ пёЏ", function(State) kometa.toggles.farmrares = State end)
-- farmsecond:Cheat("Checkbox", "Use Instant Converters", function(State) kometa.toggles.instantconverters = State end)

local psec1 = planterstab:Sector("First Planter")
psec1:Cheat("Dropdown", "Planter", function(Option) kometa.planterssettings[1].Type = Option end, {options=require(game:GetService("ReplicatedStorage").PlanterTypes).INVENTORY_ORDER})
psec1:Cheat("Dropdown", "Field", function(Option) kometa.planterssettings[1].field = Option end, {options=fieldstable})
psec1:Cheat("Slider", "Growth Percent", function(Value) kometa.planterssettings[1].growth = (Value / 100) or 1 / 100 end, {min = 0, max = 100, suffix = "%", default = 100})
psec1:Cheat("Checkbox", "Auto Plant", function(State) kometa.planterssettings[1].enabled = State end)

local psec2 = planterstab:Sector("Second Planter")
psec2:Cheat("Dropdown", "Planter", function(Option) kometa.planterssettings[2].Type = Option end, {options=require(game:GetService("ReplicatedStorage").PlanterTypes).INVENTORY_ORDER})
psec2:Cheat("Dropdown", "Field", function(Option) kometa.planterssettings[2].field = Option end, {options=fieldstable})
psec2:Cheat("Slider", "Growth Percent", function(Value) kometa.planterssettings[2].growth = (Value / 100) or 1 / 100 end, {min = 0, max = 100, suffix = "%", default = 100})
psec2:Cheat("Checkbox", "Auto Plant", function(State) kometa.planterssettings[2].enabled = State end)

local psec3 = planterstab:Sector("Third Planter")
psec3:Cheat("Dropdown", "Planter", function(Option) kometa.planterssettings[3].Type = Option end, {options=require(game:GetService("ReplicatedStorage").PlanterTypes).INVENTORY_ORDER})
psec3:Cheat("Dropdown", "Field", function(Option) kometa.planterssettings[3].field = Option end, {options=fieldstable})
psec3:Cheat("Slider", "Growth Percent", function(Value) kometa.planterssettings[3].growth = (Value / 100) or 1 / 100 end, {min = 0, max = 100, suffix = "%", default = 100})
psec3:Cheat("Checkbox", "Auto Plant", function(State) kometa.planterssettings[3].enabled = State end)

local mobkill = combtab:Sector("Combat")
mobkill:Cheat("Checkbox", "Train Crab", function(State) if State then api.teleport(CFrame.new(-375, 110, 535)) task.wait(5) api.humanoidrootpart().CFrame = CFrame.new(-256, 110, 475) end cocopad.CanCollide = State kometa.toggles.traincrab = State end)
mobkill:Cheat("Checkbox", "Train Snail", function(State) kometa.toggles.trainsnail = State fd = game.Workspace.FlowerZones['Stump Field'] if State then api.humanoidrootpart().CFrame = CFrame.new(fd.Position.X, fd.Position.Y-10, fd.Position.Z) else api.humanoidrootpart().CFrame = CFrame.new(fd.Position.X, fd.Position.Y+2, fd.Position.Z) end end)
mobkill:Cheat("Checkbox", "Kill Mondo", function(State) kometa.toggles.killmondo = State end)
mobkill:Cheat("Checkbox", "Kill Vicious", function(State) kometa.toggles.killvicious = State end)
mobkill:Cheat("Checkbox", "Kill Windy", function(State) kometa.toggles.killwindy = State end)
mobkill:Cheat("Checkbox", "Auto Kill Mobs", function(State) kometa.toggles.autokillmobs = State end)
mobkill:Cheat("Checkbox", "Avoid Mobs", function(State) kometa.toggles.avoidmobs = State end)
mobkill:Cheat("Checkbox", "Auto Ant", function(State) kometa.toggles.autoant = State end)

local amks = combtab:Sector("Auto Kill Mobs Settings")
amks:Cheat("Textbox",'Kill Mobs After x Convertions', function(Value) kometa.vars.monstertimer = tonumber(Value) end, { placeholder = 'default = 3'})

local statssector = statstab:Sector("Statistics")
statssector:Cheat("Label", "Gained Honey")
statssector:Cheat("Label", "Honey Per Hour")
statssector:Cheat("Label", "Elapsed Time")
statssector:Cheat("Label", "Farmed Sprouts")
statssector:Cheat("Label", "Killed Vicious")
statssector:Cheat("Label", "Killed Windy")
statssector:Cheat("Label", "Farmed Ants")

local mobstimers = statstab:Sector("Mob Timers")
mobstimers:Cheat("Label", "Coming soon!")

local wayp = wayptab:Sector("Waypoints")
wayp:Cheat("Dropdown", "Field Teleports", function(Option) game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game:GetService("Workspace").FlowerZones:FindFirstChild(Option).CFrame end, {options = fieldstable})
wayp:Cheat("Dropdown", "Monster Teleports", function(Option) d = game:GetService("Workspace").MonsterSpawners:FindFirstChild(Option) game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(d.Position.X, d.Position.Y+3, d.Position.Z) end, {options = spawnerstable})
wayp:Cheat("Dropdown", "Toys Teleports", function(Option) d = game:GetService("Workspace").Toys:FindFirstChild(Option).Platform game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(d.Position.X, d.Position.Y+3, d.Position.Z) end, {options = toystable})
wayp:Cheat("Button", "Teleport to hive", function() game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game:GetService("Players").LocalPlayer.SpawnPos.Value end, {placeholder = ' '})

local ghive = hivetab:Sector("Hive Position && Food Type")
ghive:Cheat("Textbox", "X", function(Value) kometa.beessettings.general.x = Value end, {placeholder = ' '})
ghive:Cheat("Textbox", "Y", function(Value) kometa.beessettings.general.y = Value end, {placeholder = ' '})
ghive:Cheat("Textbox", "Amount", function(Value) kometa.beessettings.general.amount = tonumber(Value) or 1 end, {placeholder = ' '})
ghive:Cheat("Dropdown", "Food Type", function(Option) kometa.beessettings.foodtype = Option end, { options = {"Treat", "SunflowerSeed", "Blueberry", "Strawberry", "Bitterberry", "Pineapple"--[[,"GingerbreadBear"]]}})
local arjhive = hivetab:Sector("Auto Royal Jelly")
arjhive:Cheat("Dropdown", "Bee", function(Value) kometa.beessettings.usb = Value end, {options = beestable})
arjhive:Cheat("Checkbox", "Until Selected Bee", function(State) kometa.beessettings.usbtoggle = State if not State then game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.BeePopUp.TypeName.Text = "" end end)
local ugbhive = hivetab:Sector("Autofeed")
ugbhive:Cheat("Checkbox", "Food Until Gifted", function(State) kometa.beessettings.ugb = State if not State then game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.BeePopUp.TypeName.Text = "" end end)
ugbhive:Cheat("Checkbox", "Auto Feed", function(State) kometa.beessettings.af = State end)
local feedhive = hivetab:Sector("Manual Feeding")
feedhive:Cheat("Button", "Feed Selected Bee", function()
    temptable.feed(kometa.beessettings.general.x, kometa.beessettings.general.y, kometa.beessettings.foodtype, kometa.beessettings.general.amount)
end, {text = 'Feed'})
feedhive:Cheat("Button", "Feed All Bees", function()
    for xbee = 1, 5, 1 do
        for ybee = 1, 10, 1 do
            temptable.feed(xbee, ybee, kometa.beessettings.foodtype, kometa.beessettings.general.amount)
        end
    end
end, {text = 'Feed'})
local umhive = hivetab:Sector("Mutation Rolling")
umhive:Cheat("Dropdown", "Mutation", function(Option) kometa.beessettings.mutation = Option end, {options = {"Convert Amount", "Gather Amount", "Ability Rate", "Attack", "Energy"}})
umhive:Cheat("Checkbox", "Roll Until Mutation", function(State) kometa.beessettings.umb = State if not State then game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.BeePopUp.MutationFrame.MutationLabel.Text.Text = "" end end)

local miscc = misctab:Sector("Misc")
miscc:Cheat("Button", "Ant Challenge Semi-Godmode", function() api.tween(1, CFrame.new(93.4228, 32.3983, 553.128)) task.wait(1) game.ReplicatedStorage.Events.ToyEvent:FireServer("Ant Challenge") game.Players.LocalPlayer.Character.HumanoidRootPart.Position = Vector3.new(93.4228, 42.3983, 553.128) task.wait(2) game.Players.LocalPlayer.Character.Humanoid.Name = 1 local l = game.Players.LocalPlayer.Character["1"]:Clone() l.Parent = game.Players.LocalPlayer.Character l.Name = "Humanoid" task.wait() game.Players.LocalPlayer.Character["1"]:Destroy() api.tween(1, CFrame.new(93.4228, 32.3983, 553.128)) task.wait(8) api.tween(1, CFrame.new(93.4228, 32.3983, 553.128)) end, {text = ''})
miscc:Cheat("Checkbox", "Walk Speed", function(State) kometa.toggles.loopspeed = State end)
miscc:Cheat("Checkbox", "Jump Power", function(State) kometa.toggles.loopjump = State end)
miscc:Cheat("Checkbox", "Godmode", function(State) kometa.toggles.godmode = State if State then bssapi:Godmode(true) else bssapi:Godmode(false) end end)
miscc:Cheat("Checkbox", "Always Visual Night", function(State) kometa.toggles.visualnight = State end)

local webhooking = misctab:Sector("Discord Webhooking")
webhooking:Cheat("Textbox", "Webhook", function(Value) kometawebhook.webhook = Value end, {placeholder = ' '})
webhooking:Cheat("Button", "Remind Webhook", function() api.notify('kometa', 'Your webhook is '..kometawebhook.webhook) end, {text = ''})
webhooking:Cheat("Checkbox", "Send After Converting", function(State) kometa.webhooking.convert = State end)
webhooking:Cheat("Checkbox", "Send On Vicious", function(State) kometa.webhooking.vicious = State end)
webhooking:Cheat("Checkbox", "Send On Windy", function(State) kometa.webhooking.windy = State end)
webhooking:Cheat("Checkbox", "Send When Player Joins Server", function(State) kometa.webhooking.playeradded = State end)

local AutoUsing = misctab:Sector("Auto Buffs")
AutoUsing:Cheat("Checkbox", "Auto Use Red Extract", function(State) kometa.AutoUseSettings['Red Extract'] = State end)
AutoUsing:Cheat("Checkbox", "Auto Use Blue Extract", function(State) kometa.AutoUseSettings['Blue Extract'] = State end)
AutoUsing:Cheat("Checkbox", "Auto Use Glitter", function(State) kometa.AutoUseSettings['Glitter'] = State end)
AutoUsing:Cheat("Checkbox", "Auto Use Glue", function(State) kometa.AutoUseSettings['Glue'] = State end)
AutoUsing:Cheat("Checkbox", "Auto Use Oil", function(State) kometa.AutoUseSettings['Oil'] = State end)
AutoUsing:Cheat("Checkbox", "Auto Use Enzymes", function(State) kometa.AutoUseSettings['Enzymes'] = State end)
AutoUsing:Cheat("Checkbox", "Auto Use Tropical Drink", function(State) kometa.AutoUseSettings['Tropical Drink'] = State end)
AutoUsing:Cheat("Checkbox", "Auto Use Purple Potion", function(State) kometa.AutoUseSettings['Purple Potion'] = State end)
AutoUsing:Cheat("Checkbox", "Auto Use Super Smoothie", function(State) kometa.AutoUseSettings['Super Smoothie'] = State end)

local misco = misctab:Sector("Other")
misco:Cheat("Dropdown", "Equip Accesories", function(Option) local ohString1 = "Equip" local ohTable2 = { ["Mute"] = false, ["Type"] = Option, ["Category"] = "Accessory" } game:GetService("ReplicatedStorage").Events.ItemPackageEvent:InvokeServer(ohString1, ohTable2) end, {options = accesoriestable})
misco:Cheat("Dropdown", "Equip Masks", function(Option) local ohString1 = "Equip" local ohTable2 = { ["Mute"] = false, ["Type"] = Option, ["Category"] = "Accessory" } game:GetService("ReplicatedStorage").Events.ItemPackageEvent:InvokeServer(ohString1, ohTable2) end, {options = masktable})
misco:Cheat("Dropdown", "Equip Collectors", function(Option) local ohString1 = "Equip" local ohTable2 = { ["Mute"] = false, ["Type"] = Option, ["Category"] = "Collector" } game:GetService("ReplicatedStorage").Events.ItemPackageEvent:InvokeServer(ohString1, ohTable2) end, {options = collectorstable})
misco:Cheat("Dropdown", "Generate Amulet", function(Option) local A_1 = Option.." Generator" local Event = game:GetService("ReplicatedStorage").Events.ToyEvent Event:FireServer(A_1) end, {options = {"Supreme Star Amulet", "Diamond Star Amulet", "Gold Star Amulet","Silver Star Amulet","Bronze Star Amulet","Moon Amulet"}})
misco:Cheat("Button", "Export Stats Table", function() local StatCache = require(game.ReplicatedStorage.ClientStatCache)writefile("Stats_"..api.nickname..".json", StatCache:Encode()) end, {text = ''})
misco:Cheat("Textbox", "Ping Spoofer", function(Option) settings():GetService("NetworkSettings").IncomingReplicationLag = tonumber(Option)/1000 or 0 end, {placeholder = 'Add ms to ping'})

local extras = extrtab:Sector("Extras")
extras:Cheat("Textbox", "Glider Speed", function(Value) local StatCache = require(game.ReplicatedStorage.ClientStatCache) local stats = StatCache:Get() stats.EquippedParachute = "Glider" local module = require(game:GetService("ReplicatedStorage").Parachutes) local st = module.GetStat local glidersTable = getupvalues(st) glidersTable[1]["Glider"].Speed = Value setupvalue(st, st[1]'Glider', glidersTable) end)
extras:Cheat("Textbox", "Glider Float", function(Value) local StatCache = require(game.ReplicatedStorage.ClientStatCache) local stats = StatCache:Get() stats.EquippedParachute = "Glider" local module = require(game:GetService("ReplicatedStorage").Parachutes) local st = module.GetStat local glidersTable = getupvalues(st) glidersTable[1]["Glider"].Float = Value setupvalue(st, st[1]'Glider', glidersTable) end)
extras:Cheat("Button", "Invisibility", function(State) api.teleport(CFrame.new(0,0,0)) wait(1) if game.Players.LocalPlayer.Character:FindFirstChild('LowerTorso') then Root = game.Players.LocalPlayer.Character.LowerTorso.Root:Clone() game.Players.LocalPlayer.Character.LowerTorso.Root:Destroy() Root.Parent = game.Players.LocalPlayer.Character.LowerTorso api.teleport(game:GetService("Players").LocalPlayer.SpawnPos.Value) end end, {text = ''})
extras:Cheat("Checkbox", "Float", function(State) temptable.float = State end)

local optimize = extrtab:Sector("Optimization")
optimize:Cheat("Button", "Hide nickname", function() loadstring(game:HttpGet("https://s.kometa.ga/other/nicknamespoofer.lua"))()end, {text = ''})
optimize:Cheat("Button", "Boost FPS", function()loadstring(game:HttpGet("https://s.kometa.ga/other/fpsboost.lua"))()end, {text = ''})
optimize:Cheat("Button", "Destroy Decals", function()loadstring(game:HttpGet("https://s.kometa.ga/other/destroydecals.lua"))()end, {text = ''})
optimize:Cheat("Checkbox", "Disable 3D Render On Unfocus", function(State) kometa.toggles.disablerender = State end)
optimize:Cheat("Checkbox", "Disable 3D Render", function(State) game:GetService("RunService"):Set3dRenderingEnabled(not State) end)

local windsh = extrtab:Sector("Wind Shrine")
windsh:Cheat("Dropdown", "Item", function(Option) kometa.vars.donatit[1] = Option end, {options=temptable.item_names}) 
windsh:Cheat("Textbox", "Count", function(Value) kometa.vars.donatit[2] = tonumber(Value) end)
windsh:Cheat("Checkbox", "Auto Donate", function(State) kometa.toggles.autodonate = State end)
windsh:Cheat("Checkbox", "Don't Spawn Drop On Donate", function(State) kometa.toggles.donotdonatedrop = State end)
windsh:Cheat("Button", "Donate", function()
    game.ReplicatedStorage.Events.WindShrineDonation:InvokeServer(kometa.vars.donatit[1], kometa.vars.donatit[2])
    if not kometa.toggles.donotdonatedrop then game.ReplicatedStorage.Events.WindShrineTrigger:FireServer() end
end, {text = 'Once'})
windsh:Cheat("Button", "Spawn Drop", function()
    game.ReplicatedStorage.Events.WindShrineTrigger:FireServer()
end, {text = 'Spawn'})

local farmsettings = setttab:Sector("Autofarm Settings")
-- farmsettings:Cheat("Textbox", "Autofarming Walkspeed", function(Value) kometa.vars.farmspeed = Value end, {placeholder = "Default Value = 60"})
-- farmsettings:Cheat("Checkbox", "^ Loop Speed On Autofarming", function(State) kometa.toggles.loopfarmspeed = State end)
farmsettings:Cheat("Checkbox", "Randomize Speed On Autofarming", function(State) kometa.toggles.randomizespeed = State end)
farmsettings:Cheat("Checkbox", "Don't Walk In Field", function(State) kometa.toggles.farmflower = State end)
farmsettings:Cheat("Checkbox", "Convert Hive Balloon", function(State) kometa.toggles.convertballoons = State end)
farmsettings:Cheat("Checkbox", "Don't Convert", function(State) kometa.toggles.donotconvert = State end)
farmsettings:Cheat("Checkbox", "Don't Farm Tokens", function(State) kometa.toggles.donotfarmtokens = State end)
farmsettings:Cheat("Checkbox", "Enable Token Blacklisting", function(State) kometa.toggles.enabletokenblacklisting = State end)
farmsettings:Cheat("Slider", "Walk Speed", function(Value) kometa.vars.walkspeed = Value end, {min = 0, max = 120, suffix = " studs", default = 70})
farmsettings:Cheat("Slider", "Jump Power", function(Value) kometa.vars.jumppower = Value end, {min = 0, max = 120, suffix = " studs", default = 70})
local raresettings = setttab:Sector("Tokens Settings")
raresettings:Cheat("Textbox", "Asset ID", function(Value) rarename = Value end, {placeholder = 'rbxassetid'})
raresettings:Cheat("Button", "Add Rare", function()
    table.insert(kometa.rares, rarename)
    game.CoreGui.kometaUI.Container.Categories.Settings:FindFirstChild("Tokens Settings", true).Container["Rares List"]:Destroy()
    raresettings:Cheat("Dropdown", "Rares List", function(Option)
    end, {
        options = kometa.rares
    })
end, {text = 'Add'})
raresettings:Cheat("Button", "Remove Rare", function()
    table.remove(kometa.rares, api.tablefind(kometa.rares, rarename))
    game.CoreGui.kometaUI.Container.Categories.Settings:FindFirstChild("Tokens Settings", true).Container["Rares List"]:Destroy()
    raresettings:Cheat("Dropdown", "Rares List", function(Option)
    end, {
        options = kometa.rares
    })
end, {text = 'Remove'})
raresettings:Cheat("Button", "Add To Blacklist", function()
    table.insert(kometa.bltokens, rarename)
    game.CoreGui.kometaUI.Container.Categories.Settings:FindFirstChild("Tokens Settings", true).Container["Tokens Blacklist"]:Destroy()
    raresettings:Cheat("Dropdown", "Tokens Blacklist", function(Option)
    end, {
        options = kometa.bltokens
    })
end, {text = 'Add'})
raresettings:Cheat("Button", "Remove From Blacklist", function()
    table.remove(kometa.bltokens, api.tablefind(kometa.bltokens, rarename))
    game.CoreGui.kometaUI.Container.Categories.Settings:FindFirstChild("Tokens Settings", true).Container["Tokens Blacklist"]:Destroy()
    raresettings:Cheat("Dropdown", "Tokens Blacklist", function(Option)
    end, {
        options = kometa.bltokens
    })
end, {text = 'Remove'})
raresettings:Cheat("Dropdown", "Tokens Blacklist", function(Option) end, {options = kometa.bltokens})
raresettings:Cheat("Dropdown", "Rares List", function(Option) end, {options = kometa.rares})
local dispsettings = setttab:Sector("Auto Dispenser & Auto Boosters Settings")
dispsettings:Cheat("Checkbox", "Royal Jelly Dispenser", function(State) kometa.dispensesettings.rj = not kometa.dispensesettings.rj end)
dispsettings:Cheat("Checkbox", "Blueberry Dispenser",  function(State) kometa.dispensesettings.blub = not kometa.dispensesettings.blub end)
dispsettings:Cheat("Checkbox", "Strawberry Dispenser",  function(State) kometa.dispensesettings.straw = not kometa.dispensesettings.straw end)
dispsettings:Cheat("Checkbox", "Treat Dispenser",  function(State) kometa.dispensesettings.treat = not kometa.dispensesettings.treat end)
dispsettings:Cheat("Checkbox", "Coconut Dispenser",  function(State) kometa.dispensesettings.coconut = not kometa.dispensesettings.coconut end)
dispsettings:Cheat("Checkbox", "Glue Dispenser",  function(State) kometa.dispensesettings.glue = not kometa.dispensesettings.glue end)
dispsettings:Cheat("Checkbox", "Mountain Top Booster",  function(State) kometa.dispensesettings.white = not kometa.dispensesettings.white end)
dispsettings:Cheat("Checkbox", "Blue Field Booster",  function(State) kometa.dispensesettings.blue = not kometa.dispensesettings.blue end)
dispsettings:Cheat("Checkbox", "Red Field Booster",  function(State) kometa.dispensesettings.red = not kometa.dispensesettings.red end)
local guisettings = setttab:Sector("GUI Settings")
guisettings:Cheat("Keybind", "Set toggle button", function(Value) ui.ChangeToggleKey(Value) end, {text = 'Set New'})
guisettings:Cheat("Dropdown", "GUI Options (Dropdown)", function(Option) if Option == "Color Reset" then game:GetService("CoreGui").kometaUI.Container.BackgroundColor3 = Color3.fromRGB(32,32,33) else game.CoreGui.kometaUI:Destroy() temptable.tokensfarm = false end end, { options = { "Destroy GUI", "Color Reset" } })
-- guisettings:Cheat("Checkbox", "Disable Separators", function(State)
--     if Settings.disableseperators == false then
--         Settings.disableseperators = true
--         for i,v in pairs(game:GetService("CoreGui").kometaUI.Container:GetChildren()) do
--             if v.Name == "Separator" then
--                 v.Visible = false
--             end
--             task.wait()
--         end
--     else
--         for i,v in pairs(game:GetService("CoreGui").kometaUI.Container:GetChildren()) do
--             if v.Name == "Separator" then
--                 v.Visible = true
--             end
--             task.wait()
--         end
--         Settings.disableseperators = false
--     end
-- end)
guisettings:Cheat("Checkbox", "RGB GUI", function(State)
    if kometa.toggles.rgbui == false then
        kometa.toggles.rgbui = true
            while kometa.toggles.rgbui do
                for hue = 0, 255, 4 do
                    if kometa.toggles.rgbui then
                        game.CoreGui.kometaUI.Container.BorderColor3 = Color3.fromHSV(hue/256, 1, 1)
                        game.CoreGui.kometaUI.Container.BackgroundColor3 = Color3.fromHSV(hue/256, .5, .8)
                        task.wait()
                    end
                end
            end
        else
        end
        kometa.toggles.rgbui = false
end)
guisettings:Cheat("ColorPicker", "GUI Color", function(Value) game:GetService("CoreGui").kometaUI.Container.BackgroundColor3 = Value end)
guisettings:Cheat("Textbox", "GUI Transparency", function(Value)game:GetService("CoreGui").kometaUI.Container.BackgroundTransparency = Value for i,v in pairs(game:GetService("CoreGui").kometaUI.Container:GetChildren()) do    if v.Name == "Separator" then    v.BackgroundTransparency = Value end end	game:GetService("CoreGui").kometaUI.Container.Shadow.ImageTransparency = Value end, { placeholder = "between 0 and 1"})
local fieldsettings = setttab:Sector("Fields Settings")
fieldsettings:Cheat("Dropdown", "Best White Field", function(Option) kometa.bestfields.white = Option end, {options = temptable.whitefields})
fieldsettings:Cheat("Dropdown", "Best Red Field", function(Option) kometa.bestfields.red = Option end, {options = temptable.redfields})
fieldsettings:Cheat("Dropdown", "Best Blue Field", function(Option) kometa.bestfields.blue = Option end, {options = temptable.bluefields})
fieldsettings:Cheat("Dropdown", "Field", function(Option) temptable.blackfield = Option end, {options = fieldstable})
fieldsettings:Cheat("Button", "Add Field To Blacklist", function() table.insert(kometa.blacklistedfields, temptable.blackfield) game:GetService("CoreGui").kometaUI.Container.Categories.Settings:FindFirstChild("Fields Settings", true).Container["Blacklisted Fields"]:Destroy() fieldsettings:Cheat("Dropdown", "Blacklisted Fields", function(Option) end, {options = kometa.blacklistedfields}) end, {text = ' '})
fieldsettings:Cheat("Button", "Remove Field From Blacklist", function() table.remove(kometa.blacklistedfields, api.tablefind(kometa.blacklistedfields, temptable.blackfield)) game:GetService("CoreGui").kometaUI.Container.Categories.Settings:FindFirstChild("Fields Settings", true).Container["Blacklisted Fields"]:Destroy() fieldsettings:Cheat("Dropdown", "Blacklisted Fields", function(Option) end, {options = kometa.blacklistedfields}) end, {text = ' '})
fieldsettings:Cheat("Dropdown", "Blacklisted Fields", function(Option) end, {options = kometa.blacklistedfields})
local pts = setttab:Sector("Autofarm Priority Tokens")
pts:Cheat("Textbox", "Asset ID", function(Value) rarename = Value end, {placeholder = 'rbxassetid'})
pts:Cheat("Button", "Add Token To Priority List", function() table.insert(kometa.priority, rarename) game:GetService("CoreGui").kometaUI.Container.Categories.Settings:FindFirstChild("Autofarm Priority Tokens", true).Container["Priority List"]:Destroy() pts:Cheat("Dropdown", "Priority List", function(Option) end, {options = kometa.priority}) end, {text = ''})
pts:Cheat("Button", "Remove Token From Priority List", function() table.remove(kometa.priority, api.tablefind(kometa.priority, rarename)) game:GetService("CoreGui").kometaUI.Container.Categories.Settings:FindFirstChild("Autofarm Priority Tokens", true).Container["Priority List"]:Destroy() pts:Cheat("Dropdown", "Priority List", function(Option) end, {options = kometa.priority}) end, {text = ''})
pts:Cheat("Dropdown", "Priority List", function(Option) end, {options = kometa.priority})
local aqs = setttab:Sector("Auto Quest Settings")
aqs:Cheat("Dropdown", "Do NPC Quests", function(Option) kometa.vars.npcprefer = Option end, {options = {'All Quests', 'Bucko Bee', 'Brown Bear', 'Riley Bee', 'Polar Bear'}})
aqs:Cheat("Checkbox", "Teleport To NPC", function(State) kometa.toggles.tptonpc = State end)

local Local_Configs = Configs_Category:Sector("Local Configs")
Local_Configs:Cheat("Textbox", "Config Name", function(Value) temptable.configname = Value end, {placeholder = 'ex: stumpconfig'})
Local_Configs:Cheat("Button", "Load Config", function() kometa = game:service'HttpService':JSONDecode(readfile("kometa/BSS_"..temptable.configname..".json")) kometawebhook = game:service'HttpService':JSONDecode(readfile("kometa/BSS_webhook_"..temptable.configname..".json")) end, {text = ' '})
Local_Configs:Cheat("Button", "Save Config", function() writefile("kometa/BSS_"..temptable.configname..".json",game:service'HttpService':JSONEncode(kometa)) writefile("kometa/BSS_webhook_"..temptable.configname..".json",game:service'HttpService':JSONEncode(kometawebhook)) end, {text = ' '})
Local_Configs:Cheat("Button", "Reset Config", function() kometa = defaultkometa kometawebhook = defaultkometawebhook end, {text = ' '})

if (syn or Krnl or (identifyexecutor() and identifyexecutor() == 'ScriptWare')) and temptable.WebSocket then
    local Global_Configs = Configs_Category:Sector("Global Configs")
    Global_Configs:Cheat("Textbox", "Config ID", function(Value) temptable.globalconfigname = Value end, {placeholder = 'ex: OdACVVunos'})
    Global_Configs:Cheat("Textbox", "Description", function(Value) temptable.GlobalConfigDescription = Value end, {placeholder = 'ex: pine tree autofarm'})
    Global_Configs:Cheat("Button", "Load Configs", function() temptable.WebSocket:Send(game:service'HttpService':JSONEncode({ action = 'ConfigLoad', hwid = game:GetService("RbxAnalyticsService"):GetClientId(), id = temptable.globalconfigname })) end, {text = ' '})
    Global_Configs:Cheat("Button", "Save Configs", function() temptable.WebSocket:Send(game:service'HttpService':JSONEncode({ hwid = game:GetService("RbxAnalyticsService"):GetClientId(), action = 'ConfigSave', cfg = game:service'HttpService':JSONEncode(kometa), ver = temptable.version, desc = temptable.GlobalConfigDescription })) end, {text = ' '})
end


task.spawn(function() while task.wait() do
    if kometa.toggles.autofarm then
        --if kometa.toggles.farmcoco then getcoco() end
        --if kometa.toggles.collectcrosshairs then getcrosshairs() end
        if kometa.toggles.farmflame then getflame() end
        if kometa.toggles.farmglitchedtokens then getglitchtoken() end
        -- if kometa.toggles.farmfuzzy then getfuzzy() end
    end
end end)

game.Workspace.Particles.ChildAdded:Connect(function(v)
    if not temptable.started.vicious and not temptable.started.ant then
        if v.Name == "WarningDisk" and not temptable.started.vicious and kometa.toggles.autofarm and not temptable.started.ant and kometa.toggles.farmcoco and (v.Position-api.humanoidrootpart().Position).magnitude < temptable.magnitude and not temptable.converting then
            table.insert(temptable.coconuts, v)
            getcoco(v)
            gettoken()
        elseif v.Name == "Crosshair" then
            task.wait(.2)
            if v ~= nil and v.BrickColor ~= BrickColor.new("Forest green") and not temptable.started.ant and v.BrickColor ~= BrickColor.new("Flint") and (v.Position-api.humanoidrootpart().Position).magnitude < temptable.magnitude and kometa.toggles.autofarm and kometa.toggles.collectcrosshairs and not temptable.converting then
                if #temptable.crosshairs <= 3 then
                    table.insert(temptable.crosshairs, v)
                    getcrosshairs(v)
                    gettoken()
                end
            end
        elseif v.Name == "DustBunnyInstance" then
            task.wait(.1)
            if v:FindFirstChild("Plane") then
                if kometa.toggles.farmfuzzy and kometa.toggles.autofarm and tonumber((v.Plane.Position-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude) < temptable.magnitude/1.4 then
                    -- repeat api.teleport(CFrame.new(v:FindFirstChild("Plane").Position)) task.wait() until v ~= nil or v.Parent ~= nil
                    table.insert(temptable.FuzzyBombs, v)
                    getfuzzy(v)
                end
            end
        end
    end
end)

game.Workspace.Particles.Snowflakes.ChildAdded:Connect(function(Object)
    if kometa.toggles.farmsnowflakes and temptable.collecting.snowflake == false then farmsnowflakes(Object) end
end)

task.spawn(function() while task.wait(5) do
    UseDispensers()
end end)

task.spawn(function() while task.wait(1) do
    if kometa.toggles.automasks and api.humanoidrootpart() then
        if findFieldWithRay(api.humanoidrootpart().Position, Vector3.new(0,-90,0)) then
            local MaskField = findFieldWithRay(api.humanoidrootpart().Position, Vector3.new(0,-90,0))
            local FieldColor
            if MaskField:FindFirstChild("ColorGroup") then FieldColor = MaskField:FindFirstChild("ColorGroup").Value else FieldColor = 'White' end
            if temptable.LastFieldColor == FieldColor then continue end
            temptable.LastFieldColor = FieldColor
            if FieldColor == 'Blue' then
                maskequip('Bubble Mask')
                maskequip('Diamond Mask')
            elseif FieldColor == 'Red' then
                maskequip('Fire Mask')
                maskequip('Demon Mask')
            else
                maskequip('Honey Mask')
                maskequip('Gummy Mask')
            end
        end
    end
end end)

task.spawn(function() while task.wait() do
    if temptable.collecting.tickets then continue end
    if temptable.collecting.rares then continue end
    if kometa.toggles.autofarm then
        temptable.magnitude = 70
        if game.Players.LocalPlayer.Character:FindFirstChild("ProgressLabel",true) then
        local pollenprglbl = game.Players.LocalPlayer.Character:FindFirstChild("ProgressLabel",true)
        maxpollen = tonumber(pollenprglbl.Text:match("%d+$"))
        local pollencount = game.Players.LocalPlayer.CoreStats.Pollen.Value
        pollenpercentage = pollencount/maxpollen*100
        fieldselected = game:GetService("Workspace").FlowerZones[kometa.vars.field]
        if kometa.toggles.autodoquest then
            if game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.Menus.Children.Quests.Content:FindFirstChild("Frame") then
                for i,v in next, game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.Menus.Children.Quests:GetDescendants() do
                    if v.Name == "Description" then
                        if string.match(v.Parent.Parent.TitleBar.Text, kometa.vars.npcprefer) or kometa.vars.npcprefer == "All Quests" and not string.find(v.Text, "Puffshroom") then
                            pollentypes = {'White Pollen', "Red Pollen", "Blue Pollen", "Blue Flowers", "Red Flowers", "White Flowers"}
                            text = v.Text
                            if api.returnvalue(fieldstable, text) and not string.find(v.Text, "Complete!") and not api.findvalue(kometa.blacklistedfields, api.returnvalue(fieldstable, text)) then
                                d = api.returnvalue(fieldstable, text)
                                fieldselected = game:GetService("Workspace").FlowerZones[d]
                                break
                            elseif api.returnvalue(pollentypes, text) and not string.find(v.Text, 'Complete!') then
                                d = api.returnvalue(pollentypes, text)
                                if d == "Blue Flowers" or d == "Blue Pollen" then
                                    fieldselected = game:GetService("Workspace").FlowerZones[kometa.bestfields.blue]
                                    break
                                elseif d == "White Flowers" or d == "White Pollen" then
                                    fieldselected = game:GetService("Workspace").FlowerZones[kometa.bestfields.white]
                                    break
                                elseif d == "Red Flowers" or d == "Red Pollen" then
                                    fieldselected = game:GetService("Workspace").FlowerZones[kometa.bestfields.red]
                                    break
                                end
                            end
                        end
                    end
                end
            else
                api.notify('kometa | Auto Quest Error', 'You have autoquest enabled, but quests tab is closed. Please open it.')
            end
        else
            fieldselected = game:GetService("Workspace").FlowerZones[kometa.vars.field]
        end
        fieldpos = CFrame.new(fieldselected.Position.X, fieldselected.Position.Y+3, fieldselected.Position.Z)
        fieldposition = fieldselected.Position
        if temptable.sprouts.detected and temptable.sprouts.coords and kometa.toggles.farmsprouts then
            fieldposition = temptable.sprouts.coords.Position
            fieldpos = temptable.sprouts.coords
        end
        if kometa.toggles.farmpuffshrooms and game.Workspace.Happenings.Puffshrooms:FindFirstChildOfClass("Model") then 
            if api.partwithnamepart("Mythic", game.Workspace.Happenings.Puffshrooms) then
                temptable.magnitude = 25 
                fieldpos = api.partwithnamepart("Mythic", game.Workspace.Happenings.Puffshrooms):FindFirstChild("Puffball Stem").CFrame
                fieldposition = fieldpos.Position
            elseif api.partwithnamepart("Legendary", game.Workspace.Happenings.Puffshrooms) then
                temptable.magnitude = 25 
                fieldpos = api.partwithnamepart("Legendary", game.Workspace.Happenings.Puffshrooms):FindFirstChild("Puffball Stem").CFrame
                fieldposition = fieldpos.Position
            elseif api.partwithnamepart("Epic", game.Workspace.Happenings.Puffshrooms) then
                temptable.magnitude = 25 
                fieldpos = api.partwithnamepart("Epic", game.Workspace.Happenings.Puffshrooms):FindFirstChild("Puffball Stem").CFrame
                fieldposition = fieldpos.Position
            elseif api.partwithnamepart("Rare", game.Workspace.Happenings.Puffshrooms) then
                temptable.magnitude = 25 
                fieldpos = api.partwithnamepart("Rare", game.Workspace.Happenings.Puffshrooms):FindFirstChild("Puffball Stem").CFrame
                fieldposition = fieldpos.Position
            else
                temptable.magnitude = 25 
                fieldpos = api.getbiggestmodel(game.Workspace.Happenings.Puffshrooms):FindFirstChild("Puffball Stem").CFrame
                fieldposition = fieldpos.Position
            end
        end
        if tonumber(pollenpercentage) < tonumber(kometa.vars.convertat) or kometa.toggles.donotconvert then
            if not temptable.tokensfarm then
                api.tween(1, fieldpos)
                temptable.tokensfarm = true
                if kometa.toggles.autosprinkler then makesprinklers() end
            else
                if kometa.toggles.killmondo then
                    while kometa.toggles.killmondo and game.Workspace.Monsters:FindFirstChild("Mondo Chick (Lvl 8)") and not temptable.started.vicious and not temptable.started.monsters do
                        temptable.started.mondo = true
                        while game.Workspace.Monsters:FindFirstChild("Mondo Chick (Lvl 8)") do
                            disableall()
                            game:GetService("Workspace").Map.Ground.HighBlock.CanCollide = false 
                            temptable.MondoPosition = game.Workspace.Monsters["Mondo Chick (Lvl 8)"].Head.Position
                            api.tweenNoDelay(0.1, CFrame.new(temptable.MondoPosition.x, temptable.MondoPosition.y - 55, temptable.MondoPosition.z))
                            task.wait(1)
                            temptable.float = true
                        end
                        temptable.MondoCollectTokens = true
                        task.wait(.5) game:GetService("Workspace").Map.Ground.HighBlock.CanCollide = true temptable.float = false api.teleport(game.Workspace.FlowerZones['Mountain Top Field'].CFrame) task.wait(1) api.teleport(game.Workspace.FlowerZones['Mountain Top Field'].CFrame)       
                        for i = 0, 50 do 
                            gettoken(CFrame.new(73.2, 176.35, -167).Position) 
                        end 
                        temptable.MondoCollectTokens = false
                        enableall() 
                        api.tween(2, fieldpos) 
                        temptable.started.mondo = false
                    end
                end
                if (fieldposition-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude > temptable.magnitude then
                    api.teleport(fieldpos)
                    if kometa.toggles.autosprinkler then makesprinklers() end
                end
                getprioritytokens()
                if kometa.toggles.avoidmobs then avoidmob() end
                if kometa.toggles.farmclosestleaf then closestleaf() end
                if kometa.toggles.farmbubbles then getbubble() end
                if kometa.toggles.farmclouds then getcloud() end
                --if kometa.toggles.farmballoons then getballoons() end
                if not kometa.toggles.donotfarmtokens and done then getlinktoken() gettoken() end
                if not kometa.toggles.farmflower then getflower() end
            end
        elseif tonumber(pollenpercentage) >= tonumber(kometa.vars.convertat) then
            temptable.tokensfarm = false
            api.tween(1, game:GetService("Players").LocalPlayer.SpawnPos.Value * CFrame.fromEulerAnglesXYZ(0, 110, 0) + Vector3.new(0, 0, 9))
            temptable.converting = true
            repeat
                converthoney()
            until game.Players.LocalPlayer.CoreStats.Pollen.Value == 0
            if kometa.toggles.convertballoons and gethiveballoon() then
                task.wait(6)
                repeat
                    task.wait()
                    converthoney()
                until gethiveballoon() == false or not kometa.toggles.convertballoons
            end
            temptable.converting = false
            temptable.act = temptable.act + 1
            if kometa.webhooking.convert then api.imagehook(kometawebhook.webhook, "Total Honey: `"..game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.MeterHUD.HoneyMeter.Bar.TextLabel.Text.."`\nGained Honey: `"..api.suffixstring(temptable.honeycurrent - temptable.honeystart).."`\nElapsed Time: `"..api.toHMS(temptable.stats.runningfor).."`", "kometa в„пёЏ", "https://static.wikia.nocookie.net/bee-swarm-simulator/images/f/f6/HoneyDrop.png/revision/latest/scale-to-width-down/90?cb=20200521143648&path-prefix=ru") end
            task.wait(6)
            if kometa.toggles.autoant and not game:GetService("Workspace").Toys["Ant Challenge"].Busy.Value and rtsg().Eggs.AntPass > 0 then farmant() end
            if kometa.toggles.autoquest then makequests() end
            collectplanters()
            plantplanters()
            if kometa.toggles.autokillmobs then 
                if temptable.act >= kometa.vars.monstertimer then
                    temptable.started.monsters = true
                    temptable.act = 0
                    killmobs() 
                    temptable.started.monsters = false
                end
            end
        end
    end
end end end)

task.spawn(function()
    while task.wait(1) do
		if kometa.toggles.killvicious and temptable.detected.vicious and temptable.converting == false and not temptable.started.monsters then
            temptable.started.vicious = true
            disableall()
			local vichumanoid = game:GetService("Players").LocalPlayer.Character.HumanoidRootPart
			for i,v in next, game.workspace.Particles:GetChildren() do
				for x in string.gmatch(v.Name, "Vicious") do
					if string.find(v.Name, "Vicious") then
						api.tween(1,CFrame.new(v.Position.x, v.Position.y, v.Position.z)) task.wait(1)
						api.tween(0.5, CFrame.new(v.Position.x, v.Position.y, v.Position.z)) task.wait(.5)
					end
				end
			end
			for i,v in next, game.workspace.Particles:GetChildren() do
				for x in string.gmatch(v.Name, "Vicious") do
                    while kometa.toggles.killvicious and temptable.detected.vicious do task.wait() if string.find(v.Name, "Vicious") then
                        for i=1, 4 do temptable.float = true vichumanoid.CFrame = CFrame.new(v.Position.x+10, v.Position.y, v.Position.z) task.wait(.3)
                        end
                    end end
                end
			end
            enableall()
			task.wait(1)
			temptable.float = false
            temptable.started.vicious = false
            temptable.stats.killedvicious = temptable.stats.killedvicious + 1
		end
	end
end)

task.spawn(function() while task.wait() do
    if kometa.toggles.killwindy and temptable.detected.windy and not temptable.converting and not temptable.started.vicious and not temptable.started.mondo and not temptable.started.monsters then
        temptable.started.windy = true
        wlvl = "" aw = false awb = false -- some variable for autowindy, yk?
        disableall()
        while kometa.toggles.killwindy and temptable.detected.windy do
            if not aw then
                for i,v in pairs(workspace.Monsters:GetChildren()) do
                    if string.find(v.Name, "Windy") then wlvl = v.Name aw = true -- we found windy!
                    end
                end
            end
            if aw then
                for i,v in pairs(workspace.Monsters:GetChildren()) do
                    if string.find(v.Name, "Windy") then
                        if v.Name ~= wlvl then
                            temptable.float = false task.wait(5) for i =1, 5 do gettoken(api.humanoidrootpart().Position) end -- collect tokens :yessir:
                            wlvl = v.Name
                        end
                    end
                end
            end
            if not awb then api.tween(1,temptable.gacf(temptable.windy, 5)) task.wait(1) awb = true end
            if awb and temptable.windy.Name == "Windy" then
                api.humanoidrootpart().CFrame = temptable.gacf(temptable.windy, 25) temptable.float = true task.wait()
            end
        end 
        enableall()
        temptable.stats.killedwindy = temptable.stats.killedwindy + 1
        temptable.float = false
        temptable.started.windy = false
    end
end end)

task.spawn(function() while task.wait(0.001) do
    -- if kometa.toggles.traincrab then game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-259, 111.8, 496.4) * CFrame.fromEulerAnglesXYZ(0, 110, 90) temptable.float = true temptable.float = false end
    if kometa.toggles.autodig then 
        if game.Players.LocalPlayer and game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool") and game.Players.LocalPlayer.Character:FindFirstChildOfClass('Tool'):FindFirstChild('ClientScriptMouse') then
            pcall(function()getsenv(game.Players.LocalPlayer.Character:FindFirstChildOfClass('Tool').ClientScriptMouse).collectStart()end)
        end 
    end
end end)

game:GetService("Workspace").Particles.Folder2.ChildAdded:Connect(function(child)
    if child.Name == "Sprout" then
        temptable.sprouts.detected = true
        temptable.sprouts.coords = child.CFrame
    end
end)
game:GetService("Workspace").Particles.Folder2.ChildRemoved:Connect(function(child)
    if child.Name == "Sprout" then
        task.wait(30)
        temptable.sprouts.detected = false
        temptable.sprouts.coords = ""
    end
    if kometa.toggles.farmsprouts then temptable.stats.farmedsprouts = temptable.stats.farmedsprouts + 1 end
end)

Workspace.Particles.ChildAdded:Connect(function(instance)
    if string.find(instance.Name, "Vicious") then
        temptable.detected.vicious = true
        if kometa.webhooking.vicious then api.imagehook(kometawebhook.webhook, "Vicious Bee detected!", "kometa в„пёЏ", "https://static.wikia.nocookie.net/bee-swarm-simulator/images/1/1f/Vicious_Bee.png/revision/latest/scale-to-width-down/350?cb=20181130115012&path-prefix=ru") end
    end
end)
Workspace.Particles.ChildRemoved:Connect(function(instance)
    if string.find(instance.Name, "Vicious") then
        temptable.detected.vicious = false
    end
end)
game:GetService("Workspace").NPCBees.ChildAdded:Connect(function(v)
    if v.Name == "Windy" then
        task.wait(3) temptable.windy = v temptable.detected.windy = true
        if kometa.webhooking.windy then api.imagehook(kometawebhook.webhook, "Windy Bee detected!", "kometa в„пёЏ", "https://static.wikia.nocookie.net/bee-swarm-simulator/images/8/85/Windy_Bee.png/revision/latest?cb=20200404000105") end
    end
end)
game:GetService("Workspace").NPCBees.ChildRemoved:Connect(function(v)
    if v.Name == "Windy" then
        task.wait(3) temptable.windy = nil temptable.detected.windy = false
    end
end)

task.spawn(function() while task.wait(.1) do
    if not temptable.converting then
        if kometa.toggles.autodonate then
            game.ReplicatedStorage.Events.WindShrineDonation:InvokeServer(kometa.vars.donatit[1], kometa.vars.donatit[2])
            if not donotdonatedrop then game.ReplicatedStorage.Events.WindShrineTrigger:FireServer() end
            for i,v in pairs(game.Workspace.Collectibles:GetChildren()) do
                if (v.Position-Vector3.new(-484, 142, 413)).magnitude < 35 and v.CFrame.YVector.Y == 1 then
                    api.humanoidrootpart().CFrame = v.CFrame
                end
            end
        end
        if kometa.toggles.autosamovar then
            platformm = game:GetService("Workspace").Toys.Samovar.Platform
            for i,v in pairs(game.Workspace.Collectibles:GetChildren()) do
                if (v.Position-platformm.Position).magnitude < 25 and v.CFrame.YVector.Y == 1 then
                    api.humanoidrootpart().CFrame = v.CFrame
                end
            end
        end
        if kometa.toggles.autostockings then
            platformm = game:GetService("Workspace").Toys.Stockings.Platform
            for i,v in pairs(game.Workspace.Collectibles:GetChildren()) do
                if (v.Position-platformm.Position).magnitude < 25 and v.CFrame.YVector.Y == 1 then
                    api.humanoidrootpart().CFrame = v.CFrame
                end
            end
        end
        if kometa.toggles.autoonettart then
            platformm = game:GetService("Workspace").Toys["Onett's Lid Art"].Platform
            for i,v in pairs(game.Workspace.Collectibles:GetChildren()) do
                if (v.Position-platformm.Position).magnitude < 25 and v.CFrame.YVector.Y == 1 then
                    api.humanoidrootpart().CFrame = v.CFrame
                end
            end
        end
        if kometa.toggles.autocandles then
            platformm = game:GetService("Workspace").Toys["Honeyday Candles"].Platform
            for i,v in pairs(game.Workspace.Collectibles:GetChildren()) do
                if (v.Position-platformm.Position).magnitude < 25 and v.CFrame.YVector.Y == 1 then
                    api.humanoidrootpart().CFrame = v.CFrame
                end
            end
        end
        if kometa.toggles.autofeast then
            platformm = game:GetService("Workspace").Toys["Beesmas Feast"].Platform
            for i,v in pairs(game.Workspace.Collectibles:GetChildren()) do
                if (v.Position-platformm.Position).magnitude < 25 and v.CFrame.YVector.Y == 1 then
                    api.humanoidrootpart().CFrame = v.CFrame
                end
            end
        end
    end
end end)

task.spawn(function() while task.wait(1) do
    temptable.stats.runningfor = temptable.stats.runningfor + 1
    temptable.stats.sincelasthoneymaking = temptable.stats.sincelasthoneymaking + 1
    temptable.honeycurrent = statsget().Totals.Honey
    -- if kometa.toggles.honeystorm then game.ReplicatedStorage.Events.ToyEvent:FireServer("Honeystorm") end
    -- if kometa.toggles.autospawnsprout then game.ReplicatedStorage.Events.ToyEvent:FireServer("Sprout Summoner") end
    -- if kometa.toggles.collectgingerbreads then game:GetService("ReplicatedStorage").Events.ToyEvent:FireServer("Gingerbread House") end
    -- if kometa.toggles.autodispense then
    --     if kometa.dispensesettings.rj then local A_1 = "Free Royal Jelly Dispenser" local Event = game:GetService("ReplicatedStorage").Events.ToyEvent Event:FireServer(A_1) end
    --     if kometa.dispensesettings.blub then game:GetService("ReplicatedStorage").Events.ToyEvent:FireServer("Blueberry Dispenser") end
    --     if kometa.dispensesettings.straw then game:GetService("ReplicatedStorage").Events.ToyEvent:FireServer("Strawberry Dispenser") end
    --     if kometa.dispensesettings.treat then game:GetService("ReplicatedStorage").Events.ToyEvent:FireServer("Treat Dispenser") end
    --     if kometa.dispensesettings.coconut then game:GetService("ReplicatedStorage").Events.ToyEvent:FireServer("Coconut Dispenser") end
    --     if kometa.dispensesettings.glue then game:GetService("ReplicatedStorage").Events.ToyEvent:FireServer("Glue Dispenser") end
    -- end
    -- if kometa.toggles.autoboosters then 
    --     if kometa.dispensesettings.white then game.ReplicatedStorage.Events.ToyEvent:FireServer("Field Booster") end
    --     if kometa.dispensesettings.red then game.ReplicatedStorage.Events.ToyEvent:FireServer("Red Field Booster") end
    --     if kometa.dispensesettings.blue then game.ReplicatedStorage.Events.ToyEvent:FireServer("Blue Field Booster") end
    -- end
    -- if kometa.toggles.clock then game:GetService("ReplicatedStorage").Events.ToyEvent:FireServer("Wealth Clock") end
    -- if kometa.toggles.freeantpass then game:GetService("ReplicatedStorage").Events.ToyEvent:FireServer("Free Ant Pass Dispenser") end
    -- if kometa.toggles.freerobopass then game:GetService("ReplicatedStorage").Events.ToyEvent:FireServer("Free Robo Pass Dispenser") end
    game.CoreGui.kometaUI.Container.Categories.Statistics:FindFirstChild("Statistics", true).Container["Gained Honey"].Title.Text = "Gained Honey: "..api.suffixstring(temptable.honeycurrent - temptable.honeystart)
    game.CoreGui.kometaUI.Container.Categories.Statistics:FindFirstChild("Statistics", true).Container["Honey Per Hour"].Title.Text = "Honey Per Hour: "..api.suffixstring((temptable.honeycurrent - temptable.honeystart)/(temptable.stats.runningfor/3600))
    game.CoreGui.kometaUI.Container.Categories.Statistics:FindFirstChild("Statistics", true).Container["Elapsed Time"].Title.Text = "Elapsed Time: "..api.toHMS(temptable.stats.runningfor)
    game.CoreGui.kometaUI.Container.Categories.Statistics:FindFirstChild("Statistics", true).Container["Farmed Sprouts"].Title.Text = "Farmed Sprouts: "..temptable.stats.farmedsprouts
    game.CoreGui.kometaUI.Container.Categories.Statistics:FindFirstChild("Statistics", true).Container["Killed Windy"].Title.Text = "Killed Windy: "..temptable.stats.killedwindy
    game.CoreGui.kometaUI.Container.Categories.Statistics:FindFirstChild("Statistics", true).Container["Killed Vicious"].Title.Text = "Killed Vicious: "..temptable.stats.killedvicious
    game.CoreGui.kometaUI.Container.Categories.Statistics:FindFirstChild("Statistics", true).Container["Farmed Ants"].Title.Text = "Farmed Ants: "..temptable.stats.farmedants
    if kometa.toggles.visualnight then
        game:GetService("Lighting").TimeOfDay = '00:00:00'
        game:GetService("Lighting").Brightness = 0.03
        game:GetService("Lighting").ClockTime = 0
    end
end end)

game:GetService('RunService').Heartbeat:connect(function() 
    if kometa.toggles.autoquest then firesignal(game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.NPC.ButtonOverlay.MouseButton1Click) end
    if kometa.toggles.loopspeed then game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = kometa.vars.walkspeed end
    if kometa.toggles.loopjump then game.Players.LocalPlayer.Character.Humanoid.JumpPower = kometa.vars.jumppower end
end)

game:GetService('RunService').Heartbeat:connect(function()
    for i,v in next, game.Players.LocalPlayer.PlayerGui.ScreenGui:WaitForChild("MinigameLayer"):GetChildren() do for k,q in next, v:WaitForChild("GuiGrid"):GetDescendants() do if q.Name == "ObjContent" or q.Name == "ObjImage" then q.Visible = true end end end
end)

game:GetService('RunService').Heartbeat:connect(function() 
    if temptable.float then 
        if not setfflag then
            game.Players.LocalPlayer.Character.Humanoid.BodyTypeScale.Value = 0 
            floatpad.CanCollide = true 
            floatpad.CFrame = CFrame.new(game.Players.LocalPlayer.Character.HumanoidRootPart.Position.X, game.Players.LocalPlayer.Character.HumanoidRootPart.Position.Y-3.75, game.Players.LocalPlayer.Character.HumanoidRootPart.Position.Z) task.wait(0)  
        else
            api.humanoid():ChangeState(11)
        end
    else 
        if not setfflag then floatpad.CanCollide = false end
    end
end)

local vu = game:GetService("VirtualUser")
game:GetService("Players").LocalPlayer.Idled:connect(function() vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)task.wait(1)vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
end)

game.Players.PlayerAdded:Connect(function(player)
    if kometa.webhooking.playeradded then api.imagehook(kometawebhook.webhook, player.Name.." joined your server", "kometa в„пёЏ", "https://www.roblox.com/headshot-thumbnail/image?userId="..player.UserId.."&width=420&height=420&format=png") end
end)

game.Workspace.Collectibles.ChildAdded:Connect(function(token)
    if kometa.toggles.farmtickets and temptable.collecting.tickets == false then farmtickets(token) end
    if kometa.toggles.farmrares and temptable.collecting.rares == false then farmrares(token) end
    if kometa.toggles.traincrab then farmcombattokens(token, CFrame.new(-256, 110, 475), 'crab') end
    if kometa.toggles.trainsnail then farmcombattokens(token, CFrame.new(game.Workspace.FlowerZones['Stump Field'].Position.X, game.Workspace.FlowerZones['Stump Field'].Position.Y-10, game.Workspace.FlowerZones['Stump Field'].Position.Z), 'snail') end
    if kometa.toggles.killmondo and not temptable.MondoCollectTokens and temptable.MondoPosition then farmcombattokens(token, CFrame.new(temptable.MondoPosition.x, temptable.MondoPosition.y - 55, temptable.MondoPosition.z), 'mondo') end
    -- if kometa.toggles.farmsnowflakes and temptable.collecting.snowflake == false then farmsnowflakes(token) end
end)

game.Players.LocalPlayer.CharacterAdded:Connect(function(char)
    humanoid = char:WaitForChild("Humanoid")
    humanoid.Died:Connect(function()
        if kometa.toggles.autofarm then
            temptable.dead = true
            kometa.toggles.autofarm = false
            temptable.converting = false
            temptable.farmtoken = false
        end
        if temptable.dead then
            task.wait(25)
            temptable.dead = false
            kometa.toggles.autofarm = true local player = game.Players.LocalPlayer
            temptable.converting = false
            temptable.tokensfarm = true
        end
    end)
end)

game:GetService("Workspace").Particles:FindFirstChild('PopStars').ChildAdded:Connect(function(v)
    task.wait(.1)
    if (v.Position-api.humanoidrootpart().Position).magnitude < 15 then
        temptable.foundpopstar = true
    end
end)

game:GetService("Workspace").Particles:FindFirstChild('PopStars').ChildRemoved:Connect(function(v)
    if (v.Position-api.humanoidrootpart().Position).magnitude < 15 then
        temptable.foundpopstar = false
        temptable.float = false
    end
end)

for _,v in next, game.Workspace.Collectibles:GetChildren() do
    if string.find(v.Name,"") then
        v:Destroy()
    end
end 

task.spawn(function() while task.wait() do
    pos = game.Players.LocalPlayer.Character.HumanoidRootPart.Position
    task.wait(0.00001)
    currentSpeed = (pos-game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude
    if currentSpeed > 0 then
        temptable.running = true
    else
        temptable.running = false
    end
end end)

task.spawn(function() while task.wait(15*60+10) do 
    if kometa.AutoUseSettings['Glitter'] and findFieldWithRay(api.humanoidrootpart().Position, Vector3.new(0, -90, 0)) then
        game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = "Glitter"})
    end
    if kometa.AutoUseSettings['Purple Potion'] then
        game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = "Purple Potion"})
    end
end end)

task.spawn(function() while task.wait(10*60+10) do
    if kometa.AutoUseSettings['Blue Extract'] then
        game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = "Blue Extract"})
    end
    if kometa.AutoUseSettings['Red Extract'] then
        game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = "Red Extract"})
    end
    if kometa.AutoUseSettings['Glue'] then
        game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = "Glue"})
    end
    if kometa.AutoUseSettings['Oil'] then
        game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = "Oil"})
    end
    if kometa.AutoUseSettings['Enzymes'] then
        game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = "Enzymes"})
    end
    if kometa.AutoUseSettings['Tropical Drink'] then
        game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = "Tropical Drink"})
    end
end end)

task.spawn(function() while task.wait(20*60+10) do
    if kometa.AutoUseSettings['Super Smoothie'] then
        game:GetService("ReplicatedStorage").Events.PlayerActivesCommand:FireServer({["Name"] = "Super Smoothie"})
    end
end end)

task.spawn(function() while task.wait(.00000000000000001) do
    if kometa.beessettings.usbtoggle then
        if not game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.BeePopUp.TypeName.Text:match(kometa.beessettings.usb) then
            temptable.feed(kometa.beessettings.general.x, kometa.beessettings.general.y, "RoyalJelly")
        end
    end
    if kometa.beessettings.ugb then
        if not game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.BeePopUp.TypeName.Text:match("Gifted") then
            temptable.feed(kometa.beessettings.general.x, kometa.beessettings.general.y, kometa.beessettings.foodtype)
        end
    end
    if kometa.beessettings.af then
        temptable.feed(kometa.beessettings.general.x, kometa.beessettings.general.y, kometa.beessettings.foodtype, kometa.beessettings.general.amount)
    end
    if kometa.beessettings.umb then
        if not game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.BeePopUp.MutationFrame.MutationLabel.Text:match(kometa.beessettings.mutation) then
            temptable.feed(kometa.beessettings.general.x, kometa.beessettings.general.y, "Bitterberry", kometa.beessettings.general.amount)
        end
    end
end end)

-- Rendering game

game:GetService("UserInputService").WindowFocused:Connect(function()
	if kometa.toggles.disablerender then
        game:GetService("RunService"):Set3dRenderingEnabled(true)
    end
end)

game:GetService("UserInputService").WindowFocusReleased:Connect(function()
	if kometa.toggles.disablerender then
        game:GetService("RunService"):Set3dRenderingEnabled(false)
    end
end)

for _, Gate in next, workspace.Gates:GetDescendants() do 
    if Gate:IsA("BasePart") and string.find(Gate.parent.Name, "Bee Gate") then 
        Gate.CanCollide = false 
        Gate.Transparency = Gate.Transparency < 0.5 and 0.5 or Gate.Transparency
        task.wait() 
    end 
end

for _, Part in next, workspace:FindFirstChild("FieldDecos"):GetDescendants() do 
    if Part:IsA("BasePart") then 
        Part.CanCollide = false 
        Part.Transparency = Part.Transparency < 0.5 and 0.5 or Part.Transparency 
        task.wait() 
    end 
end

for _, Part in next, workspace:FindFirstChild("Decorations"):GetDescendants() do 
    if Part:IsA("BasePart") and (Part.Parent.Name == "Bush" or Part.Parent.Name == "Blue Flower") then 
        Part.CanCollide = false 
        Part.Transparency = Part.Transparency < 0.5 and 0.5 or Part.Transparency 
        task.wait() 
    end 
end

for _, Part in next, workspace.Decorations.Misc:GetDescendants() do 
    if Part.Parent.Name == "Mushroom" then 
        Part.CanCollide = false 
        Part.Transparency = 0.5 
    end 
end

setfflag("HumanoidParallelRemoveNoPhysics", "False")setfflag("HumanoidParallelRemoveNoPhysicsNoSimulate2", "False")
hives = game.Workspace.Honeycombs:GetChildren() for i = #hives, 1, -1 do  v = game.Workspace.Honeycombs:GetChildren()[i] if v.Owner.Value == nil then game.ReplicatedStorage.Events.ClaimHive:FireServer(v.HiveID.Value) end end
if _G.autoload then if isfile("kometa/BSS_".._G.autoload..".json") then kometa = game:service'HttpService':JSONDecode(readfile("kometa/BSS_".._G.autoload..".json")) end end
game:GetService("Players").LocalPlayer.PlayerGui.ScreenGui.BeePopUp.MutationFrame.MutationLabel.Text = ""
if table.find(temptable.bestblacklistever, game.Players.LocalPlayer.UserId) then game.Players.LocalPlayer:Kick("рџ¤“") end
