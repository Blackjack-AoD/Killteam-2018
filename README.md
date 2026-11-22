# Warhammer 40,000: Kill Team (2018)

BattleScribe / New Recruit data for Kill Team (2018): the game system and 27 faction catalogues, recovered from
`BSData/wh40k-killteam` (final state before Kill Team 2021, 27 Jul 2021) after the BSData GitHub org was deleted in
November 2026.

- `bsdata-original` (tag): the files exactly as BSData left them.
- `main`: category-link clean-up, and a **Tactic** profile type for adding tactics (below).

## Adding tactics

BSData's files never included tactics. The game system now has a **Tactic** profile type
(`CP`, `Phase`, `Requires`, `Description`), so they can be added by hand from the books.
[Muster](https://github.com/Blackjack-AoD/Muster) shows them from each force's **Tactics** button
(the book icon): the game system's generic tactics first, then the faction's own. They're also on the army sheet and
in share links.

**Requires** (optional) limits a tactic to lists that hold something: the name of a unit or model
(`Tech-priest Dominus`), a selection such as a specialism (`Combat`), or a category (`Commander`). Give several
separated by commas or "or" (`Tech-priest Dominus or Archmagos`). Leave it out, or put `-`, for a tactic any list can
use. Muster lists only the tactics a list can use, with a switch to show the rest.

Where they go:

- **Generic tactics** (in the core rules, available to every faction): in `Warhammer 40,000 Kill Team.gst`, inside its
  existing `<sharedProfiles>`.
- **A faction's tactics**: in that faction's `.cat` file, inside its `<sharedProfiles>` (most files already have one,
  holding abilities). If a file has none, add `<sharedProfiles></sharedProfiles>` after its other `shared…` sections
  (`sharedSelectionEntries`, `sharedSelectionEntryGroups`, `sharedRules`) and before `sharedInfoGroups` or
  `catalogueLinks`, as the file format expects that order.

One tactic, to copy and fill in. Give each profile its own `id` (any unused `xxxx-xxxx-xxxx-xxxx` hex value); keep the
`typeId` values as they are, as they point at the Tactic type in the game system:

```xml
<profile id="0000-0000-0000-0001" name="Tactic name" hidden="false" typeId="f047-9443-8abf-4d07" typeName="Tactic">
  <characteristics>
    <characteristic name="CP" typeId="4437-4b63-0996-9977">1</characteristic>
    <characteristic name="Phase" typeId="51a1-bfc7-d6c9-3414">Shooting</characteristic>
    <characteristic name="Requires" typeId="4e37-d12d-8b6f-53ec">Combat</characteristic>
    <characteristic name="Description" typeId="03f9-a9e3-0178-543f">What the tactic does.</characteristic>
  </characteristics>
</profile>
```

Bump the file's `revision` attribute when you change it, so apps that cache data pick up the change.
BattleScribe and New Recruit only show a shared profile where an entry links to it, so tactics added this way show in
Muster, not in those apps.
