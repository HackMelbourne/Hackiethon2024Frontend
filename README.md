# Hackiethon_2024_participant
The codebase for hackiethon 2024

# TLDR - Make a bot in 3 minutes:
	- Go to Submissions and create a new bot
 	- Get code from template.py and paste it into the new file (in our case newbot.py)
	- Select PRIMARY_SKILL and SECONDARY_SKILL (see skills in documentation below)
	- Edit get_move() function


### Setup Frontend (MacOS):
	- Download folder
	- Open XCode Project (Update XCode if necessary)
	- In Resources/Data/StreamingAssets/Round_1, update the p1.json, p2.json and round.json files
	- Run game

### Setup Frontend (Windows):
	- In Resources/Data/StreamingAssets/Round_1, update the p1.json, p2.json and round.json files
	- Run the .exe file







# Introduction:
### The tournament will follow a bracket-style format where teams control characters using ai who will engage in one-on-one battles.



#Winning Conditions:
	- There are three ways to win a match:
	- Knockout: Defeat the opponent by reducing their character's health to zero.
	- Time Limit: If the match reaches the time limit, the player with more remaining health wins.
	- Coin Flip: If both players have the same amount of health when the time runs out, the winner will be determined by a coin flip.





## Game mechanics:
Hp:
Each player has 100 hp which does not regenerate unless a player has the “meditate” skill,

Tick system
Actions are taken in intervals called "ticks" or turns. During each tick, players have the opportunity to perform actions unless they are incapacitated by being stunned. Unlike traditional fighting games, there is no initial delay before actions can be taken. However, certain moves may have a built-in cooldown for balance purposes.

Time limit 
The time limit is 60 seconds with each player having 120 ticks per game

Basic Mechanic

Movements
These actions are common for all characters
Movement 
All movements have priority, meaning that characters will always move first before an attack can land
Jump : moves the character up by 2 X-positions, then back to the original position, taking 4 ticks in total
Jump forward : moves the character up by 1 X-position and 1 Y-position, moving in an arc and landing 3 X-positions forward on the ground, taking 3 ticks total
Jump backwards: similar to jump forward, but backwards
Move forward/back : moves character forward or back by 1 X-Position, takes 1 tick
	Constants defined in code
JUMP = ("move", (0,1))
FORWARD = ("move", (1,0))
BACK = ("move", (-1,0))
JUMP_FORWARD = ("move", (1,1))
JUMP_BACKWARD = ("move", (-1, 1))

Example:
return BACK

Attack
Light attack - Short range (1 X-pos) attack that does small damage with neither knockback nor stun
Startup: 0
Cooldown: 1
Damage: 1
Horizontal Range: 10
Vertical Range: 0
Blockable: False
Knockback: 2
Stun: 2



startup: int,
   cooldown: int,
   damage: int,
   xRange: int,
   vertical: int,
   blockable: bool,
   knockback: int,
   stun: int
)
0, 1, 5, 1, 0, True, 0, 1
Heavy attack - Short range (1 X-pos) attack that does medium damage with some knockback and stun
Startup: 0
Cooldown: 1
Damage: 1
Horizontal Range: 10
Vertical Range: 0
Blockable: False
Knockback: 2
Stun: 2



1, 4, 10, 1, 0, True, 2, 2
Combo attack - If the previous two moves were light attacks, the next light or heavy attack will deal increased damage, knockback and stun




Constants in code
LIGHT = ("light",)
HEAVY = ("heavy",)
Example:
return LIGHT


Block
When a character is blocking, they gain a shield with a certain amount of shield HP
Shield HP will absorb the damage a character would take, and their knockback and stun are reduced to 0.
If a character blocks continuously and accumulates more damage than their shield HP, they will be stunned for some time.
BLOCK = ("block",)
return BLOCK


Parry
A parry occurs when a character starts blocking on the same tick they would be hit by an attack.
A successful parry by the character stuns the enemy for a short period of time.


Cooldown
All primary and secondary skills have cooldowns, which vary based on the potency of the skill for game balance. Movement and blocking do not have cooldowns. Light attacks do not have a cooldown, but heavy attacks have a short cooldown so as to disincentivize spamming them.


