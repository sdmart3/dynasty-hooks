# Dynasty Hooks for College Football 27

A DLL that hooks the game's dynasty decisions (how CPU teams develop players, who enters the portal,
who redshirts, what coaches buy, how a game resumes, when Super Sim is offered, whether play history keeps counting, which plays the coach suggests, which abilities play at Heisman, and more) and a settings tool to switch each
hook on or off. Offline dynasties only; built and tested on the 22 September 2026 game build and the
1 October 2026 title update.

**Everything ships off.** With nothing switched on, a game launched under it is vanilla. (The one switch
that ships on, `patch_safe`, only decides HOW a hook you turn on is written into the game; it changes nothing
by itself.) Switch on what you want in the settings tool; your choices live in your `autoprogress.ini` and are kept
when you update (copy the new files over the old ones except that ini and your own `playcall-rules.txt` /
`playcall-rules-wholebook.txt` if you edited them; the DLL treats a key that is missing from an older ini as off).

## Start here

1. Unzip anywhere (a folder in Documents is fine) and keep the files together. **Required:** add the
   folder as an antivirus exclusion before the first launch (see "Antivirus" below). Otherwise Windows
   Security removes `inject.exe` the first time it runs and nothing loads.
2. Double-click `Start AutoProgress.cmd`. A console window waits for the game, loads the DLL about
   30 seconds after the game appears, and keeps waiting for the next launch. Leave it open while you
   play. A launch without it is vanilla.
   **Or let the MMC Mod Manager load it:** if you launch the game from the Mod Manager, the Mod Manager
   plugin (a separate download, `DynastyHooks-ModManagerPlugin-*.zip`) does this step for you every time you
   press Launch, and Start AutoProgress is not needed. See "Loading from the Mod Manager" below.
3. The settings tool is a separate download on the release page (`DynastyHooks-Settings-*.zip`,
   about 100 MB because it carries its own browser runtime): unzip it so that the `Dynasty Hooks
   Settings` folder sits inside this folder, then open `Dynasty Hooks Settings.exe`. Its Guide tab is
   this text; the other tabs are the features, one card each, with their switches.
   Change what you want and press "Save to autoprogress.ini". The game reads the ini when it
   launches, so save before you start it.
4. Launch the game. The tool's header shows "game running", and each card shows whether its hook
   installed in that session. `autoprogress.log` next to the DLL has the detail.

The ini works on its own too: every key is documented in the file, and the tool only edits values in
place, so hand edits survive.

## The hooks

