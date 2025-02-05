# Pentamana

Pentamana is a scoreboard-based mana mod that runs server-side providing mana modification and damage calculation hooks.

Pentamana acts as a mana system, handling tasks such as mana regeneration and mana consumption. It display manabar in actionbar.

## Mana Mechanic

Each player starts with 33,554,431 mana capacity (1 star in the manabar). Mana capacity is maxed out at 2,147,483,647 mana, or 64 stars total. The mana capacity would be calculated by the formula below:

``` txt
Mana Capacity = 33554431 + Capacity Enchantment Level * 33554432
```

A player basicly regenerate 1,048,576 mana every tick (32 ticks per star). The mana regeneration amount per tick would be calculated by the formula below:

``` txt
Mana Regen = 1048576 + Stream Enchantment Level * 65536
```

The output magic damage from casting would be calculated by the formula below: (Needs self-implementation)

``` txt
Magic Damage = amount * (Mana Capacity / Mana Scale) + [0.5 + Potency Enchantment Level * 0.5](if potency presented)
```

## Enchantments

Enchantment damage for custom weapons would need additional apply.

### Capacity

- Maximum level: II
- Primary items: Stick
- Secondary items: Axe, Hoe, Mace, Pickaxe, Shovel, Sword, Trident
- Enchantment weight: 2

Capacity adds extra mana capacity 33,554,432 per level.

### Stream

- Maximum level: III
- Primary items: Stick
- Secondary items: Axe, Hoe, Mace, Pickaxe, Shovel, Sword, Trident
- Enchantment weight: 5

Stream adds extra mana regeneration 65,536 per level.

### Potency

- Maximum level: V
- Primary items: Stick
- Secondary items: Axe, Hoe, Mace, Pickaxe, Shovel, Sword, Trident
- Enchantment weight: 10

Potency adds 1 extra casting magic damage for the first level and 0.5 for all subsequent levels.

### Utilization

- Maximum level: V
- Primary items: Stick
- Secondary items: Axe, Hoe, Mace, Pickaxe, Shovel, Sword, Trident
- Enchantment weight: 5

Utilization reduces the mana cost of casting by 10% per level.

## Commands

`/mana enbale` Enable this mod for yourself.

`/mana disable` Disable this mod for yourself completely.

`/mana character full <full>` Set mana character for 2 point mana.

`/mana character full <half>` Set mana character for 1 point mana.

`/mana character full <zero>` Set mana character for 0 point mana.

`/mana color <value>` Set mana color which is used in manabar.

`/mana reset` Reset mana character and color settings.

This mod is disbled for every player by default.

## Configuration

The config file is not shipped along with the mod. Below is a template config file `config/pentamana.json` filled with default values. You may only write the lines you would like to modify.

```json
{
  // Amount of mana be considered as 1 point mana.
  "manaScale": 16777216,
  // Initial mana capacity, should be odd.
  "manaCapacityBase": 33554431,
  // Used in capacity enchantment, should be even.
  "manaCapacityIncrementBase": 33554432,
  // Initial mana regen amount per tick.
  "manaRegenBase": 1048576,
  // Used in stream enchantment
  "manaRegenIncrementBase": 65536,
  // Ticks of actionbar updating suppression when interrupted
  "maxManabarLife": 40,
  // Mana character for 2 point mana.
  "manaCharFull": "★",
  // Mana character for 1 point mana.
  "manaCharHalf": "⯪",
  // Mana character for 0 point mana.
  "manaCharZero": "☆",
  // Used in manabar
  "manaColor": "aqua",
  // Make the mod enabled for every player when setting to ture, do not modify their own preference.
  "forceEnabled": false
}
```

Enchantments are registed using datapack. You can open mod jar and edit it.

## Objectives

`pentamana.mana` Mana supply at last tick

`pentamana.mana_capacity` Mana capacity at last tick

`pentamana.mana_regen` Amount of mana regened at last tick

`pentamana.mana_consume` Amount of mana to consume

`pentamana.manabar_life` Ticks left till next update if idle

`pentamana.mana_char_full` The complement of mana character for 2 point mana.

`pentamana.mana_char_half` The complement of mana character for 1 point mana.

`pentamana.mana_char_zero` The complement of mana character for 0 point mana.

`pentamana.color` The mana color which is used in manabar.

`pentamana.enabled` 1 if enabled, otherwise not.

## Events

- `TickManaCallback` Called after the mana capacity calculation, before everything else.

- `RegenManaCallback` Called when a player is regenerating mana. After the mana regeneration calculation, before regenerating mana.

- `ConsumeManaCallback` Called when a player is consuming mana. After the mana consumption calculation, before consuming mana.

## Tutorial: Create your very own magic weapon

This tutorial assumes that you already have a method that will be called when the weapon is used.

First, set the mana the weapon consumes. For example, 2,000,000.

```java
ServerCommandSource source = player.getServerCommandSource();
ManaCommand.executeSetManaConsume(source, 2000000)
```

Second, consume the mana and fire your weapon if successful consumed.

```java
if (ManaCommand.executeConsume(source) == 0) {
  return;
}

// Your code here
```

The result code will look like this:

```java
public void UseExampleWeapon(ServerPlayerEntity player) {
  ServerCommandSource source = player.getServerCommandSource();
  ManaCommand.executeSetManaConsume(source, 2000000)

  if (ManaCommand.executeConsume(source) == 0) {
    return;
  }

  // Your code here
}
```

## License

You are free to port it to any mod loader other than fabric as long as you credit the origin version.
