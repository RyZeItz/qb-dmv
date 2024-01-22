# DMVSchool
Players Can Take a Theoritical test and get their Permit, they can then take 1 Driving Test each for: Personal Vehicles, Bikes/Motorcycles, and CDL License. Depending on how you want to set it up you can make it so when the player takes the Motorcycle Test it just adds an endorsement to the players Drivers License or you can give the player a Bike License to carry around. Each Type of driving test can have a unique route along with multiple locations for a DMV Ped.

<br />

# Driving School MLO
https://forum.cfx.re/t/mlo-driving-school-interior/1466079

**DISCLAIMER: This is not my MLO OR SCRIPT. I just found both for free online and saw it wasnt updated to the new qb so thats why i am here :)   Original Creator: https://github.com/bamablood94/qb-dmv/tree/main?tab=readme-ov-file**

<br />

# Installation:
<br />

## QB-CORE
<br />

> ## QB-Core/Shared/Items.lua:
```
    permit						 = {name = 'permit',						label = 'Driving Permit',			weight = 0,			type = 'item',		image = 'id_card.png',				unique = true,useable = true,		shouldClose = false,   combinable = nil,   description = 'A Driving permit to show you can drive a vehicle as long as you have a passenger'},
    cdl_license				 = { name  = 'cdl_license',					label = 'Commercial License',				weight = 0,			type = 'item',		image = 'driver_license.png',		unique = true,		useable = true,		shouldClose = false,   combinable = nil,   description = 'Permit to show you can drive a Commercial Vehicle.'},
    bike_license			 = { name = 'bike_license',					label = 'Bike License',				weight = 0,			type = 'item',		image = 'driver_license.png',		unique = true,		useable = true,		shouldClose = false,	combinable = nil,	description = 'Permit to show you can drive a Motorcycle/ATV'},
    driver_license               = { name = 'driver_license', label = 'Drivers License', weight = 0, type = 'item', image = 'driver_license.png', unique = true, useable = true, shouldClose = false, combinable = nil, description = 'Permit to show you can drive a car' },
```

<br />

> ## QB-Core/Server/Players.lua
**FIND:**
```
    PlayerData.metadata['licences'] = PlayerData.metadata['licences'] or {
		['driver'] = false,
        ['business'] = false,
        ['weapon'] = false
    }
```
**AND REPLACE WITH:**
```
    PlayerData.metadata['licences'] = PlayerData.metadata['licences'] or {
        ['permit'] = false,
        ['driver'] = false,
        ['cdl'] = false,
        ['bike'] = false,
        ['business'] = false,
        ['weapon'] = false
    }
```
<br />

## QB-CITYHALL
<br />

