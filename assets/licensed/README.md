# Licensed asset drop-in folder

Put officially licensed NPL assets here; the app picks them up without a code change.

- `players/<player_id>.jpg` — player photo. `player_id` is the id shown in the app URL
  (`/players/<player_id>`), e.g. `players/9e8bd87a.jpg`.
- `teams/<team_id>.png` — franchise logo. `team_id` is the slug in `/teams/<team_id>`,
  e.g. `teams/lumbini-lions.png`.

Resolution order in the app: local licensed file here -> Wikimedia Commons photo recorded
by the data agent (with attribution) -> initials avatar in franchise colours.

Record the licence for anything you add in `LICENCES.md` next to this file.