Each is one card in the tool with a master switch and a few dials, and each writes a line to the log
saying what it did. Every switch ships off. "Shipped" means measured over seasons, or, for a flow
feature with nothing to measure (resume, cadence), exercised end to end in game; "proven" means a
probe and the change were confirmed in a test dynasty but not measured over many seasons, so treat a
season with one on as your own measurement. The maths and the measurements behind the shipped four
are in `AutoProgress-Method.html` in this folder (also at https://sdmart3.github.io/dynasty-hooks/).

| Hook | master switch | status | what it does when on |
| --- | --- | --- | --- |
| Resume a game, set up a situation | `resume_save`, `resume_force` | shipped | record a game's situation at every snap, edit it or type your own, and have the next game put into it at its first snap |
| CPU skill-point spending | `mode` (`weighted`) | shipped | the game picks a random affordable skill group; with this a CPU player's points go to the groups that raise his overall and on-field value: 4.6 overall per player's offseason points against 2.3 for the random pick (4.8 for a perfect picker) |
| Development-trait falls | `regress` | shipped | a Star or Elite player with a poor season can drop a tier by the same roll the game uses to raise one; about 50 falls a season against 100 to 150 rises |
| Development Spread | `spread` | shipped | every rostered player's ceilings are re-rolled once a year around players like him, about 8% bust, and the freed levels fund the breakouts (the Dynasty Development app's roster pass, inside the game; originals remembered in `spread-baseline.tsv`) |
| Program quality | `spread_bias` (with `spread`) | shipped | coach, prestige, facilities and upgrades score every program and nudge its players' ceiling targets; top-quarter programs grew players about 1.3 overall more than bottom-quarter ones over three seasons |
| Dynasty Auto Cadence | `cadence_force` | shipped | the alternate cadences a config mod enables in Play Now work in Dynasty games too |
| Super Sim anytime | `supersim_anytime` | proven | the pause menu offers Super Sim when you pause at the line, not only on the play-call screen (for auto-playcalling) |
| Play History fix | `playhistory_fix` | proven | the play-call screen's times called and yards per call keep counting for every play instead of resetting each time the game loads (about half the plays in stock and custom books) |
| Play History per dynasty / season / game | `phmodes` | proven (per season, per game); per dynasty experimental | the play-call screen's times called and yards per call count only this dynasty, this season or this game instead of your lifetime total; the game's own lifetime history is kept |
| Press A to skip | `a_skip` | proven | one tap of A ends a non-football scene (pregame intro, the ref's penalty announcement, cutaways, celebrations, replays, drive starters, timeouts, quarter and halftime breaks, the end-of-game scenes); never at the line, in a play, on the play-call screen or on a menu |
| Speed-up behind the play-call screen | `cutscene_speed_behind_playcall` | proven | the cutaways and crowd shots that keep playing behind the play-call screen run this many times faster (55 recommended) |
| Heisman abilities | `heisman_player1..64`, `heisman_rank5` | proven | chosen players' abilities play at the Heisman tier in every game, hot or cold (the game normally reaches Heisman only while a Platinum player is hot); pick them, or a whole team, on the settings tool's Player Abilities tab |
| Coach suggestions from your whole playbook | `wholebook_suggest` | proven | your offense's coach suggestions come from every play in your book, scored for the snap, with your gameplan still leading |
| Play-call rules | `playcall_rules` | proven | a rules file nudges the suggestions by down, distance, field position, clock and score ('on 3rd and long, deep passes x5') |
| Transfer portal | `portal_gate` | proven | scale every player's chance to enter the portal (`portal_scale`) and cap it per player (`portal_cap`): vanilla 47% of evaluated players leave; scale 0.5 gave 21%, cap 50 gave 27% |
| Skill-cap raises | `cap_raise_gate` | proven | give every program some ceiling growth (`cap_raise_floor`, 3% a roll) where the game only grows ceilings through a coach talent 32 of 138 programs carry: 3,074 raises on 2,470 players a season against 602 vanilla |
| Coach-quality spending | `coach_gate` | proven | how sharply a CPU team spends its players' points follows its program rank instead of one league-wide setting |
| XP by program quality | `xp_gate` | proven | each CPU team's offseason skill-point award is scaled by its program rank, 0.85x at the weakest to 1.15x at the strongest; your team only with `xp_user = 1` |
| Early NFL declarations | `nfl_gate` | proven | scale and cap how often underclassmen declare for the draft |
| Coach talent purchases | `talent_gate` | proven | reorder what a CPU coach buys first: the CPU picks in shopping-list order, so the order is the whole decision |
| Physical ability tiers | `abilitytier_gate` | proven | how often the CPU buys an ability upgrade (Bronze to Platinum) with leftover skill points |
| Mental abilities | `mental_gate` | proven | the game never moves a mental ability after generation; this rule lets a filled slot rise or fall a rank once a season by the season score, dev trait and program, and adds a Bronze to an empty slot now and then, keeping the league's shape steady (28 dials) |
| Practice injuries | `injury_gate` | proven | scale and cap the weekly practice-injury chance |
| CPU redshirting | `cpu_redshirt_gate` | proven (write) | CPU teams redshirt 8-12 deep-bench freshmen at the preseason step, which the game then treats as real redshirts at season end (class year kept); whether they also sit out the season is not yet proven |

## Resume a game, or set up any situation

The DLL can put a game into a saved situation at its first snap. The situation is a small text file,
`resume-last.ini` next to the DLL, which the DLL writes while you play or which you build in the
settings tool's resume panel (Flow tab). Any numbers work.

1. **Record.** Press "Record games" in the panel (`resume_save = 1`), save the ini, play. At every snap
   the DLL rewrites the file, so the last snap before you quit is the situation. Quit to the hub
   from the pause menu: in Dynasty the game stays unplayed and can be started again. After an in-game
   sim, let the offense line up once before you quit.
2. **Edit, if you want.** Load the last recorded game into the panel or start blank: quarter, clock,
   both scores, which side gets the ball, down, ball spot (a side and a yard line), yards to go or
   "goal", timeouts. Blank fields are left as the live game has them. "Save the situation file".
3. **Arm.** "Arm: live" (`resume_force = 1`), save the ini, launch, start the game (teams, stadium and
   weather come from the game you launch). The write happens at the first snap from scrimmage in the
   saved half where the chosen side has the ball, so a 3rd- or 4th-quarter situation waits until you
   sim into the second half, and who receives the kickoff does not matter. The arm is spent after one
   attempt; the DLL sets `resume_force` back to 0 itself. "Arm: dry run" logs the writes and touches
   nothing.
4. **Play on.** Scores add to the restored score, possession changes, quarters end into the half as
   normal. The pause menu's summary shows the old game until halftime, then catches up.

Proven in game on 23 September 2026: the recorded file matched the HUD on every field; the live write
put a game at 3-0, 2nd quarter, 5:00, 2nd and 3 at the 11 at the first snap, a touchdown added to it,
halftime ran, the third quarter kicked off at 23-0; a situation typed in the panel (4th quarter, 2:20,
55-0, 2nd and 5 at own 45) was written at the first third-quarter snap. Not restored: the drive log,
box-score stats, injuries, momentum, the play clock. Overtime situations are refused.

## Dynasty Auto Cadence: alternate cadences in Dynasty games

`cadence_force = 1` makes the alternate cadences (the ones a config mod enables in Play Now) work in
Dynasty games too: the DLL sets the allow flag at every pre-play. Proven in game 2026-09-22. Off by
default; on the Flow tab. The same feature ships on its own as a separate zip for people who want
only that.

## Super Sim anytime

The pause menu only offers Super Sim on the play-call screen, so with an auto-playcalling mod (or any
time you skip play call) you never get the chance. `supersim_anytime = 1` makes the pause menu offer it
when you pause while lined up before the snap as well. It changes one answer in the game's own "is
Super Sim allowed?" check, after that check's other rules (online games, game mode) have run, and
picking the tile runs the game's own Super Sim with all of its options. `supersim_anytime_states`
chooses the moments: `1` = at the line (tested), `1,4` = also right after the whistle (untested).
Proven in game 2026-09-24 with an auto-playcalling mod on. Off by default; on the Flow tab.

## Play History fix

The play-call screen keeps a running total for every play: times called and yards per call. For about
half the plays in the game, stock and custom playbooks alike, it never builds up: the game saves those
plays under a cut-down id, cannot find them after a reload, and starts them from zero every time.
`playhistory_fix = 1` makes the game save the full id (a 4-byte change to the code that saves your
profile; nothing in your dynasty save changes). History already lost stays lost; counting starts from
the first game played with it on. Proven in game 2026-09-26. Off by default; on the Flow tab.

## Play History per dynasty / season / game

The play-call screen's times called and yards per call normally count every game you have ever played. `phmodes`
picks what they count instead: `0` lifetime (the game's own numbers, the default), `1` this dynasty, `2` this
season, `3` this game. Only games you play to the final whistle are added to a season or a dynasty; a game you quit
is dropped (unless you continue it with the resume settings, then it counts once it is finished). In Play Now,
per dynasty and per season show the current game. The game's own lifetime history is not touched, so going back to
`0` shows it again. The totals live in a `playhistory` folder next to the DLL, one file per dynasty; an existing
dynasty starts at zero. Change it with the game closed. Turn on the Play History fix too. Proven in game 2026-10-01.
Off by default; on the Flow tab.

## Press A to skip, and the speed-up behind the play-call screen

Both are on the settings tool's **Flow** tab, card "Press A to skip". Both ship OFF.

- **Press A to skip** (`a_skip = 1`). Tap A during a scene that is not football and it ends: the pregame
  intro, the ref announcing a penalty, a cutaway or celebration, a replay after a score, a drive starter, a
  timeout, the quarter or halftime break, and the end-of-game scenes. `a_skip_groups = all` (the default) covers
  all of those; to leave the end-of-game scenes alone, list the groups you want instead (every group in `all`
  except `postgame`; the ini lists them). Your A still reaches the game as always. Nothing is skipped at the
  line, during a play, on the play-call screen or on any menu (penalty accept / decline, injury decision, coin
  toss). When the play-call screen or a menu comes next, the skip is quiet: the scene just ends, with no fade
  and no team-logo wipe. Proven in game 2026-10-01 (no loading screens, no flashes, accept / decline worked).
- **Speed-up behind the play-call screen** (`cutscene_speed_behind_playcall`, recommended `55`). Cutaways, the
  coach's reaction to a flag and the crowd shots keep playing behind the play-call screen, where A picks your
  play, so A cannot skip them. This runs them that many times faster while the screen is up (at most one extra
  second of scene per frame; at 30 fps, 55 acts like about 31). Your play call is not touched. Proven in game
  2026-09-30.

Turn on both for the full effect: `a_skip = 1` and `cutscene_speed_behind_playcall = 55`.

## Loading from the Mod Manager (plugin)

If you start the game with the MMC Mod Manager's Launch button (1.1.0.6 or 1.1.0.5), the Mod Manager plugin
loads Dynasty Hooks for you; you no longer need the Start AutoProgress window. Download
`DynastyHooks-ModManagerPlugin-*.zip`, close the Mod Manager, and follow its `README.txt`. Two setups:

- **A. You already have this Dynasty Hooks folder.** Copy the zip's `Plugins` folder into the Mod Manager
  folder (merge it), then set `hooks_dir` in `Plugins\DynastyHooks\plugin.cfg` to this folder. The plugin
  loads THIS folder's DLL with THIS folder's `autoprogress.ini`, so the settings tool keeps working on the same
  file. Close Start AutoProgress and don't use it any more (the plugin stands aside while it runs).
- **B. You only want the plugin.** The same zip carries its own DLL, ini and `inject.exe`. Copy its `Plugins`
  folder into the Mod Manager folder and leave `hooks_dir` empty; your settings file is then
  `Plugins\DynastyHooks\autoprogress.ini`.

**Required for both, before the first launch:** an antivirus exclusion for the folder the hooks load from:
`Plugins\DynastyHooks` in the Mod Manager folder (setup B), or this Dynasty Hooks folder (setup A). Without it,
Windows Security removes `inject.exe` the first time it runs, and `plugin.log` then says
`...\inject.exe is missing; not loading the hooks` at every launch. See "Antivirus" below.

`Plugins\DynastyHooks\plugin.log` says what the plugin did at each launch; `autoprogress.log` (next to the
ini that applies) has the hooks' own lines. Turn it off with `plugin_enabled = 0` in `plugin.cfg`; remove it by
deleting `Plugins\DynastyHooksPlugin.dll` and `Plugins\DynastyHooks`. Setup A is proven in game
(2026-10-01); setup B is new in this release.

## Experimental switches (off, not yet proven in a game)

These are built and self-tested, and their cards say EXPERIMENTAL. Leave them off unless you want to try one
on a copy of a save: per-dynasty play history (`phmodes = 1`), the general cutscene speed (`cutscene_speed`,
every scene; only its read-only probe has run), bowl practices (`bowl_xp`), coach XP speed
(`coachxp_cpu_scale` / `coachxp_user_scale`), the auto play-calling switch (`auto_playcall`), resume-a-game
stats and injuries (`resume_stats_merge`), Advance N weeks (`advance_weeks`), and the read-only diagnostics
(`db_probe`, `playbook_handle_probe`, `advance_probe`, `hook_timing`).

## Heisman abilities

In the game a player's ability only reaches the Heisman tier while he is hot on a Platinum ability, and it
drops back when he cools. This keeps chosen players' abilities at Heisman for the whole game, hot or cold.

- Open the settings tool, **Player Abilities** tab. Press **+ Add player**, type the name as it appears in the
  game, pick his player type and tick the abilities (or **All his abilities**). Save, then load a game.
- The tab also lists every player type and its five abilities, straight from the game's own tables.
- It raises abilities the player already has; it cannot give him new ones. Mental abilities are not changed.
- It applies to both teams (a CPU player with the same name is lifted too) and to every game from the next
  load. Your save is not changed: his in-game ability card shows Heisman, while the dynasty menus keep
  showing the tier he really has.
- **+ Add whole team** does a whole roster instead: pick the school, and every player on it (anyone who
  joins later too) gets all the physical abilities he has earned at Heisman. A player with no abilities yet
  gets nothing, because the game only loads abilities that are at least Bronze.
- Up to 64 rows; a whole team counts as one.

## Coach suggestions and play-call rules

Your offense's coach suggestions normally come only from your playbook's gameplan for the situation: about 20
plays, often all one kind. Two switches on the settings tool's **Play Calling** tab change that (offline games,
your offense only; your defense, special teams and the CPU's play calling are untouched):

- **Coach suggestions from your whole playbook** (`wholebook_suggest`): the list is built from every play in your
  book and scored for the snap. Your gameplan still leads, the rest of the book fills in with plays that fit the
  down, distance, field, clock and score, and a play you just called shows up less for a few snaps. Kicks, spikes,
  kneels and goal-line sets on normal downs are left out. Needs Coach Suggestions ON in the game settings.
- **Play-call rules** (`playcall_rules`): `playcall-rules.txt` next to the DLL holds one rule per line, e.g.
  'on 3rd and 7 or longer, deep passes x5'. The file is re-read the moment you save it, even mid-game. It works
  best together with the whole-playbook suggestions; `playcall-rules-wholebook.txt` is a draft rulebook of 25
  gentle college-football rules to start from. The grammar is in the file headers.

## Read-only probes (for the curious)

Recruiting (the weekly board, interest and pitches, commitments), cut day, the coach carousel and
scheme conversions have `*_probe` switches that log what the game decides without changing anything,
summarised by default. They exist so that the next hooks can be designed from real numbers; turning
one on costs log lines, nothing else. Leave the `*_dump` and per-line `*_log` keys at 0 in a season
you care about the sim speed of.

## Antivirus

**Windows Security and most scanners quarantine `inject.exe`**, on sight or the first time it runs
(in testing: `Behavior:Win32/DefenseEvasion.A!ml`, 4 seconds after it loaded the hooks). Loading a DLL into
another program is the whole job of this tool, and some malware does the same thing, so antivirus treats it
as suspicious behaviour. There is nothing else in it: the source is `src/inject.c` in the repository, about
120 lines. `autoprogress.dll` has not been flagged in testing; the same exclusion covers it.

**Add the exclusion before the first launch (required):**

1. Windows Security > **Virus & threat protection** > **Manage settings** (under "Virus & threat protection
   settings") > **Exclusions** > **Add or remove exclusions**.
2. **Add an exclusion** > **Folder**, and pick the folder `inject.exe` runs from: this Dynasty Hooks folder
   (Start AutoProgress, or the Mod Manager plugin's setup A), or `Plugins\DynastyHooks` in the Mod Manager
   folder (the plugin's setup B). Doing it before you unzip is best.

**If `inject.exe` is already gone:** add the exclusion, then restore it (Windows Security > Virus & threat
protection > **Protection history**, the `inject.exe` entry, **Actions** > **Allow** or **Restore**) or
extract it from the zip again into the same folder.

Injector options: `--watch` (keep running, inject every launch; what the .cmd runs), `--delay <sec>`
(default 30), `--poll <sec>` (default 2). It never injects twice into one game process. If it reports
`OpenProcess failed`, run the .cmd as administrator.

## Settings that matter (`autoprogress.ini`)

| key | default | set it to |
| --- | --- | --- |
| `mode` | off | `weighted` for the CPU spending change (the tested setting); `log` to run it with zero effect and only write the log (a first test) |
| `regress` | 0 | 1 for trait falls. **Leave 0 if you run the Dynasty Development app:** its Regression Tool already handles falls |
| `spread` | 0 | 1 for the Development Spread. **Leave 0 if you run the Dynasty Development app on this dynasty:** both writing ceilings would double-apply |
| `spread_fuel_rate` | 0.4 | with `spread = 1`: skill points handed back per ceiling level cut; 0.2 holds the league average flat |
| `spread_bias` | 1 | with `spread = 1`: 0 = program quality stops influencing ceilings (scores still logged) |
| `logpicks` | 1 | 0 for a small log (one summary line per 2,000 picks instead of one per pick) |
| `log_sync` | 0 | 1 = flush the log after every line (slower sims; only for chasing a crash) |

Everything else is documented in the file itself and in the tool. The dials under each master switch
ship at their tested values, so switching a master on gives the tested configuration.

## Things to know before you turn it on

* **Originals are remembered once, forever.** The first ceilings the DLL sees for a player are his
  originals, in `spread-baseline.tsv` next to the DLL. Keep that file with the DLL.
* **Several dynasties, one file, one ini.** Dynasties from the same base roster share their real
  players, so one baseline file serves all of them; the ini applies to every dynasty the game opens.
  A dynasty the Dynasty Development app has already stretched should get its own file
  (`spread_store = <name>.tsv`) or `spread = 0`.
* **Reloading a save and replaying Training Results** is fine: the spread lands on the same numbers.
* **Game updates.** The DLL finds the game's code by fingerprint and checks it byte by byte before
  patching. After an update it either still matches or logs and does nothing. The 22 September and
  1 October 2026 updates moved the code and every fingerprint still matched; with `patch_verify = 1` the
  bytes around each site are also compared with the hash recorded for that build before anything is written.
* **First try on a copy.** Copy the save, run one Training Results, read the `spread run 1:` line and
  your roster, then decide.
* **Your own team.** The defaults never touch the user team's progression screens. Where a hook
  could (XP scaling, the portal, mental abilities), a `*_user` key exists and ships 0.

## What the log tells you

* `pick ...`: every CPU skill-point spend, with what vanilla would have picked beside it.
* `devtrait row=... FELL`: a trait fall, with the season score and the roll.
* `spread run N: ...`: one line per Training Results with the whole pass summarised.
* `<feature> ... installed` / `<feature>: <key>=0, leaving ... alone`: per hook, once per launch. A
  `not patching` or `signature not found` line means the game build changed and that hook did nothing.
* `trampoline: page ... (+-N MB)`: where a hook's jump was placed. `trampoline: no free 64 KB slot within 2 GB`
  means the game had no room left near that hook this launch; it did nothing until the next launch.
* `resume force applied ...` / `resume force refused: <why>`: what a resume attempt did.
* `supersim_anytime: flow state 1 -> Super Sim offered`: the pause menu was given the Super Sim tile at the line.
* `playhistory_fix installed: Play History writer at ...`: the profile save now keeps full play ids.
* `phmodes: game #N START (...)` / `phmodes: game #N reached the final whistle: added ...`: what the play-call screen counts this game, and what was saved.
* `a_skip hook installed: a_skip=1 ...` once per launch; `a_skip SKIP ...` / `a_skip SKIP quiet ...` per skip; `a_skip summary: ...` every 10 minutes (with the speed-up's `sped_up=` count).

## Known limits

* Windows only, x64, Steam offline dynasties. A signed build is not planned yet.
* The player table is walked as 17,500 rows, which every save examined holds.
* The resume write restores the field, not the history: drives and box-score stats start empty at
  the restored point. Possession is waited for, never forced.
* CPU redshirting: the written status holds through the season and the game rolls it over like its
  own, but whether the player also sat out is unmeasured.
* The speed-up behind the play-call screen does not speed up the crowd shot when play resumes after a
  quarter break (another part of that scene holds it at normal speed).
* A-skip on the end-of-game scenes is new in this release; its in-game check is still pending.

Built 2026-10-02 (0.7.0) and checked against the 22 September and 1 October 2026 game builds.
