# Dynasty Hooks for College Football 27

A DLL that hooks the game's dynasty decisions (how CPU teams develop players, who enters the portal,
who redshirts, what coaches buy, how a game resumes, and more) and a settings tool to switch each
hook on or off. Offline dynasties only; built and tested on the 22 September 2026 game build.

**Everything ships off.** With nothing switched on, a game launched under it is vanilla. Switch on
what you want in the settings tool; your choices live in your `autoprogress.ini` and are kept
when you update (copy the new files over the old ones except that ini; the DLL treats a key that is
missing from an older ini as off).

## Start here

1. Unzip anywhere (a folder in Documents is fine) and keep the files together. Add the folder as an
   antivirus exclusion first: see "Antivirus" below, `inject.exe` will be quarantined otherwise.
2. Double-click `Start AutoProgress.cmd`. A console window waits for the game, loads the DLL about
   30 seconds after the game appears, and keeps waiting for the next launch. Leave it open while you
   play. A launch without it is vanilla.
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
are in `AutoProgress-Method.html` in this folder (also at https://sdmart3.github.io/cfb-27-autoprogress/).

| Hook | master switch | status | what it does when on |
| --- | --- | --- | --- |
| Resume a game, set up a situation | `resume_save`, `resume_force` | shipped | record a game's situation at every snap, edit it or type your own, and have the next game put into it at its first snap |
| CPU skill-point spending | `mode` (`weighted`) | shipped | the game picks a random affordable skill group; with this a CPU player's points go to the groups that raise his overall and on-field value: 4.6 overall per player's offseason points against 2.3 for the random pick (4.8 for a perfect picker) |
| Development-trait falls | `regress` | shipped | a Star or Elite player with a poor season can drop a tier by the same roll the game uses to raise one; about 50 falls a season against 100 to 150 rises |
| Development Spread | `spread` | shipped | every rostered player's ceilings are re-rolled once a year around players like him, about 8% bust, and the freed levels fund the breakouts (the Dynasty Development app's roster pass, inside the game; originals remembered in `spread-baseline.tsv`) |
| Program quality | `spread_bias` (with `spread`) | shipped | coach, prestige, facilities and upgrades score every program and nudge its players' ceiling targets; top-quarter programs grew players about 1.3 overall more than bottom-quarter ones over three seasons |
| Dynasty Auto Cadence | `cadence_force` | shipped | the alternate cadences a config mod enables in Play Now work in Dynasty games too |
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

## Read-only probes (for the curious)

Recruiting (the weekly board, interest and pitches, commitments), cut day, the coach carousel and
scheme conversions have `*_probe` switches that log what the game decides without changing anything,
summarised by default. They exist so that the next hooks can be designed from real numbers; turning
one on costs log lines, nothing else. Leave the `*_dump` and per-line `*_log` keys at 0 in a season
you care about the sim speed of.

## Antivirus

**Windows Security and most scanners quarantine `inject.exe` on sight**, because loading a DLL into
another program (the whole job of this tool) is a technique some malware uses. There is nothing else
in it: the source is `src/inject.c` in the repository, about 120 lines. Add the Dynasty Hooks folder
as an exclusion *before* unzipping (Virus & threat protection > Manage settings > Exclusions), or
restore the file from quarantine and re-extract it afterwards. `autoprogress.dll` has not been
flagged in testing; the same exclusion covers it.

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
  patching. After an update it either still matches or logs and does nothing. The 22 September 2026
  update moved the code and every fingerprint still matched.
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
* `resume force applied ...` / `resume force refused: <why>`: what a resume attempt did.

## Known limits

* Windows only, x64, Steam offline dynasties. A signed build is not planned yet.
* The player table is walked as 17,500 rows, which every save examined holds.
* The resume write restores the field, not the history: drives and box-score stats start empty at
  the restored point. Possession is waited for, never forced.
* CPU redshirting: the written status holds through the season and the game rolls it over like its
  own, but whether the player also sat out is unmeasured.

Built and tested 2026-09-23 on the 22 September 2026 game build.