Parameters:
There are 4 parameters available for the player to pick. 
Player (the player’s class)
Enemy (the enemy’s class)
Player_projectiles (list of projectiles player have spawned)
Enemy_projectiles (list of projectiles enemy have spawned)
Character information:

Example:
    player.get_hp()
def get_pos(self)  - Character position in (x,y) form
def get_proj_pos(proj) - get projectile’s pos in (x,y) form
def get_hp(self) - Character HP
def get_blocking(self) - check Blocking, if blocking, return shield HP
def primary_on_cooldown(player) - Check Primary skill cooldown
def secondary_on_cd(self) - Check Secondary skill cooldown
def heavy_on_cd(self) - Check heavy attack cooldown
def primary_range(self) - Primary skill range
def secondary_range(self) - Secondary skill range
def get_last_move(self) - last move
def get_past_move(self, turns) -  get nth move
def get_stun(self) - check character stun
def get_recovery(self) - Current recovery status
def skill_cancellable(self) - check if current skill can be cancelled













File Functions
GameManager.py
This is the main file which calls functions from every other file
Responsible for ensuring that player actions are validated
At every tick:
Updates player and projectiles positions
Manages various player parameters
Appends relevant information to json output files for Unity side

playerActions.py
This is where the logic for skills and actions are
When an action is input, the action is passed to the relevant function in playerActions
Then updates to player HP, block, knockback, stun etc is calculated and returned

Skills.py
Contains the Skills superclass and various skills subclasses
Various skill parameters such as damage, cooldown, range are set here
Important subclass: AttackSkill
It is the superclass for all damaging skills and projectiles
projectiles.py
Auxiliary file to Skills.py
Contains subclasses for various projectile skills 
Contains the ProjectileSkill superclass

test.py
Contains functions that correct player position, validate movement and player orientation
Also contains a function to print player info for debugging

turnUpdates.py
Contains functions that are called every turn
Such as cooldown management, midair movement management, projectile movement and buff duration management
Also includes json writing functions for the Unity side

PlayerConfigs.py
Contains various parameters for a player character, such as HP, position, list of moves, stun etc
Also contains a range of get functions that return said parameters

Player1.py and Player2.py
This is where the user will write their bots.

Use this to initialize your primary and secondary skills
For example, Teleport and Hadoken are chosen to be the skills
PRIMARY_SKILL = TeleportSkill
SECONDARY_SKILL = Hadoken

Then this function will be called by the GameManager to initialize the player’s skills.
def init_player_skills():
    return PRIMARY_SKILL, SECONDARY_SKILL

This is the main function, which will be called every turn by the GameManager, to get the next move from the player
def get_move(player, enemy, player_projectiles, enemy_projectiles):

Information about the player, enemy, player and enemy projectiles (if any) are passed to the function from the GameManager.
The user should use information from these sources to build conditionals that allow them to choose the next best move. This information can be fetched from the get functions mentioned in the Parameters section.

NOTE: init_player_skills must NOT be modified, and the file MUST have the get_move function.
Additional functions can be defined, just ensure that get_move returns an action.

TLDR:
Set primary and secondary skills
Make sure init_player_skills and get_move functions both exist and parameters unchanged
Make sure get_move returns an action


Priority
Orders of actions
Non-damaging actions, such as movement, blocking and non-damaging skills, take effect first, allowing for characters to dodge or parry attacks.
Then actions or skills that do damage, including projectiles, take effect afterwards
Then finally, all currently existing projectiles move one step along their paths.
In other words:
Movement/block -> buffing skills -> attack -> damage Skill
You can always block or jump away from attacks 



Abilities
Moves to be selected by players - Players can select 1 primary and 1 secondary ability

Key terms:

Startup: The number of startup frames
Cooldown: Number of cooldown frames
Damage: Damage dealt
Horizontal Range: how many squares the ability will affect
Vertical Range: 10
Blockable: Boolean of whether ability can be blocked
Knockback: Horizontal distance enemy is moved upon being hit
Stun: How many stun frames enemy is stunned for upon being hit by ability
Travel Range: Range a projectile can travel before it disappears








