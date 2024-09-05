# 3SP-Bot-Concept

**3SP-Bot** (name is not final) is a Discord bot written in Python using the [hikari](https://www.hikari-py.dev/]) and [hikari-lightbulb](https://hikari-lightbulb.readthedocs.io/en/latest/) libraries.
  
At the core it is a CRUD app, utilizing permissionbound commands within Discord to manage different aspects of the game **Foxhole**.
  
The goal of the bot is to provide helpful functionality to the Sundial Coalition and its regiments.

## Development Team

- [[3SP] Labret](https://github.com/Labretx) (Maintainer)

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

## Msupps

## Orders

### MPF Orders

### Msupps Orders

### Custom Orders

## Map Info

### Resources

### Buildings

## Facilities

## Admin
