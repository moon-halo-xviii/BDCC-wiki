This guide assumes you have a basic understanding of the dev environment for BDCC. If you don't, you should try making a [basic item mod](Your-first-mod.-New-item) first. It will help get you familiar with how developing mods for the game goes, which is a good thing.

This guide goes through World Edits, what they can do, and then finally yes how to add a floor to an existing floor. If all you can for was a guide for doing the latter, feel free to click [here](#adding-a-room)

## Contents

- [World Edits](#world-edits)
- [GameWorld](#world-edits)
- [Getting A Room ID](#getting-a-room-id)
- [Adding a Room](#adding-a-room)
- [Real World Example](#real-world-example)

## World Edits

Mods can change the floors and rooms of the base-game with `World Edits`. A basic world edit can be seen in [Artica's module here][Artica World Edit]:

```gdscript
extends WorldEditBase

func _init():
    id = "ArticaWorldEdit"

func apply(world: GameWorld):
    if(getFlag("ArticaModule.corruptionBegan", false)):
        world.setRoomSprite("main_bench3", RoomStuff.RoomSprite.PERSON)
```

This **does not** add a new room to the floor, but instead edits an existing room. In this case, it changes the [sprite][Wikipedia Sprite] of the room `main_bench_3` to indicate there's a person in it. If you've completed Artica's content, you probably noticed this. It's when she leaves the cafeteria and takes up residence in the main hall.

<div align="center">
    <img src="images\room-to-existing-floor\artica-before-after.png" alt="An image showcasing the main hall before and after the above World Edit is applied" width="80%"/>
</div>

World Edits are the mechanism for doing things like this, and also adding new rooms to the floor.

[Artica World Edit]: https://github.com/Alexofp/BDCC/blob/main/Modules/ArticaModule/c1Corruption/ArticaWorldEdit.gd
[Wikipedia Sprite]: https://en.wikipedia.org/wiki/Sprite_(computer_graphics)

## Game World

The `apply` function of a `WorldEdit` gets a reference to the `GameWorld`. This is also exposed globally as `GM.world` if you need to get ahold of it elsewhere in code. You probably don't need to do this though.

In essence, `GameWorld` controls the map of the game. It handles things like path finding and the camera for the map too, but you probably don't need to worry about that.

You can give the code a look [here][Game World] because there are a lot of useful functions on it. Here's some of them that you may care about:

- `setRoomSprite(id: String, newRoomSprite: int)`, to set the sprite of a room
- `setRoomColor(id: String, newRoomColor: int)`, to set the _color_ of a room.
- `setRoomGridColor(id: String, newRoomColor: int)`, to set the color of the diagonal lines on the room

<div align="center">
<img src="images\room-to-existing-floor\grid-rooms.png" alt="Some rooms with their room color set to Green and the grid color set to Red" width="80%"/>

<sub>Some rooms with their room color set to Green and the grid color set to Red</sub>
</div>

- `switchToFloor(floorID: String)`, to switch the camera to a new floor
- `setDarknessVisible(vis: bool)`, to enable or disable the darkness on the map, like when you're blindfolded or in the drug den
- `addRoom(floorID: String, roomID: String, roomPosition: Vector2, roomData: Dictionary)`, to add a new room. We're getting there, I promise.

The colors and sprites you can use are stored in `RoomStuff`. You can read them [here][Room Stuff].

World Edits can also run every update if necessary. To do that, add `isRegular = true` to their `_init` function. Only do this if you know what you're doing.

[Game World]: https://github.com/Alexofp/BDCC/blob/main/Game/World/World.gd
[Room Stuff]: https://github.com/Alexofp/BDCC/blob/main/Game/World/RoomStuff.gd

## Getting A Room ID

You probably noticed that all of the functions on `GameWorld` require an ID. The room IDs aren't shown anywhere in-game, and you will need the Godot Editor for that. Once there, open BDCC in the editor and in the `FileSystem` panel, navigate to `Game/World/Floors`.

This contains all of the floors for the game. If you double click on a **.tscn** file in the `FileSystem` panel, you it will open the map in the editor viewport.

<details>
<summary> If you double click on a `.gd` file, you need to click `2D` at the top of the screen. Click here for a picture. </summary>

<div align="center">
    <img src="images\room-to-existing-floor\2d-view.png" alt="An arrow pointing to the top middle of the screen at a button that reads '2D'" width="80%"/>
</div>

</details>

To get the ID of a room, click on the room in the editor and then read the Room Id in the Inspector panel.

<div align="center">
    <img src="images\room-to-existing-floor\get-room-id.png" alt="An arrow pointing to the top right of the screen at the Room Id section of the Inspector panel" width="80%"/>
</div>

Probably the floors you care about are:
- `Cellblock`, the cells floor
- `MainHall`, the main prison area
- `FightClubFloor`, the fight club floor
- `Medical`, the medical floor
- `MiningFloor`, the floor with mining and engineering
- `BlacktailMarket`, the Blacktail ship

## Adding a Room

To add a new room, simply call `addRoom` on the `GameWorld` in a World Edit:

```gdscript
extends WorldEditBase

func _init():
    id = "Tutorial_WorldEdit"

func apply(world: GameWorld):
    world.addRoom("Cellblock", "tutorial_island", Vector2(1, 1), {})
```

Then, make sure the World Edit is registered in the `Module.gd`.

<sub>Again, if you need help with this part, see the item tutorial.</sub>

```gdscript
extends Module

func _init():
    id = "Tutorial"
    author = "YourNameHere"

    worldEdits = [
        "res://PathTo/WorldEdit.gd"
    ]
```

The first [parameter][Wikipedia Parameter] of `addRoom` is the floor ID. It's the name of one of the floors. These match the filenames in `Game/World/Floors`.

The second parameter is the new room's ID. This can be whatever you want, but it _must_ be unique. This includes other people's mods, not just yours. It is good practice to add a prefix to indicate what the room ID is for, like `yourname_` or `tutorial_` so that as many mods as possible are compatible with yours.

The third parameter is the location of the new room. The grid used by the game is automatically handled by the function. That means you can just use e.g. `Vector2(1, 1)` and the game will put it in the right spot even though the game engine does not use the same scale. The coordinate system is [covered more below](#the-coordinate-system).

The fourth parameter is options for the creation of the room. It's a dictionary that contains data for creating the room. The following options are supported:

```gdscript
{
    # The name of the room that's displayed near the map.
    "name": "Tutorial Room",
    # The description of the room that's shown when you're standing in it.
    "desc": "You are standing in a room. It looks weird.",
    # The icon that's shown in the room. Things like stairs and a person.
    "icon": RoomStuff.RoomSprite.IMPORTANT,
    # The main background color of the room on the map
    "color": RoomStuff.RoomColor.Red,
    # The color of the diagonal grid lines on a room. If you want them to be invisible, set this to `White`.
    "gridColor": RoomStuff.RoomColor.Blue,
    # Whether or not you can enter the room from the North of it
    "canN": true,
    # Whether or not you can enter the room from the East of it
    "canE": true,
    # Whether or not you can enter the room from the South of it
    "canS": true,
    # Whether or not you can enter the room from the West of it
    "canW": true,
}
```

If you leave any or all of those options out, the room will still exist but it may not look like you want it to.

[Wikipedia Parameter]: https://en.wikipedia.org/wiki/Parameter_(computer_programming)

## The Coordinate System

You're here to mod a game, not do math. This will be quick.

Rooms are laid out on a grid. Where `(0, 0)` is varies from floor to floor. You can check this by opening the floor in Godot's editor and looking to see where the crosshair is. When setting the position of a room, room `(1, 0)` is right next to the room at `(0, 0)`. The game will handle spacing it appropriately for you. However, positive and negative coordinates are probably not what you're used to. Here's a diagram:

<div align="center">
    <img src="images\room-to-existing-floor\cellblock-coordinates.png" alt="A diagram of coordinates for BDCC. Top left is labeled as (-, -). Top right is labeled (+, -). Bottom left is labeled (-, +). Bottom right is labeled (+, +)." width="60%"/>
</div>

These quadrants are relative to the origin (where `(0, 0)` is). So for example a room located at `(1, 1)` would be down and to the right of the room at `(0, 0)`. A room at `(-1, 0)` would be directly left of the one at `(0, 0)`.

## Real World Example

If you had this World Edit:

```gdscript
extends WorldEditBase

func _init():
    id = "Tutorial_WorldEdit"

func apply(world: GameWorld):
    world.addRoom("MainHall", "tutorial_room", Vector2(-1, 0), {
        "name": "Tutorial Room",
        "desc": "You are standing in a room. It looks weird.",
        "icon": RoomStuff.RoomSprite.IMPORTANT,
        "color": RoomStuff.RoomColor.Red,
        "gridColor": RoomStuff.RoomColor.Blue,
        "canN": true,
        "canE": true,
        "canS": true,
        "canW": true,
    })
```

The result is this room:

<div align="center">
    <img src="images\room-to-existing-floor\end-room.png" alt="The result of the World Edit above." width="30%"/>
    <img src="images\room-to-existing-floor\end-result.png" alt="The result of the World Edit above." width="80%"/>
</div>