Primary abilities
There are 2 types of primary abilities - “non-damaging” and “damaging”.

Non-damaging abilities:

Skill: Teleport
Description: 
Instantly moves the character a certain distance (5 units) away from the enemy
Startup: 0
Cooldown: 10
Damage: 10
Horizontal Range: 10
Vertical Range: 0
Blockable: False
Knockback: 2
Stun: 2


Skill: Super Saiyan
Description: 
Increases the speed of the character, make their attacks deal more damage but slows them down after the buff ends for a certain period of time
Super Saiyan State: 
Double attack strength
Duration: 5 ticks
Slowed-down state: 
Half attack strength
All attacks now have 1 tick startup 
Duration: 5 ticks

Startup: 0
Cooldown: 15
Damage: N/A
Horizontal Range: N/A
Vertical Range: N/A
Blockable: N/A
Knockback: N/A
Stun: N/A


Skill: Super Armour
Description: Player gains armour which makes players take less damage and makes player invulnerable to stun.
Startup: 0
Cooldown: 30
Damage: N/A
Horizontal Range: N/A
Vertical Range: N/A
Blockable: N/A
Knockback: N/A
Stun: N/A


Skill: Super Jump
Description: Allows Player to jump higher
Startup: 0
Cooldown: 30
Damage: N/A
Horizontal Range: N/A
Vertical Range: N/A
Blockable: N/A
Knockback: N/A
Stun: N/A


Skill: Meditate
Description:
Heals character for 10 HP.
Startup: 0
Cooldown: 20
Damage: N/A
Horizontal Range: N/A
Vertical Range: N/A
Blockable: N/A
Knockback: N/A
Stun: N/A



Damaging abilities

Skill: Dash Attack
Description: 
Instantly moves the character a certain distance in the direction towards the enemy, dealing medium damage if the dash hits the enemy.
Startup: 0
Cooldown: 10
Damage: 10
Horizontal Range: 4
Vertical Range: 0
Blockable: False
Knockback: 0
Stun: 2


Skill: Uppercut
Description: 
Medium damage attack that hits airborne enemies.
Startup: 0
Cooldown: 10
Damage: 15
Horizontal Range: 1
Vertical Range: 2
Blockable: True
Knockback: 2
Stun: 2



Skill: One Punch
Description: 
Slow, heavy damage attack that breaks shields with great knockback and stun
Startup: 0
Cooldown: 10
Damage: 20
Horizontal Range: 1
Vertical Range: 0
Blockable: False
Knockback: 4
Stun: 3






Secondary abilities

Skill: Hadoken
Description:
Basic projectile that does damage if it hits an enemy along its path
Startup: 0
Cooldown: 10
Damage: 10
Travel Range: 5
Vertical Range: 0
Blockable: True
Knockback: 2
Stun: 2



Skill: Boomerang
Description:
Travels forwards, then back towards the character who casted it, dealing weak damage to the enemy upon being hit.
Startup: 0
Cooldown: 10
Damage: 5
Travel Range: 5
Vertical Range: 0
Blockable: True
Knockback: 2
Stun: 2


Skill: Grenade
Description:
Initially travels upwards in an arc, exploding and dealing damage at the end of the arc
Travel path: 3 x-coord and 1 y-coord from cast position
Explodes after 3 ticks and damages in an area 1 x-coord around it
Startup: 0
Cooldown: 10
Damage: 10
Travel Range: 5
Vertical Range: 0
Blockable: False
Knockback: 3
Stun: 3
Time-to-live: 3





Skill: Bear Trap
Description:
Travels forward a short distance and stays at that position, deals damage to the enemy if they step on it after it travels (2 ticks of travel).
Travel range: 2 x-coords from cast position
Startup: 0
Cooldown: 10
Damage: 10
Travel Range: 5
Vertical Range: 0
Blockable: False
Knockback: 0
Stun: 3
Time-to-live: 5





Visualizing your code
Ensure that player.py and player2.py has the correct code
Run GameManager.py
There should be a json file outputted 
Move the json files to “Hackethon_FrontEnd_Data/Json_Files”
Run the hackethon_frontend

