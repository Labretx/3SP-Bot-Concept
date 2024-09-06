# 3SP-Bot-Concept

**3SP-Bot** (name is not final) is a Discord bot written in Python using the [hikari](https://www.hikari-py.dev/]) and [hikari-lightbulb](https://hikari-lightbulb.readthedocs.io/en/latest/) libraries.
  
At the core it is a CRUD app, utilizing permissionbound commands within Discord to manage different aspects of the game **Foxhole**.
  
The goal of the bot is to provide helpful functionality to the Sundial Coalition and its regiments.

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

- [ ] Stockpiles
  - [ ] Create
  - [ ] Delete
  - [ ] List
  - [ ] SunCreate
  - [ ] SunDelete
  - [ ] SunList

- [ ] Msupps
  - [ ] Task to reduce inventory by demand every 24h
  - [ ] Commands
    - [ ] Create
    - [ ] Delete
    - [ ] List
    - [ ] Edit
    - [ ] Add
    - [ ] MapCreate
    - [ ] MapDelete
    - [ ] MapShow

- [ ] Orders
- [ ] Map Info
- [ ] Facilities
- [ ] Admin

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
There may be an exception for a coalition server.

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
> Only the user who created the stockpile will be able to delete it again.\
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

---

#### Creating Sundial Stockpiles

**Usage:**

```
/stockpile suncreate [hex] [city] [code] [type]
```

**Arguments:**

- hex: The hex the stockpile is located in. This will autocomplete as you type and is case insensitive.
- city: The city the stockpile is located in. This will autocomplete as you type and will only show cities that have access to a seaport or storage depot and is located in the selected hex.
- code: The code to access the stockpile with.
- type: The type of stockpile to be created. Available are: `Member`, `Veteran` and `Officer`. It is not possible to create a stockpile for a higher permission than the user has. A `Veteran` cannot create an `Officer` stockpile.
  
**Recommended Permission:** `Veteran`

> [!NOTE]
> Creating a Sundial stockpile doesn't require a name.\
> The name will be generated using a schema.\
> For a member stockpile created in Tine the name would be `SunTinM`.

---

#### Deleting Sundial Stockpiles

**Usage:**

```
/stockpile sundelete [id]
```

**Arguments:**

- id: The ID of the stockpile that should be deleted. The ID of each stockpile is displayed on the stockpile list.

**Recommended Permission:** Will be the same as `/stockpile suncreate`.

---

#### Listing Sundial Stockpiles

**Usage:**

```
/stockpile sunlist [type]
```

**Arguments:**

- type: The type of stockpiles to be shown. `/stockpile sunlist Veteran` will only show `Veteran` stockpiles. A `Veteran` cannot list the `Officer` stockpiles.

**Recommended Permission:** `Veteran`

> [!NOTE]
> There could be an option to show all stockpiles of the regiments belonging to Sundial or an option for the individual regiments to list their stockpiles in the Sundial server.


## Msupps

The `Msupps` module is used to manage maintenance tunnels and their daily msupps demand as well as processing deliveries to the tunnel.

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

**Recommended Permission:** WIll be the same as `/msupps create`.

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

### MPF Orders

### Msupps Orders

### Custom Orders

## Map Info

### Resources

### Buildings

## Facilities

## Admin