> ## QB-Cityhall/server/main.lua
**FIND:**
```
RegisterNetEvent('qb-cityhall:server:requestId', function(item, hall)
    local src = source
    local Player = QBCore.Functions.GetPlayer(src)
    if not Player then return end
    local itemInfo = Config.Cityhalls[hall].licenses[item]
    if not Player.Functions.RemoveMoney('cash', itemInfo.cost, 'cityhall id') then return TriggerClientEvent('QBCore:Notify', src, ('You don\'t have enough money on you, you need %s cash'):format(itemInfo.cost), 'error') end
    local info = {}
    if item == 'id_card' then
        info.citizenid = Player.PlayerData.citizenid
        info.firstname = Player.PlayerData.charinfo.firstname
        info.lastname = Player.PlayerData.charinfo.lastname
        info.birthdate = Player.PlayerData.charinfo.birthdate
        info.gender = Player.PlayerData.charinfo.gender
        info.nationality = Player.PlayerData.charinfo.nationality
    elseif item == 'driver_license' then
        info.firstname = Player.PlayerData.charinfo.firstname
        info.lastname = Player.PlayerData.charinfo.lastname
        info.birthdate = Player.PlayerData.charinfo.birthdate
        info.type = 'Class C Driver License'
    elseif item == 'weaponlicense' then
        info.firstname = Player.PlayerData.charinfo.firstname
        info.lastname = Player.PlayerData.charinfo.lastname
        info.birthdate = Player.PlayerData.charinfo.birthdate
    else
        return false -- DropPlayer(src, 'Attempted exploit abuse')
    end
    if not Player.Functions.AddItem(item, 1, nil, info) then return end
    TriggerClientEvent('inventory:client:ItemBox', src, QBCore.Shared.Items[item], 'add')
end)
```
**AND REPLACE WITH:**
```
RegisterNetEvent('qb-cityhall:server:requestId', function(item, hall)
    local src = source
    local Player = QBCore.Functions.GetPlayer(src)
    if not Player then return end
    local itemInfo = Config.Cityhalls[hall].licenses[item]
    if not Player.Functions.RemoveMoney("cash", itemInfo.cost) then return TriggerClientEvent('QBCore:Notify', src, ('You don\'t have enough money on you, you need %s cash'):format(itemInfo.cost), 'error') end
    local info = {}
    if item == "id_card" then
        info.citizenid = Player.PlayerData.citizenid
        info.firstname = Player.PlayerData.charinfo.firstname
        info.lastname = Player.PlayerData.charinfo.lastname
        info.birthdate = Player.PlayerData.charinfo.birthdate
        info.gender = Player.PlayerData.charinfo.gender
        info.nationality = Player.PlayerData.charinfo.nationality
    elseif item == "driver_license" then
        info.firstname = Player.PlayerData.charinfo.firstname
        info.lastname = Player.PlayerData.charinfo.lastname
        info.birthdate = Player.PlayerData.charinfo.birthdate
        info.type = "Class R Driver License"
        if Player.PlayerData.metadata['licences']['bike'] then
            info.endorsement = 'Motorcycle Endorsement'
        else
            info.endorsement = 'None'
        end
    elseif item == 'cdl_license' then
        info.firstname = Player.PlayerData.charinfo.firstname
        info.lastname = Player.PlayerData.charinfo.lastname
        info.birthdate = Player.PlayerData.charinfo.birthdate
        info.type = "Class A Driver License"
    elseif item == 'bike_license' then
        info.firstname = Player.PlayerData.charinfo.firstname
        info.lastname = Player.PlayerData.charinfo.lastname
        info.birthdate = Player.PlayerData.charinfo.birthdate
    elseif item == 'permit' then
        info.firstname = Player.PlayerData.charinfo.firstname
        info.lastname = Player.PlayerData.charinfo.lastname
        info.birthdate = Player.PlayerData.charinfo.birthdate
        info.type = "Class R Driver License"
    elseif item == "weaponlicense" then
        info.firstname = Player.PlayerData.charinfo.firstname
        info.lastname = Player.PlayerData.charinfo.lastname
        info.birthdate = Player.PlayerData.charinfo.birthdate
    else
        return DropPlayer(src, 'Attempted exploit abuse')
    end
    if not Player.Functions.AddItem(item, 1, nil, info) then return end
    TriggerClientEvent('inventory:client:ItemBox', src, QBCore.Shared.Items[item], 'add')
end)
```

> ## QB-Cityhall/config.lua
**FIND:**
```
Config.Cityhalls = {
    { -- Cityhall 1
        coords = vec3(-265.0, -963.6, 31.2),
        showBlip = true,
        blipData = {
            sprite = 487,
            display = 4,
            scale = 0.65,
            colour = 0,
            title = 'City Services'
        },
        licenses = {
            ['id_card'] = {
                label = 'ID Card',
                cost = 50,
            },
            ['driver_license'] = {
                label = 'Driver License',
                cost = 50,
                metadata = 'driver'
            },
            ['weaponlicense'] = {
                label = 'Weapon License',
                cost = 50,
                metadata = 'weapon'
            },
        }
    },
}
```
**AND REPLACE WITH:**
```
Config.Cityhalls = {
    { -- Cityhall 1
        coords = vec3(-265.0, -963.6, 31.2),
        showBlip = true,
        blipData = {
            sprite = 487,
            display = 4,
            scale = 0.65,
            colour = 0,
            title = "City Services"
        },
        licenses = {
            ["id_card"] = {
                label = "ID Card",
                cost = 50,
            },
            ["permit"] = {
                label = 'Permit',
                cost = 25,
                metadata = 'permit',
            },
            ["cdl"] = {
                label = 'CDL',
                cost = 75,
                metadata = 'cdl',
            },
            ["bike"] = {
                label = 'Bike License',
                cost = 50,
                metadata = 'bike',
            },
            ["driver_license"] = {
                label = "Driver License",
                cost = 50,
                metadata = "driver"
            },
            ["weaponlicense"] = {
                label = "Weapon License",
                cost = 50,
                metadata = "weapon"
            },
        }
    },
}
```

