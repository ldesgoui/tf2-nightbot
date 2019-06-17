# TF2-related Nightbot commands

I fixed them PogU

## Usage

After [Nightbot has joined your room](https://beta.nightbot.tv/), simply call addcom/editcom to add the commands.

```
!addcom !logs $(eval ... )
```

Read the last few lines of each command to see where your custom identifiers should be placed.


## Get the 3 latest logs from [logs.tf](http://logs.tf/about#json)
`logs.tf/1765763 (7 hours ago) | logs.tf/1765710 (8 hours ago) | logs.tf/1765651 (9 hours ago)`
```javascript
!addcom !logs $(eval (function(api) {
  if (!api.success) { return "Query was unsuccessful"; }
  var hmnz = (ts)=>[1,60,3600,86400,604800,2419200,29030400].reduce((a,x,i)=>ts<x?a:`${ts/x|0} ${["second","minute","hour","day","week","month","year"][i]}${(ts/x|0)>1?"s":""}`,"less than a second");
  return api.logs.map((x) => `logs.tf/${x.id} (${hmnz(new Date()/1000 - x.date)} ago)`).join(" | ");
})(
  $(urlfetch json http://logs.tf/json_search?limit=5&player=76561198020242938)
))
```
(replace 76561198020242938 with your 64 bit steamid (steamid.io))


## Get map currently played on
Doesn't work if you're offline or private on Steam

```javascript
!addcom !map $(eval (function(api) {
  return (api.map === undefined ? "Map not found" : api.map);
})(
  $(urlfetch json https://us-central1-tf2-nightbot.cloudfunctions.net/ssq?host=$(steam twiikuu "{{gameServer}}"))
))
```
(replace twiikuu with your [customURL](https://steamid.io))


## Next [ETF2L](http://api.etf2l.org/#Team) official
`Week 6, unexpected vs Se7en on cp_snakewater_final1, cp_granary_pro_rc4 will be played on Sun, 02 Jul 2017 19:15:00 GMT`
```javascript
!addcom !offi $(eval (function(api) {
  if (api.status.code !== 200) { return api.status.message; }
  if (api.matches === null) { return "No upcoming fixtures"; }
  var m = api.matches[0];
  return `Week ${m.week}, ${m.clan1.name} vs ${m.clan2.name} on ${m.maps.join(', ')} will be played on ${new Date(m.time * 1000).toGMTString()}`;
})(
  $(urlfetch json http://api.etf2l.org/team/22474/matches.json)
))
```
(replace 22474 with your team's ID, the number at the end of etf2l.org/teams/XXXX)

## Random TF2 Tip (from the loading screen)

```javascript
!addcom !tip $(urlfetch https://tf2-tips.ldesgoui.workers.dev)
```
