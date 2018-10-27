# zombie Shield generator

1. First Download the file

2. Put the shield folder in your mission pbo root folder

3. Open your custom fn_selfactions.sqf and find:

if(_player_deleteBuild) then {
		if (s_player_deleteBuild < 0) then {
			s_player_deleteBuild = player addAction [format[localize "str_actions_delete",_text], "\z\addons\dayz_code\actions\remove.sqf",_cursorTarget, 1, true, true, "", ""];
		};
	} else {
		player removeAction s_player_deleteBuild;
		s_player_deleteBuild = -1;
	};
Then past this under it:

_lever = cursorTarget;

//Zombie Shield
	if (_cursorTarget isKindof TypeOfZShield) then {
		if (EnableZShield == 1) then {
			if (s_player_ZombieShield_on < 0) then {
				s_player_ZombieShield_on = player addAction [format[("<t color=""#FFF700"">" + ("Zombie Shield On") +"</t>"),_adminText], "shield\ZombieShield.sqf", [_lever, true], 6, true, true, "", ""];
			};
			if (s_player_ZombieShield_off < 0) then {
				s_player_ZombieShield_off = player addAction [format[("<t color=""#FFF700"">" + ("Zombie Shield Off") +"</t>"),_adminText], "shield\ZombieShield.sqf", [_lever, false], 6, false, true, "", ""];
			};
		} else {
			if (s_player_ZombieShield_on < 0) then {
				s_player_ZombieShield_on = player addAction [format[("<t color=""#FFF700"">" + ("Zombie Shields are disabled on this server") +"</t>"),_adminText], "", [], 6, false, true, "", ""];
			};
			player removeAction s_player_ZombieShield_off;
			s_player_ZombieShield_off = -1;
		};
	} else {
		player removeAction s_player_ZombieShield_on;
		s_player_ZombieShield_on = -1;
		player removeAction s_player_ZombieShield_off;
		s_player_ZombieShield_off = -1;
	};
//End Shield
4. Then find this line in fn_selfactions.sqf

if(DZE_AllowForceSave) then {
		//Allow player to force save
		if((_isVehicle or _isTent) and !_isMan) then {
			if (s_player_forceSave < 0) then {
				s_player_forceSave = player addAction [format[localize "str_actions_save",_text], "\z\addons\dayz_code\actions\forcesave.sqf",_cursorTarget, 1, true, true, "", ""];
			};
		} else {

***************Replace STUFF IN HERE
		
			};
	};
Where I put *******************Replace STUFF IN HERE, delete everything there and add this in its place:

			s_player_ZombieShield_on = -1;
			player removeAction s_player_ZombieShield_off;
			s_player_ZombieShield_off = -1;
			player removeAction s_player_forceSave;
			s_player_forceSave = -1;
5. Open your custom variable.sqf and paste this at the bottom, this is also where you can customize all your settings.

/**************************************************** Zombie Shield Generator Settings ****************************************************/
	EnableZShield				= 1; //Enable toggleable zombie shield generator/ 1 = Enabled // 0 = Disabled (If disabled, players can still build shield generators, they just wont do anything)
	TypeOfZShield				= "CDF_WarfareBUAVterminal"; //Type of object used for Zombie Shield, included this only in case some maps have this object banned
	AllZShieldTypes			= ["CDF_WarfareBUAVterminal"]; //DO NOT REMOVE ITEMS FROM THIS ARRAY, you can ADD an object class if you want a different building to be used as a Zombie Shield Generator!
	MaxZShields				= 1; //Maximum number of zombie shield generators a player can be added to, default is 1
	ZShieldRadius				= 50; //Radius for zombie shield generator, default is 50
	ZShieldClean				= 0; //Delete Zombies when they enter active shield radius/ 1 = Enabled // 0 = Disabled (If disabled, zombies will be killed but not deleted, could lead to zombie loot farming)
Close up, repack and done. Then any object CDF_WarfareBUAVterminal or anything you replace it with will then have a scroll option to turn a shield on and off. 
