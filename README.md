If you want to download the scripts, please use this link: https://github.com/nPoseTeam/nPose-LM-LG-chains-plugin/releases/latest
The master branch may or may not contain the latest released version.

# nPose LM/LG chains plugin
Allows you to use Lockmeister and Lockgueard particle chains.  
based on `plugin_lockmeister_lockguard v0.01` written by xandrinex  
modified by pfil payne, Leona (slmember1 Resident)

## Setup example
### Chains in the main build
1.  Include the `nPose LM/LG chains plugin` script in the main build. Include the `nPose SAT/NOTSAT plugin` script in the main build (if it doesn't already exists)
2.  Add the chain points also to the build.
3.  Make the description of each chain point unique so they can be referred to in nPose notecard. In this example we use `leftloop` and `rightloop`.
4.  Make a `SET` card and use `SATMSG` (nPose V3.00 or older) or `ON_SIT` (nPose V3.10 or newer) for telling the plugin to send chains and where they should go when someone sits this seat.  
  `SATMSG` in this form: `SATMSG|2732|leftloop~lcuff~rightloop~rcuff` (nPose V3.00 or older)  
  `ON_SIT` in this form: `ON_SIT|seatnumber|CHAINS_ADD|leftloop~lcuff~rightloop~rcuff` (nPose V3.10 or newer)
    1. Replace "seatnumber" with the number of the seat
    2. The arb num 2732 is what the chains plugin is looking for and is interpreted as a command to send chains.
    3. The next is a list of chain point~cuff point matching pairs.  In the above `SATMSG` the pairs are as follows:  leftloop to lcuff, and rightloop to rcuff.
    4. Chains are drawn from the chain point to the designated cuff point (or vice versa). See references below for a list of cuff point names.
5. Add a `NOTSATMSG` (nPose V3.00 or older) or `ON_UNSIT` (nPose V3.10 or newer) to drop chains when this person stands or changes pose sets.  
  `NOTSATMSG` in this form: `NOTSATMSG|2733|leftloop~rightloop`(nPose V3.00 or older)  
  `ON_UNSIT` in this form `ON_UNSIT|seatnumber|CHAINS_REMOVE|leftloop~rightloop` (nPose V3.10 or newer)
    1. Replace "seatnumber" with the number of the seat
    2. The arb num 2733 is what the chains plugin is looking for and is interpreted as a command to stop chains.
    3. The next is a list of chain point.  In the above `NOTSATMSG` the plugin simply stops the chains at the chain points listed.

### Chains in Props
I didn't recommend using it with a nPose Version older than nPose V3.10.
1. Include the `nPose LM/LG chains plugin` script in each prop intended to be used for chain points along with the `nPose prop` plugin.
2. Add the chain points also to the props if not using the root prim of the prop as the chain point.
3. Make the description of each chain point unique so they can be referred to in nPose notecard. In this example we use `leftloop` and `rightloop`.
4. Make a `SET` card and use `SATMSG` (nPose V3.00 or older) or `ON_SIT` (nPose V3.10 or newer) for telling the plugin to send chains and where they should go when someone sits this seat.  
  `SATMSG` in this form: `SATMSG|2732|leftloop~lcuff~rightloop~rcuff` (nPose V3.00 or older)  
  `ON_SIT` in this form `ON_SIT|seatnumber|TIMER|chains|1|LINKMSG|2732|leftloop~lcuff~rightloop~rcuff` (nPose V3.10 or newer)
    1. Replace "seatnumber" with the number of the seat
    2. The arb num 2732 is what the chains plugin is looking for and is interpreted as a command to send chains.
    3. The next is a list of chain point~cuff point matching pairs. In the above `SATMSG` the pairs are as follows:  leftloop to lcuff, and rightloop to rcuff.
    4. Chains are drawn from the chain point to the designated cuff point (or vice versa). See references below for a list of cuff point names.
    5. The Timer is necessary to make sure that the Prop is fully rezzed before we are sending messages to it. You may change the name of the timer or the time.
5. Add a `NOTSATMSG` (nPose V3.00 or older) or `ON_UNSIT` (nPose V3.10 or newer) to drop chains when this person stands or changes pose sets.  
  `NOTSATMSG` in this form: `NOTSATMSG|2733|leftloop~rightloop`
  `ON_UNSIT` in this form `ON_UNSIT|seatnumber|LINKMSG|2733|leftloop~rightloop` (nPose V3.10 or newer)
    1. Replace "seatnumber" with the number of the seat
    2. The arb num 2733 is what the chains plugin is looking for and is interpreted as a command to stop chains.
    3. The next is a list of chain point.  In the above `NOTSATMSG` the plugin simply stops the chains at the chain points listed.
6. Be sure to add the appropriate `PROP` lines in the `SET` card to rez these chain point props.

### Particle Config
1. Add a LINKMSG line:  
`LINKMSG|2734|Parameters (separated by ~)` (nPose V3.00 or older)
`CHAINS_CONFIG|Parameters (separated by ~)` (nPose V3.10 or newer)

With Parameters:
- `texture=`particle texture as uuid (default value: 245ea72d-bc79-fee3-a802-8e73c0f09473 (metal chain))
- `xsize=`particle X size as float (0.03125 to 4.0) (default value: 0.07)
- `ysize=`particle Y size as float (0.03125 to 4.0) (default value: 0.07)
- `gravity=`particle gravity as float (default value: 0.03)
- `life=`particle life time as float in seconds (default value: 1.0)
- `red=` red part of the particle color as float (0 to 1) (default value: 1.0)
- `green=` green part of the particle color as float (0 to 1) (default value: 1.0)
- `blue=` blue part of the particle color as float (0 to 1) (default value: 1.0)

## Notes
1. This Plugin is expecting all Prim Descriptions to be unique. It will warn the owner when any are not unique. If the description isn't used as a Leash Point Name the warning can be ignored.
2. If you use nPose V3.10 or newer AND the `nPose LM/LG chains plugin` is placed into your main object, then `nPose LM/LG chains plugin` will register 3 new commands within nPose:
    - `CHAINS_ADD` (which can be used instead of `LINKMSG|2732`)
    - `CHAINS_REMOVE` (which can be used instead of `LINKMSG|2733`)
    - `CHAINS_CONFIG` (which can be used instead of `LINKMSG|2734`)

## References for cuff point names:
The script uses LockMeister cuff point names (Mooring Points) which can be found here:  
http://wiki.secondlife.com/wiki/LSL_Protocol/LockMeister_System  
The next link is only for your info, you probably will not need it:
https://web.archive.org/web/20130224185823/http://lslwiki.net/lslwiki/wakka.php?wakka=exchangeLockGuardItem

## History
`nPose LM/LG chains plugin v1.03` (upcoming version)
- fixed #2 (allow multiple chains to one Lockmeister mooring point) (not lockguard compatible)
- code cleanup

`nPose LM/LG chains plugin v1.02`
- fixed #1 (Chain is not working if the chain point is a single prim prop)
- slightly improved particle generator
- fixed some rare timing issues

`nPose LM/LG chains plugin v1.01`
- added CHAINS_ADD, CHAINS_REMOVE, CHAINS_CONFIG commands to be used with nPose V3.10 and newer

`nPose LM/LG chains plugin v0.04`
- allow the rootprim to be a leashpoint

`nPose LM/LG chains plugin v0.02`
- made the plugin usable for more than one victim
- added LMv2 compatibility
- scan for leashpoints (cuffs that are attached after the initial particle command will be recognized)
- added particle config

`plugin_lockmeister_lockguard v0.01`
- Removed some of the error reporting to allow this plugin to be used in prim chain points without errors.





# GERMAN
## Setup
### Ketten im Hauptobjekt
1. Füge das Skript `nPose LM/LG chains plugin` dem Hauptobjekt hinzu. Füge das Skript `nPose SAT/NOTSAT plugin` dem Hauptobjekt hinzu (falls noch nicht vorhanden).
2. Füge für jede Kette dem Hauptobjekt ein Prim hinzu (nachfolgend "chain point" genannt).
3. Jeder chain point braucht einen einmaligen Bezeichner, der in das Beschreibungsfeld des Prims einzutragen ist.
4. Erstelle eine `SET:` Notecard und und benutze den `SATMSG` (nPose V3.00 oder älter) bzw. `ON_SIT` (nPose V3.10 oder neuer) um dem script mitzuteilen von wo nach wo eine Kette gezogen werden soll. Beispiel:  
  `SATMSG|2732|leftloop~lcuff~rightloop~rcuff` (nPose V3.00 oder älter)  
  `ON_SIT|seatnumber|CHAINS_ADD|leftloop~lcuff~rightloop~rcuff` (nPose V3.10 oder neuer)
    1. Ersetze "seatnumber" mit der Nummer des gewünschten Sitzes
    2. Die Zahl 2732 sagt dem skript das es eine neue Kette anlegen soll
    3. der Text danach ist eine liste mit chain point ~ cuff point Namen, wobei wir den chain point Namen bereits durch das Eintragen im Beschreibungsfeld des chain points festgelegt haben (siehe Punkt 3.).
    4. Die cuff point Namen entnehmen wir der Referenz.
5. Füge der `SET:`NC eine weitere Zeile mit dem `NOTSAT` (nPose V3.00 oder älter) bzw. `ON_UNSIT` (nPose V3.10 oder neuer) hinzu. Beispiel:  
  `NOTSATMSG|2733|leftloop~rightloop` (nPose V3.00 oder älter)  
  `ON_UNSIT|seatnumber|CHAINS_REMOVE|leftloop~rightloop` (nPose V3.10 oder neuer)
    1. Ersetze "seatnumber" mit der Nummer des gewünschten Sitzes
    2. Die Zahl 2733 sagt dem plugin, dass es die Kette lösen soll
    3. Der Text ist eine Liste mit Chain points (die wir bereits in 3. erstellt haben).