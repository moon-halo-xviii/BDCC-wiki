# Status Effects

# Types of Status Effects
## Battle Effects

## Sex Engine Effects


# Your First Status Effect

# Class Reference

## Properties

| Type | Name | Default Value |
| -------- | -------- | -------- |
| bool | [alwaysCheckedForNPCs](#bool-alwayscheckedfornpcs--false) | false |
| bool | [alwaysCheckedForPlayer](#bool-alwayscheckedforplayer--false) | false |
| BaseCharacter | [character](#basecharacter-character) | |
| String | [id](#string-id) | |
| bool | [isBattleOnly](#bool-isbattleonly--false) | false |
| bool | [isSexEngineOnly](#bool-issexengineonly--false) | false |
| int | [priorityDuringChecking](#int-priorityduringchecking) | 0 |
| bool | [removedOnDungeonStart](#bool-removedondungeonstart--false) | false |
| bool | [subscribeCheckOnFightStart](#bool-subscribecheckonfightstart--false) | false |
| int | [turns](#int-turns---1) | -1 |

## Methods

| Return Type | Name |
| -------- | -------- |
| Buff | [buff](#buff-buff) |
| bool | [checkAvoidedBuff](#bool-checkavoidedbuff) |
| Array | [checkOnFightStart](#array-checkonfightstart) |
| void | [combine](#void-combine) |
| BaseCharacter | [contextGetEnemy](#basecharacter-contextgetenemy) |
| String | [contextGetEnemyID](#string-contextgetenemyid) |
| float | [getAccuracyMod](#float-getaccuracymod) |
| Array | [getBuffs](#array-getbuffs) |
| float | [getDamageMultiplierMod](#float-getdamagemultipliermod) |
| float | [getDodgeMod](#float-getdodgemod) |
| String | [getEffectDesc](#string-geteffectdesc) |
| String | [getEffectImage](#string-geteffectimage) |
| String | [getEffectName](#string-geteffectname) |
| Color | [getIconColor](#color-geticoncolor) |
| float | [getRecievedDamageMod](#float-getrecieveddamagemod) |
| String | [getVisibleDescription](#string-getvisibledescription) |
| void | [_init](#void-_init) |
| void | [initArgs](#void-initargs) |
| bool | [isDrugEffect](#bool-isdrugeffect) |
| void | [loadData](#void-loaddata) |
| void | [onFightEnd](#void-onfightend) |
| void | [onFightStart](#void-onfightstart) |
| void | [onSleeping](#void-onsleeping) |
| void | [onSexEnded](#void-onsexended) |
| void | [onSexEvent](#void-onsexevent) |
| void | [onSexStarted](#void-onsexstarted) |
| void | [processBattleTurn](#void-processbattleturn) |
| void | [processBattleTurnContex](#void-processbattleturncontex) |
| void | [processSexTurn](#void-processsexturn) |
| void | [processSexTurnContex](#void-processsexturncontex) |
| void | [processTime](#void-processtime) |
| Dictionary | [saveData](#dictionary-savedata) |
| void | [setCharacter](#void-setcharacter) |
| bool | [shouldApplyTo](#bool-shouldapplyto) |
| bool | [shouldHaveWideTooltip](#bool-shouldhavewideittooltip) |
| void | [stop](#void-stop) |

## Property Descriptions

### *bool* alwaysCheckedForNPCs = false

### *bool* alwaysCheckedForPlayer = false
When true, 

### *BaseCharacter* character
When an instance of a status effect is created and applied to a character, this property stores a reference to said character, allowing you to easily access their own properties and methods.

### *String* id
The unique reference for the status effect in the code. Not to be confused with the status effect's visible name, which is returned by the getEffectName method.

### *bool* isBattleOnly = false
When true, this effect will be removed at the end of battles.

Used for status effects exclusively used in battles, such as when a character is stunned, bleeding, or collapsed on the ground.

### *bool* isSexEngineOnly = false
When true, this effect will be removed at the end of dynamic sex encounters.

Used for status effects which only apply during dynamic sex. These typically manipulate properties exclusive to the sex engine, such as arousal, dom anger/sub fear, resistance/forced obedience, etc.

### *int* priorityDuringChecking
The greater this property's value, the earlier it will be updated relative to other status effects every update.

### *bool* removedOnDungeonStart = false
When true, this effect will be removed whenever the player starts a drug den run.

Since drug den gameplay follows roguelike conventions in which players start without any perks or equipment, beneficial status effects should also be cleared when starting a new run. For example, the benefits of doing yoga and lifting weights at the gym are cleared upon starting a new drug den run.

### *bool* subscribeCheckOnFightStart = false
When true, at the start of a battle, the [checkOnFightStart](#checkonfightstart-array) method is called. This allows you to conditionally apply the status effect to a character at the beginning of a fight: see the method for more information.

### *int* turns = -1
Typically used to track the remaining duration of the effect.

When the effect is first applied, the base duration should be set with the _init method, and then decremented as time passes with the appropriate processing method:

* For battle-only status effects, this property can be used to measure the effect's duration by battle turns, as the name suggests; in this case, the property is decremented with the [processBattleTurn](#processbattleturn-void) method. For example, the Bleeding status effect lasts for three turns by default, so turns = 3.

* Alternatively, duration can be measured by game seconds, according to the in-game clock; in this case the property is decremented with the [processTime](#processtime-void) method. For example, the Yoga status effect lasts for twelve hours, so turns = 12\*60\*60.




## Method Descriptions

### *void* processBattleTurn
Runs at the (start/end) of every turn of battle.

When decrementing [turns](#turns-int) with this method, you 

### *void* processTime ( )
description

### *String* getEffectName ( )

### *Array* checkOnFightStart (_npc:BaseCharacter, _context:Dictionary )