<br />

## QB-Cityhall/config.lua
**FIND AND REMOVE**
```
Config.DrivingSchools = {
    { -- Driving School 1
        coords = vec3(240.3, -1379.89, 33.74),
        showBlip = true,
        blipData = {
            sprite = 225,
            display = 4,
            scale = 0.65,
            colour = 3,
            title = 'Driving School'
        },
        instructors = {
            'DJD56142',
            'DXT09752',
            'SRI85140',
        }
    },
}
```

<br />

## QB-Cityhall/config.lua
**FIND AND REMOVE**
```
    {
        model = 'a_m_m_eastsa_02',
        coords = vec4(240.91, -1379.2, 32.74, 138.96),
        scenario = 'WORLD_HUMAN_STAND_MOBILE',
        drivingschool = true,
        zoneOptions = { -- Used for when UseTarget is false
            length = 3.0,
            width = 3.0
        }
    }
```

## Inventory
If you use `qb-inventory` or `lj-inventory` go to your-inventory/html/js/app.js and find **`switch (itemData.name) {`** **`case "id_card":`** and add the following in between the two:
```
        case "cdl_license":
            return `<p><strong>First Name: </strong><span>'${itemData.info.firstname}'</span></p>
                    <p><strong>Last Name: </strong><span>${itemData.info.lastname}</span></p>
                    <p><strong>Birth Date: </strong><span>${itemData.info.lastname$}</span>
                    </p><p><strong>Licenses: </strong><span>${item.Data.info.type}</span></p>`;
        case "permit":
            return `<p><strong>First Name: </strong><span>'${itemData.info.firstname}'</span></p>
                    <p><strong>Last Name: </strong><span>${itemData.info.lastname}</span></p>
                    <p><strong>Birth Date: </strong><span>${itemData.info.lastname$}</span>
                    </p><p><strong>Licenses: </strong><span>${item.Data.info.type}</span></p>`;
        case "bike_license":
            return `<p><strong>First Name: </strong><span>'${itemData.info.firstname}'</span></p>
                    <p><strong>Last Name: </strong><span>${itemData.info.lastname}</span></p>
                    <p><strong>Birth Date: </strong><span>${itemData.info.lastname$}</span>
                    </p><p><strong>Licenses: </strong><span>${item.Data.info.type}</span></p>`;
```
If you don't want the bike license and instead want a `Motorcycle Endorsemeonet` on your Driver License then replace the `case "driver_license":` line with this one:
```
        case "driver_license":
            return `<p><strong>First Name: </strong><span>${itemData.info.firstname}</span></p>
            <p><strong>Last Name: </strong><span>${itemData.info.lastname}</span></p>
            <p><strong>Birth Date: </strong><span>${itemData.info.birthdate}</span>
            </p><p><strong>Licenses: </strong><span>${itemData.info.type}</span></p>
            </p><p><strong>Endorsements: </strong><span>${itemData.info.endorsement}</span></p>`;
```

> ## Qb-Cityhall/server/main.lua
Find the **`local function giveStarterItems()`** Function and add:
```
elseif itemData["name"] == "permit" then
					info.firstname = Player.PlayerData.charinfo.firstname
					info.lastname = Player.PlayerData.charinfo.lastname
					info.birthdate = Player.PlayerData.charinfo.birthdate
				elseif itemData["name"] == "cdl_license" then
					info.firstname = Player.PlayerData.charinfo.firstname
					info.lastname = Player.PlayerData.charinfo.lastname
					info.birthdate = Player.PlayerData.charinfo.birthdate
				elseif itemData["name"] == "bike_license" then
					info.firstname = Player.PlayerData.charinfo.firstname
					info.lastname = Player.PlayerData.charinfo.lastname
					info.birthdate = Player.PlayerData.charinfo.birthdate
```

<br />

If you want the endorsement for the drivers license then remove the bike_license line and instead add the following to the drivers license line:
```
if Player.PlayerData.metadata['licences']['bike'] then
						info.endorsement = 'Motorcycle Endorsement'
					else
						info.endorsement = 'None'
					end
```

<br />


That should be all for the installation. Now just start the server up and enjoy!

# Contact Me

If you have any questions or any problems please don't hesitate to message me _._ryze_._
