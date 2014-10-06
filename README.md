# Hacker News API

## Overview

In partnership with [Firebase](https://www.firebase.com), we're making the public Hacker News data available in near real time. Firebase enables easy access from [Android](https://www.firebase.com/docs/android/), [iOS](https://www.firebase.com/docs/ios/) and the [web](https://www.firebase.com/docs/web/). Servers aren't left out; there's also [REST](https://www.firebase.com/docs/rest/) support.

## URI and Versioning

The API is based at https://hacker-news.firebaseio.com.

We hope to improve it over time, and may later enable access to private per-user data using OAuth. The changes won't always be backward compatible, so we're going to version the API. This first iteration will live at https://hacker-news.firebaseio.com/v0/ and is structured as described below.

## Items

Stories, comments, Ask HNs and even polls are just items. They're identified by their ids, which are unique integers, and live under https://hacker-news.firebaseio.com/v0/item/<id>.

All items have a subset of the following properties:

Field | Description
------|------------
id | The item's unique id. Required.
deleted | `true` if the item is deleted.
type | The type of item. One of "story", "comment", "poll", or "pollopt".
by | The username of the item's author.
time | Creation date of the item, in [Unix Time](http://en.wikipedia.org/wiki/Unix_time).
text | The comment, Ask HN, or poll text. HTML.
dead | `true` if the item is dead.
parent | The item's parent. For comments, either another comment or the relevant story. For pollopts, the relevant poll.
kids | The ids of the item's comments, in ranked display order.
url | The URL of the story.
score | The story's score, or the votes for a pollopt.
title | The title of the story or poll.
parts | A list of related pollopts, in display order.

For example, a story: https://hacker-news.firebaseio.com/v0/item/8863.json?print=pretty

```json
{
  "by" : "dhouston",
  "id" : 8863,
  "kids" : [ 8952, 9224, 8917, 8884, 8887, 8943, 8869, 8958, 9005, 9671, 8940, 9067, 8908, 9055, 8865, 8881, 8872, 8873, 8955, 10403, 8903, 8928, 9125, 8998, 8901, 8902, 8907, 8894, 8878, 8870, 8980, 8934, 8876 ],
  "score" : 111,
  "time" : 1175714200,
  "title" : "My YC app: Dropbox - Throw away your USB drive",
  "type" : "story",
  "url" : "http://www.getdropbox.com/u/2/screencast.html"
}
```

comment: https://hacker-news.firebaseio.com/v0/item/2921983.json?print=pretty

```json
{
  "by" : "norvig",
  "id" : 2921983,
  "kids" : [ 2922097, 2922429, 2924562, 2922709, 2922573, 2922140, 2922141 ],
  "parent" : 2921506,
  "text" : "Aw shucks, guys ... you make me blush with your compliments.<p>Tell you what, Ill make a deal: I'll keep writing if you keep reading. K?",
  "time" : 1314211127,
  "type" : "comment"
}
```

poll: https://hacker-news.firebaseio.com/v0/item/126809.json?print=pretty

```json
{
  "by" : "pg",
  "id" : 126809,
  "kids" : [ 126822, 126823, 126993, 126824, 126934, 127411, 126888, 127681, 126818, 126816, 126854, 127095, 126861, 127313, 127299, 126859, 126852, 126882, 126832, 127072, 127217, 126889, 127535, 126917, 126875 ],
  "parts" : [ 126810, 126811, 126812 ],
  "score" : 46,
  "text" : "",
  "time" : 1204403652,
  "title" : "Poll: What would happen if News.YC had explicit support for polls?",
  "type" : "poll"
}
```

and one of its parts: https://hacker-news.firebaseio.com/v0/item/160705.json?print=pretty

```json
{
  "by" : "pg",
  "id" : 160705,
  "parent" : 160704,
  "score" : 335,
  "text" : "Yes, ban them; I'm tired of seeing Valleywag stories on News.YC.",
  "time" : 1207886576,
  "type" : "pollopt"
}
```

## Users

Users are identified by case-sensitive ids, and live under https://hacker-news.firebaseio.com/v0/user/.

Field | Description
------|------------
id | The user's unique username. Case-sensitive. Required.
delay | Delay in minutes between a comment's creation and its visibility to other users.
created | Creation date of the user, in [Unix Time](http://en.wikipedia.org/wiki/Unix_time).
karma | The user's karma.
about | The user's optional self-description. HTML.
submitted | List of the user's stories, polls and comments.

For example: https://hacker-news.firebaseio.com/v0/user/jl.json?print=pretty

```json
{
  "about" : "This is a test",
  "created" : 1173923446,
  "delay" : 0,
  "id" : "jl",
  "karma" : 2937,
  "submitted" : [ 8265435, 8168423, 8090946, 8090326, 7699907, 7637962, 7596179, 7596163, 7594569, 7562135, 7562111, 7494708, 7494171, 7488093, 7444860, 7327817, 7280290, 7278694, 7097557, 7097546, 7097254, 7052857, 7039484, 6987273, 6649999, 6649706, 6629560, 6609127, 6327951, 6225810, 6111999, 5580079, 5112008, 4907948, 4901821, 4700469, 4678919, 3779193, 3711380, 3701405, 3627981, 3473004, 3473000, 3457006, 3422158, 3136701, 2943046, 2794646, 2482737, 2425640, 2411925, 2408077, 2407992, 2407940, 2278689, 2220295, 2144918, 2144852, 1875323, 1875295, 1857397, 1839737, 1809010, 1788048, 1780681, 1721745, 1676227, 1654023, 1651449, 1641019, 1631985, 1618759, 1522978, 1499641, 1441290, 1440993, 1436440, 1430510, 1430208, 1385525, 1384917, 1370453, 1346118, 1309968, 1305415, 1305037, 1276771, 1270981, 1233287, 1211456, 1210688, 1210682, 1194189, 1193914, 1191653, 1190766, 1190319, 1189925, 1188455, 1188177, 1185884, 1165649, 1164314, 1160048, 1159156, 1158865, 1150900, 1115326, 933897, 924482, 923918, 922804, 922280, 922168, 920332, 919803, 917871, 912867, 910426, 902506, 891171, 807902, 806254, 796618, 786286, 764412, 764325, 642566, 642564, 587821, 575744, 547504, 532055, 521067, 492164, 491979, 383935, 383933, 383930, 383927, 375462, 263479, 258389, 250751, 245140, 243472, 237445, 229393, 226797, 225536, 225483, 225426, 221084, 213940, 213342, 211238, 210099, 210007, 209913, 209908, 209904, 209903, 170904, 165850, 161566, 158388, 158305, 158294, 156235, 151097, 148566, 146948, 136968, 134656, 133455, 129765, 126740, 122101, 122100, 120867, 120492, 115999, 114492, 114304, 111730, 110980, 110451, 108420, 107165, 105150, 104735, 103188, 103187, 99902, 99282, 99122, 98972, 98417, 98416, 98231, 96007, 96005, 95623, 95487, 95475, 95471, 95467, 95326, 95322, 94952, 94681, 94679, 94678, 94420, 94419, 94393, 94149, 94008, 93490, 93489, 92944, 92247, 91713, 90162, 90091, 89844, 89678, 89498, 86953, 86109, 85244, 85195, 85194, 85193, 85192, 84955, 84629, 83902, 82918, 76393, 68677, 61565, 60542, 47745, 47744, 41098, 39153, 38678, 37741, 33469, 12897, 6746, 5252, 4752, 4586, 4289 ]
}
```

## Top Stories

The current top 100 stories are at https://hacker-news.firebaseio.com/v0/topstories.

Example: https://hacker-news.firebaseio.com/v0/topstories.json?print=pretty

```json
[ 8414149, 8414078, 8413972, 8411638, 8414102, 8413204, 8413100, 8413971, 8412744, 8414003, 8412841, 8412802, 8412605, 8413548, 8413123, 8414437, 8412897, 8413028, 8413341, 8412425, 8411762, 8413623, 8412346, 8411356, 8413056, 8413365, 8412372, 8414055, 8412877, 8412167, 8413264, 8414137, 8410519, 8412933, 8411846, 8412929, 8411254, 8411512, 8412777, 8412626, 8413274, 8414389, 8414117, 8412114, 8412212, 8412759, 8412696, 8412768, 8411643, 8411866, 8413966, 8410976, 8410545, 8410358, 8413979, 8414129, 8411791, 8409075, 8410314, 8411532, 8411553, 8412099, 8412085, 8410356, 8409084, 8412862, 8409823, 8412705, 8410220, 8409323, 8414090, 8410326, 8414206, 8411026, 8408298, 8407364, 8413066, 8412104, 8412235, 8412786, 8395689, 8414318, 8406384, 8414314, 8406507, 8408501, 8413630, 8414180, 8400778, 8413804, 8407298, 8413233, 8412601, 8411277, 8409940, 8414287, 8397750, 8412679, 8412727, 8413104 ]
```
