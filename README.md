# DMVSchool
Players Can Take a Theoritical test and get their Permit, they can then take 1 Driving Test each for: Personal Vehicles, Bikes/Motorcycles, and CDL License. Depending on how you want to set it up you can make it so when the player takes the Motorcycle Test it just adds an endorsement to the players Drivers License or you can give the player a Bike License to carry around. Each Type of driving test can have a unique route along with multiple locations for a DMV Ped.

<br />

# Driving School MLO
https://forum.cfx.re/t/mlo-driving-school-interior/1466079

**DISCLAIMER: This is not my MLO OR SCRIPT. I just found both for free online and saw it wasnt updated to the new qb so thats why i am here :)   Original Creator: https://github.com/bamablood94/qb-dmv/tree/main?tab=readme-ov-file**

<br />

# Installation:
<br />

# Dependencies:
- QB-Core (https://github.com/qbcore-framework/qb-core)
- QB-Cityhall (https://github.com/qbcore-framework/qb-cityhall)
- QB-Inventory (https://github.com/qbcore-framework/qb-inventory)
- QB-Policejob (https://github.com/qbcore-framework/qb-policejob)
- QS-Inventory (https://buy.quasar-store.com/package/5677336#packageName)
- Lj-Inventory (https://github.com/loljoshie/lj-inventory)
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
**FIND `PlayerData.metadata['licences'] = PlayerData.metadata['licences'] or {` AND REPLACE WITH:**
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
**FIND `RegisterNetEvent('qb-cityhall:server:requestId', function(item, hall)` AND REPLACE WITH:**
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
**FIND `Config.Cityhalls = {` AND REPLACE WITH:**
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
            ["cdl_license"] = {
                label = 'CDL',
                cost = 75,
                metadata = 'cdl',
            },
            ["bike_license"] = {
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

> ## QB-Cityhall/config.lua
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

> ## QB-Cityhall/config.lua
**FIND AND CHANGE `drivingschool = true,` TO `drivingschool = false,`**

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

If you want the endorsement for the drivers license then remove the bike_license line and instead add the following to the drivers license line:
```
if Player.PlayerData.metadata['licences']['bike'] then
						info.endorsement = 'Motorcycle Endorsement'
					else
						info.endorsement = 'None'
					end
```

<br />

## QB-Inventory
If you use `qb-inventory` go to your-inventory/html/js/app.js and find **`switch (itemData.name) {`** and **`case "id_card":`** and add the following in between the two lines (example below):
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
![image](https://github.com/RyZeItz/qb-dmv/assets/103600006/345ae786-851a-4b1e-a08a-379f3ba94c7b)

If you don't want the bike license and instead want a `Motorcycle Endorsemeonet` on your Driver License then replace the `case "driver_license":` section with this one:
```
        case "driver_license":
            return `<p><strong>First Name: </strong><span>${itemData.info.firstname}</span></p>
            <p><strong>Last Name: </strong><span>${itemData.info.lastname}</span></p>
            <p><strong>Birth Date: </strong><span>${itemData.info.birthdate}</span>
            </p><p><strong>Licenses: </strong><span>${itemData.info.type}</span></p>
            </p><p><strong>Endorsements: </strong><span>${itemData.info.endorsement}</span></p>`;
```


<br />

## QS-Inventory
If you use `qs-inventory` go to your-inventory/config/metadata.js and find **`if (itemData.name == "id_card") {`** and **`else if (itemData.name == "driver_license") {`** and add the following in between these two sections (example in picture below):
```
else if (itemData.name == "cdl_license") {
            $(".item-info-title").html("<p>" + itemData.label + "</p>");
            $(".item-info-description").html(
                "<p><strong>First Name: </strong><span>" +
                itemData.info.firstname +
                "</span></p><p><strong>Last Name: </strong><span>" +
                itemData.info.lastname +
                "</span></p><p><strong>Birth Date: </strong><span>" +
                itemData.info.birthdate +
                "</span></p><p><strong>Licenses: </strong><span>" +
                itemData.info.type +
                "</span></p>"
            );
        } else if (itemData.name == "permit") {
            $(".item-info-title").html("<p>" + itemData.label + "</p>");
            $(".item-info-description").html(
                "<p><strong>First Name: </strong><span>" +
                itemData.info.firstname +
                "</span></p><p><strong>Last Name: </strong><span>" +
                itemData.info.lastname +
                "</span></p><p><strong>Birth Date: </strong><span>" +
                itemData.info.birthdate +
                "</span></p>"
            );
        } else if (itemData.name == "bike_license") {
            $(".item-info-title").html("<p>" + itemData.label + "</p>");
            $(".item-info-description").html(
                "<p><strong>First Name: </strong><span>" +
                itemData.info.firstname +
                "</span></p><p><strong>Last Name: </strong><span>" +
                itemData.info.lastname +
                "</span></p><p><strong>Birth Date: </strong><span>" +
                itemData.info.birthdate +
                "</span></p>"
            );
        }
```

![image](https://github.com/RyZeItz/qb-dmv/assets/103600006/a2a6bb0f-637e-4574-82c9-8c41e40d8d66)

If you don't want the bike license and instead want a `Motorcycle Endorsemeonet` on your Driver License then replace the `else if (itemData.name == "driver_license") {` section with this one:
```
else if (itemData.name == "driver_license") {
            $(".item-info-title").html("<p>" + itemData.label + "</p>");
            $(".item-info-description").html(
                "<p><strong>First Name: </strong><span>" +
                itemData.info.firstname +
                "</span></p><p><strong>Last Name: </strong><span>" +
                itemData.info.lastname +
                "</span></p><p><strong>Birth Date: </strong><span>" +
                itemData.info.birthdate +
                "</span></p><p><strong>Licenses: </strong><span>" +
                itemData.info.type +
                "<p><strong>Endorsements: </strong><span>" +
                itemData.info.endorsement +
                "</span></p>"

            );
        }
```

## LJ-Inventory
If you use `lj-inventory` go to your-inventory/config/metadata.js and find **`    if (itemData != null && itemData.info != "") {`** and **`else if (itemData.name == "driver_license") {`** and add the following in between these two sections (example in picture below):
```
else if (itemData.name == "cdl_license") {
            $(".item-info-title").html("<p>" + itemData.label + "</p>");
            $(".item-info-description").html(
                "<p><strong>First Name: </strong><span>" +
                itemData.info.firstname +
                "</span></p><p><strong>Last Name: </strong><span>" +
                itemData.info.lastname +
                "</span></p><p><strong>Birth Date: </strong><span>" +
                itemData.info.birthdate +
                "</span></p><p><strong>Licenses: </strong><span>" +
                itemData.info.type +
                "</span></p>"
            );
        } else if (itemData.name == "permit") {
            $(".item-info-title").html("<p>" + itemData.label + "</p>");
            $(".item-info-description").html(
                "<p><strong>First Name: </strong><span>" +
                itemData.info.firstname +
                "</span></p><p><strong>Last Name: </strong><span>" +
                itemData.info.lastname +
                "</span></p><p><strong>Birth Date: </strong><span>" +
                itemData.info.birthdate +
                "</span></p>"
            );
        } else if (itemData.name == "bike_license") {
            $(".item-info-title").html("<p>" + itemData.label + "</p>");
            $(".item-info-description").html(
                "<p><strong>First Name: </strong><span>" +
                itemData.info.firstname +
                "</span></p><p><strong>Last Name: </strong><span>" +
                itemData.info.lastname +
                "</span></p><p><strong>Birth Date: </strong><span>" +
                itemData.info.birthdate +
                "</span></p>"
            );
        }
```

![image](https://github.com/RyZeItz/qb-dmv/assets/103600006/2d869d49-4f90-4d80-9cda-2775891f9588)

If you don't want the bike license and instead want a `Motorcycle Endorsemeonet` on your Driver License then replace the `else if (itemData.name == "driver_license") {` section with this one:
```
else if (itemData.name == "driver_license") {
            $(".item-info-title").html("<p>" + itemData.label + "</p>");
            $(".item-info-description").html(
                "<p><strong>First Name: </strong><span>" +
                itemData.info.firstname +
                "</span></p><p><strong>Last Name: </strong><span>" +
                itemData.info.lastname +
                "</span></p><p><strong>Birth Date: </strong><span>" +
                itemData.info.birthdate +
                "</span></p><p><strong>Licenses: </strong><span>" +
                itemData.info.type +
                "<p><strong>Endorsements: </strong><span>" +
                itemData.info.endorsement +
                "</span></p>"

            );
        }
```

<br />

## QB-POLICEJOB
<br />

> ## QB-Policejob/server/main
Find `if args[2] == 'driver' or args[2] == 'weapon' then` and replace both of the lines with:
```
if args[2] == 'driver' or args[2] == 'weapon' or args[2] == 'cdl' or args[2] == 'bike' or args[2] == 'permit'  then
```

> ## QB-Policejob/server/main
Find `QBCore.Commands.Add('takedrivinglicense', Lang:t('commands.drivinglicense'), {}, false, function(source)` and add these three underneath (picture shown below this section):

```
QBCore.Commands.Add('takepermit', Lang:t('commands.permit'), {}, false, function(source)
    local src = source
    local Player = QBCore.Functions.GetPlayer(src)
    if Player.PlayerData.job.type == 'leo' and Player.PlayerData.job.onduty then
        TriggerClientEvent('police:client:SeizePermit', source)
    else
        TriggerClientEvent('QBCore:Notify', src, Lang:t('error.on_duty_police_only'), 'error')
    end
end)

QBCore.Commands.Add('takebikelicense', Lang:t('commands.bike'), {}, false, function(source)
    local src = source
    local Player = QBCore.Functions.GetPlayer(src)
    if Player.PlayerData.job.type == 'leo' and Player.PlayerData.job.onduty then
        TriggerClientEvent('police:client:SeizeBike', source)
    else
        TriggerClientEvent('QBCore:Notify', src, Lang:t('error.on_duty_police_only'), 'error')
    end
end)

QBCore.Commands.Add('takecdllicense', Lang:t('commands.cdl'), {}, false, function(source)
    local src = source
    local Player = QBCore.Functions.GetPlayer(src)
    if Player.PlayerData.job.type == 'leo' and Player.PlayerData.job.onduty then
        TriggerClientEvent('police:client:SeizeCdl', source)
    else
        TriggerClientEvent('QBCore:Notify', src, Lang:t('error.on_duty_police_only'), 'error')
    end
end)
 ```
![image](https://github.com/RyZeItz/qb-dmv/assets/103600006/83446bf1-2502-4722-aeaa-fa2ff6d3d789)

> ## QB-Policejob/client/interactions
Find `RegisterNetEvent('police:client:SeizeDriverLicense', function()` and add these three underneath (picture shown below this section):
```
RegisterNetEvent('police:client:SeizePermit', function()
    local player, distance = QBCore.Functions.GetClosestPlayer()
    if player ~= -1 and distance < 2.5 then
        local playerId = GetPlayerServerId(player)
        TriggerServerEvent('police:server:SeizePermit', playerId)
    else
        QBCore.Functions.Notify(Lang:t('error.none_nearby'), 'error')
    end
end)

RegisterNetEvent('police:client:SeizePCdl', function()
    local player, distance = QBCore.Functions.GetClosestPlayer()
    if player ~= -1 and distance < 2.5 then
        local playerId = GetPlayerServerId(player)
        TriggerServerEvent('police:server:SeizeCdl', playerId)
    else
        QBCore.Functions.Notify(Lang:t('error.none_nearby'), 'error')
    end
end)

RegisterNetEvent('police:client:SeizeBike', function()
    local player, distance = QBCore.Functions.GetClosestPlayer()
    if player ~= -1 and distance < 2.5 then
        local playerId = GetPlayerServerId(player)
        TriggerServerEvent('police:server:SeizeBike', playerId)
    else
        QBCore.Functions.Notify(Lang:t('error.none_nearby'), 'error')
    end
end)
```
![image](https://github.com/RyZeItz/qb-dmv/assets/103600006/2a41b976-62c8-4440-ae61-ad6b9af3f036)

> ## QB-Policejob/locales/(whatever language you are using en= english etc)
Find `commands = {` and add (picture shown below this section):
```
permit = 'Seize Permit License (Police Only)',
cdl = 'Seize Commercial License (Police Only)',
bike = 'Seize Mototrcycle License (Police Only)',
```
![image](https://github.com/RyZeItz/qb-dmv/assets/103600006/3fdcfe6f-0935-49b3-abde-034d73d3568b)

> ## QB-Policejob/locales/(whatever language you are using en= english etc)
Find `license_type = 'License Type (driver/weapon)',` and replace with (picture shown below this section):
```
license_type = 'License Type (driver/weapon/permit/cdl/bike)',
```

<br />



That should be all for the installation. Now just start the server up and enjoy! (PLEASE NOTE IF PLAYERS ALREADY HAVE A DRIVERS LICENSE THIS WILL CAUSE HAVING TO DO A COMMERCIAL LICENCE BEFORE HAVING ACCESS TO THE BIKE LICENCE AS SOMEONE WITHOUT A DRIVERS LICENCE WILL HAVE TO TAKE THE CAR DRIVING TEST IN ABLE TO GAIN ACCESS TO THE BIKE AND COMMERCIAL LICENCE NOT THE COMMERCIAL. THIS CAN BE FIXED THROUGH YOUR DATABASE (REMOVING THE LICENCE FROM EVERYONE), OR CREATING A NEW CHARACTER, OR USING THE BUILT IN COMMAND IN THE CONFIG `Config.CommandName = 'resetlicense'                           -- Command to reset a players license meta back to false. (permit | driver | cdl | bike)`

# Contact Me

If you have any questions or any problems please don't hesitate to message me `_._ryze_._`
