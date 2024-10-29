# 3SP-Bot-Concept

**3SP-Bot** (name is not final) is a Discord bot written in Python using the [hikari](https://www.hikari-py.dev/]) and [hikari-lightbulb](https://hikari-lightbulb.readthedocs.io/en/latest/) libraries.
  
At the core it is a CRUD app, utilizing permissionbound commands within Discord to manage different aspects of the game **Foxhole**.
  
The goal of the bot is to provide helpful functionality to the 3SP clan.

## Development Team

- [[3SP] Labret](https://github.com/Labretx) (Maintainer)

## Development Progress

- [ ] Permissions
  - [ ] Command checks
  - [ ] Database table

- [ ] Configuration
  - [ ] Permissions
  - [ ] Stockpiles
  - [ ] Msupps
  - [ ] Orders
  - [ ] Map Info
  - [ ] Facilities
  - [ ] Admin

- [x] Stockpiles
  - [x] Create
  - [x] Delete
  - [x] List

- [ ] Msupps
  - [x] Task to reduce inventory by demand every hour
  - [x] Commands
    - [x] Create
    - [x] Delete
    - [x] List
    - [x] Edit
    - [x] Add
    - [ ] MapCreate
    - [ ] MapDelete
    - [ ] MapShow

- [ ] Orders
  - [x] Msupps
  - [ ] Commands
    - [x] Create
    - [x] MPF
    - [ ] Stats
    - [x] Recurring
    - [x] Pause
    - [x] Unpause
    - [x] Delete
    - [x] List

- [ ] Map Info
  - [ ] Territory Updates
  - [ ] Commands
    - [ ] Resources
    - [ ] Buildings

- [ ] Facilities
  - [ ] Create
  - [ ] Delete
  - [ ] Edit

- [ ] Activity
  - [ ] Commands
    - [ ] Check
  - [ ] Reaction Listener
  
- [ ] Admin
  - [x] Whitelist Add
  - [x] Whitelist Remove
  - [ ] Data Create
  - [ ] Data Wipe 

# Functionalities
Below is a list of all the proposed functionalities **3SP-Bot** could have.
  
These will be individual modules and can be accessed in discord with the following command pattern:
  
```
/module command arguments
```

The only exceptions to this will be the [Permissions](#permissions), [Admin](#admin) and [Configuration](#configuration) module.
  
Each module will contain discord commands but may also have commandless features, like a task scheduler for example.

## Permissions

Each regiment might have different hierarchies and an idea of who should be allowed to do what.

For that reason there will be a few permissions available to be set:

- Admin
- Trusted
- Officer
- Veteran
- Member

`Admin` and `Trusted` permissions are managed by **[3SP] Labret**.
  
`Admin` will be able to use special commands needed to manage botdata, for example wiping all data after the end of a war.
  
`Trusted` will be able to manage the server whitelist.
  
`Officer`, `Veteran` and `Member` permissions are managed by the regiments server mods/admins in their respective servers.
  
`Officer` is the highest permission and has access to all `Officer`, `Veteran` and `Member` commands.\
This permissions should be reserved for the regiments leadership.
  
`Veteran` has access to all `Veteran` and `Member` commands.\
This permission should be used for functionalities that require a certain amount of trust or knowledge.
  
`Member` is the lowest permission and only has access to `Member` commands.\
This permission should be given to the vast majority of regiment members and used for commands, that should be available to everyone.
  
To manage these permissions, they can be linked to Discord roles via commands in the server.\
It is possible to assign a permission to multiple roles, but not multiple permissions to a single role.

> [!NOTE]
> The `Admin` and `Trusted` permissions do **NOT** grant access to `Officer`, `Veteran` or `Member` commands if the user doesn't have the respective permission given in the regiments server.\
> They are purely for managing data and access to the bot.

## Configuration

Each module will be provided with a `config` command, since a lot of bigger servers have a lot of channels and a good overview could prove difficult without proper configuration.
  
These commands will be able to set dedicated channels for usage of module commands as well as the permission needed for each command.

> [!WARNING]
> Module commands wont be able to be used if not configured in a server.

## Stockpiles

The `stockpile` module will allow members to create, delete and display the stockpiles of a regiment.\
The stockpiles are server dependant, meaning a regiments server cannot access the stockpiles of another regiments server.\

### Commands

#### Creating Stockpiles

**Usage:**
  
```
/stockpile create [name] [hex] [city] [code] [type]
```

**Arguments:**

- name: The name of the stockpile as chosen in the game.
- hex: The hex the stockpile is located in. This will autocomplete as you type and is case insensitive.
- city: The city the stockpile is located in. This will autocomplete as you type and will only show cities that have access to a seaport or storage depot and is located in the selected hex.
- code: The code to access the stockpile with.
- type: The type of stockpile to be created. Available are: `Member`, `Veteran` and `Officer`. It is not possible to create a stockpile for a higher permission than the user has. A `Veteran` cannot create an `Officer` stockpile.

**Recommended Permission:** `Veteran`

> [!NOTE]
> If a stockpile is created it is given an ID in the database, which is needed to delete the stockpile again.\
> The Discord ID of the user creating the stockpile will also be saved to the database.

---

#### Deleting Stockpiles

**Usage:**

```
/stockpile delete [id]
```

**Arguments:**

- id: The ID of the stockpile that should be deleted. The ID of each stockpile is displayed on the stockpile list.

**Recommended Permission:** Will be the same as `/stockpile create`.
  
> [!NOTE]
> Deleting a stockpile can be necessary when a stockpile is lost due to the seaport/storage depot being destroyed or when the reservation timer ran out.

---

#### Listing Stockpiles

**Usage:**

```
/stockpile list [type]
```

**Arguments:**

- type: The type of stockpiles to be shown. `/stockpile list Veteran` will only show `Veteran` stockpiles. It is not possible to list the stockpiles for a higher permission than the user has. A `Veteran` cannot list the `Officer` stockpiles.

**Recommended Permission:** `Veteran`

> [!NOTE]
> There can only be a single stockpile list per permission per server.\
> To create another one, the old message needs to be deleted first.\
> Once a stockpile list is created the `/stockpile create` and `/stockpile delete` commands will update the existing message.

## Msupps

The `msupps` module is used to manage maintenance tunnels and their daily msupps demand as well as processing deliveries to the tunnel.

### Commands

#### Creating a Maintenance Tunnel

**Usage:**

```
/msupps create [name] [demand] [inventory]
```

**Arguments:**

- name: The name of the maintenance tunnel. Should be able to be identified on a map.
- demand: The daily demand of the maintenance tunnel.
- inventory: The current amount of msupps in the maintenance tunnel.

**Recommended Permission:** `Veteran`

> [!NOTE]
> This will add a timestamp to the database when the maintenance tunnel was added and a task to reduce the inventory of the tunnel by the daily demand every 24 hours.

---

#### Deleting a Maintenance Tunnel

**Usage:**

```
/msupps delete [id]
```

**Arguments:**

- id: The ID of the maintenance tunnel. Can be found on the maintenance tunnel list.

**Recommended Permission:** Will be the same as `/msupps create`.

---

#### Listing Maintenance Tunnels

**Usage**:

```
/msupps list
```

**Recommended Permission:** Will be the same as `/msupps create`.

> [!NOTE]
> When the created task updates the inventory, it will also reflect on the list.

---

#### Filling a Maintenance Tunnel

**Usage:**

```
/msupps add [name] [amount]
```

**Arguments:**

- name: The name of the maintenance tunnel.
- amount: The amount of msupps delivered to the maintenance tunnel.

**Recommended Permission:** `Member`

---

#### Adjusting Maintenance Tunnel Inventory

**Usage:**

```
/msupps edit [name] [inventory]
```

**Arguments:**

- name: The name of the maintenance tunnel to edit.
- inventory: The new inventory of the tunnel.

**Recommended Permission:** Will be the same as `/msupps create`.

> [!NOTE]
> It might be necessary to adjust the amount of msupps in a maintenance tunnel if deliveries are not added via command, players outside of the regiments deliver to the tunnel or someone takes msupps out of the tunnel.

---

#### Creating a Maintenance Tunnel Map

**Usage:**

```
/msupps mapcreate [name] [map]
```

**Arguments:**

- name: The name of the map.
- map: An image of the map showing maintenance tunnel positions.

**Recommended Permission:** Will be the same as `/msupps create`.

---

#### Deleting a Maintenance Tunnel Map

**Usage:**

```
/msupps mapdelete [name]
```

**Arguments:**

- name: The name of the map to delete.

**Recommended Permission:** Will be the same as `/msupps create`.

---

#### Displaying a Maintenance Tunnel Map

**Usage:**

```
/msupps mapshow [name]
```

**Arguments:**

- name: The name of the map to display.

**Recommended Permission:** Will be the same as `/msupps create`.

## Orders

The `order` module allows the creation of orders for preparation of OPs, facility maintenance, moving stockpile inventory, etc.

Created orders will be sent to the chat and have a button to lock the specific order to the person clicking the button.

After locking an order, there will be another button to mark the order as fulfilled.

The database will keep track of the orders and can be used to create statistics.

### Commands

#### Creating an Order

**Usage:**

```
/order create [order] [amount]
```

**Arguments:**

- order: The text of the order. For example: *60 crates shirts to Buckler Sound*
- amount (optional): The amount of orders to be created. Default is 1.

**Recommended Permission:** `Officer`

---

#### Creating an MPF Order

**Usage:**

```
/order mpf [item] [amount]
```

**Arguments:**

- item: The item that should be queued. Will autocomplete while typing.
- amount (optional): The amount of orders to be created. Default is 1.

**Recommended Permission:** Will be the same as `/order create`.

> [!NOTE]
> MPF orders will always show the material cost in materials and crates.
> An MPF order will **always** be a full queue.

---

#### Creating a recurring Order

**Usage:**

```
/order recurring [order] [amount] [interval]
```

**Arguments:**

- order: The text of the order. For example: *60 crates shirts to Buckler Sound*
- amount (optional): The amount of orders to be created. Default is 1.
- interval (optional): The interval of the recurring order in days. Default is 1.

**Recommended Permission:** Will be the same as `/order create`.

#### Pausing a recurring Order

**Usage:**

```
/order pause [id]
```

**Arguments:**

- id: The ID of the order to pause.

**Recommended Permission:** Will be the same as `/order create`.

---

#### Unpausing a recurring Order

**Usage:**

```
/order unpause [id]
```

**Arguments:**

- id: The ID of the order to unpause.

**Recommended Permission:** Will be the same as `/order create`.

---

#### Deleting a recurring Order

**Usage:**

```
/order delete [id]
```

**Arguments:**

- id: The ID of the order to delete.

**Recommended Permission:** Will be the same as `/order create`.

---

#### Listing recurring Orders

**Usage:**

```
/order list
```

**Recommended Permission:** Will be the same as `/order create`.

---

#### Displaying stats

**Usage:**

```
/order stats
```

**Recommended Permission:** `Officer`

> [!NOTE]
> This will display a descending ranking of completed orders per member.\
> Instead of a message, this will be visualized.

### Msupps Orders

The Msupps part of the `order` module does not have commands, but is rather an extension of the `msupps` module.

An msupps order will automatically be created, if the maintenance tunnel would be empty in 24 hours or less.

## Map Info

The `mapinfo` module will allow members quick access to the location of resource fields/mines as well as industry buildings.

To do this efficiently this module will utilize the [warapi](https://github.com/clapfoot/warapi) to pull information about the current war.

The dynamic data has to be wiped after a new war starts.

### Commands

#### Locate Resources

**Usage:**

```
/mapinfo resources [resource] [hex]
```

**Arguments:**

- resource (optional): The resource you want to find locations for. If not set will display all resource fields/mines.
- hex (optional): The hex where you want to find resources in. If not set will display all map locations.

**Recommended Permission:** `Member`

---

#### Locate Buildings

**Usage:**

```
/mapinfo buildings [building] [hex]
```

**Arguments:**

- resource (optional): The building you want to find locations for. If not set will display all buildings.
- hex (optional): The hex where you want to find buildings in. If not set will display all map locations.

**Recommended Permission:** `Member`

> [!WARNING]
> Buildings only include industry buildings that are found within towns!

### Territory Updates

Using the [warapi](https://github.com/clapfoot/warapi) there will be daily updates on which territories on the map have changed owner and will be sent to the configured chat.

## Facilities

The `facilities` module is used to create facilities and useful information about them.

Each facility will have a defined in- and output, which can be changed as needed so members know what to deliver to facilities to keep them running.

### Commands

#### Creating a Facility

**Usage:**

```
/facility create [name] [input] [output] [location] [manager]
```

**Arguments:**

- name: The name of the facility.
- input: Resources to deliver to the facility.
- output: Resources the facility produces.
- location: An image of the map highlighting where to find the facility.
- manager: The member who is responsible for the facility.

**Recommended Permission:** `Officer`

---

#### Deleting a facility

**Usage:**

```
/facility delete [name]
```

**Arguments:**

- name: The name of the facility to be deleted.

**Recommended Permission:** Will be the same as `/facility create`.

> [!NOTE]
> It might be necessary to delete a facility if it is abandoned or lost.\
> Only the member that created a facility will be able to delete it again.

---

#### Editing a Facility

**Usage:**

```
/facility edit [name] [newname] [input] [output] [location] [manager]
```

**Arguments:**

- name: The name of the facility.
- newname (optional): The new name of the facility.
- input (optional): Resources to deliver to the facility.
- output (optional): Resources the facility produces.
- location (optional): An image of the map highlighting where to find the facility.
- manager (optional): The member who is responsible for the facility.

**Recommended Permission:** Will be the same as `/facility create`

> [!NOTE]
> Only the member that created the facility will be able to delete it again.

## Activity

The `activity` module can be used to create activity checks for roles and if failed will remove a members role and assigns a vacation role.

This is done to minimize access to intel and obviously to see who is active and who is not.

### Commands

#### Creating an Activity Check

**Usage:**

```
/activity check [role] [emoji] [deadline]
```

**Arguments:**

- role: The role to create an activity check for.
- emoji: The emoji people need to react to, to complete the activity check.
- deadline: The time where 

**Recommended Permission:** `Officer`

> [!NOTE]
> The activity check will be added to the next message, the person sends in the channel the command was executed in.\
> After reaching the deadline, all members who haven't completed the activity check will have all roles removed and will be assigned the configured role.

## Admin

The `admin` module is used to manage data necessary for the other modules to work correctly.

This includes for example changing locations of resource fields and industry buildings.

### Commands

#### Whitelisting a Server

**Usage:**

```
/whitelist add
```

**Required Permission:** `Trusted`

> [!NOTE]
> Whitelisting means enabling the bot for the server where the command was executed.

---

#### Removing a Server from the Whitelist

**Usage:**

```
/whitelist remove
```

**Required Permission:** `Trusted`

---

#### Adding City Configuration and Resource Locations

**Usage:**

```
/data create
```

**Required Permission:** `Admin`

---


#### Wiping dynamic Data

**Usage:**

```
/data wipe
```

**Required Permission:** `Admin`

> [!CAUTION]
> This command is ***ONLY*** to be used at the start of the war!\
> This removes all data from the stockpiles and city_buildings table. 
