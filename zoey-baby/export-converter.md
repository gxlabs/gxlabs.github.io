---
layout: page
title: "Zoey Baby - Import format"
breadcrumb: "Import format"
parent: "Zoey Baby"
parent_url: /zoey-baby
---

<style>
  #content table { border-collapse: collapse; margin: 1rem 0; width: 100%; }
  #content table th, #content table td { border: 1px solid rgba(0,0,0,.12); padding: .5rem .75rem; text-align: left; vertical-align: top; }
  #content table th { background: rgba(23,151,99,.08); }
  #content h2 { margin-top: 2rem; }
  #content h3 { margin-top: 1.5rem; font-size: 1.15rem; }
  #content code { white-space: nowrap; }
  #content pre code { white-space: pre; }
</style>

Zoey Baby imports one thing: a ZIP holding a `data.json` file, the same
archive its own export writes. That is also how you bring history over
from another tracker. Turn its export into this ZIP and the app reads it
like one of its own backups.

Import from **Settings → Privacy & data → Import Zoey Baby ZIP**, or
from the Import button during first-run setup.

## What's in the ZIP

```
zoey-baby-family-20260918-113000.zip
└── zoey-baby-family-20260918-113000/
    ├── data.json
    └── images/
        ├── 1/                  the first child in data.json
        │   ├── child/1.jpg
        │   ├── feedings/1.jpg
        │   ├── diapers/12.jpg
        │   ├── pumping/3.jpg
        │   ├── growth/2.jpg
        │   ├── temperature/1.jpg
        │   └── medication/4.jpg
        └── 2/                  the second child, same shape
            └── feedings/1.jpg
```

`data.json` is the whole import. The single wrapping folder is what the
app's own export produces; putting `data.json` and `images/` at the top
of the ZIP works just as well.

An archive holds the whole family, so photos are filed per child:
`images/<n>/` where n is the child's position in `children`, counting
from 1. `images/` is optional, and so is any folder in it. Inside, a
photo is named for the `id` of the row it belongs to, in the folder
named for that row's key, so `images/2/feedings/12.jpg` is the photo on
the second child's feeding with `"id": 12`.

## data.json

A whole archive: a family of two, with a row or two under each key.

```json
{
  "exportedAt": "2026-09-18T11:30:00-04:00",
  "children": [
    {
      "child": {
        "id": "4E9C1B60-9E1E-4E6A-9C2F-6B0A1D9A77C1",
        "firstName": "Zoey",
        "lastName": "Robertson",
        "sex": "girls",
        "birthDate": "2026-03-02T08:14:00-05:00",
        "photoPath": "images/1/child/1.jpg"
      },
      "childName": "Zoey",
      "feedings": [
        {
          "id": 1,
          "start": "2026-09-18T07:57:00-04:00",
          "end": "2026-09-18T08:12:00-04:00",
          "type": "breast milk",
          "method": "bottle",
          "amount": 120,
          "notes": "sleepy"
        },
        {
          "id": 2,
          "start": "2026-09-18T11:05:00-04:00",
          "end": "2026-09-18T11:27:00-04:00",
          "type": "breast milk",
          "method": "both breasts",
          "leftSeconds": 780,
          "rightSeconds": 540
        }
      ],
      "diapers": [
        { "id": 1, "time": "2026-09-18T08:12:00-04:00", "wet": true, "solid": true, "color": "mustard" }
      ],
      "sleep": [
        { "day": "2026-09-18T00:00:00-04:00", "awakeSeconds": 31500, "asleepSeconds": 19620, "deepSeconds": 5130 }
      ],
      "naps": [
        { "start": "2026-09-18T08:37:31-04:00", "end": "2026-09-18T10:51:31-04:00", "manual": false }
      ],
      "pumping": [
        { "id": 1, "start": "2026-09-18T07:44:00-04:00", "end": "2026-09-18T08:04:00-04:00", "amount": 150, "leftMl": 90, "rightMl": 60 }
      ],
      "growth": [
        { "id": 1, "date": "2026-09-14T09:00:00-04:00", "metric": "weight", "valueMetric": 6.42 }
      ],
      "temperature": [
        { "id": 1, "time": "2026-09-17T21:10:00-04:00", "celsius": 37.4, "method": "armpit" }
      ],
      "medication": [
        { "id": 1, "time": "2026-09-17T21:20:00-04:00", "name": "Infant Tylenol", "doseAmount": 2.5, "doseUnit": "ml" }
      ]
    },
    {
      "child": { "firstName": "Wren", "sex": "boys", "birthDate": "2024-05-11T02:40:00-04:00" },
      "childName": "Wren",
      "feedings": [],
      "diapers": [
        { "id": 1, "time": "2026-09-18T09:20:00-04:00", "wet": true, "solid": false }
      ],
      "sleep": [],
      "pumping": []
    }
  ]
}
```

`children` is the archive. Everything else at the top level is the
app's own bookkeeping, and a converted archive can leave it out. Keys
the app doesn't know are ignored, so extra fields of your own are
harmless.

## Keys

The top level:

| Key | Required | Holds |
| --- | --- | --- |
| `children` | yes | One entry per child, each with their own records. |
| `preferences` | no | Units and which buttons are on. |
| `homeAssistant` | no | Sensor settings. |
| `exportedAt` | no | When the archive was written. |

Inside one entry of `children`:

| Key | Required | Holds |
| --- | --- | --- |
| `feedings` | yes | Bottles and breast feeds, and solids. |
| `diapers` | yes | Diaper changes. |
| `sleep` | yes | One row per day of awake / asleep / deep totals. |
| `pumping` | yes | Pumping sessions. |
| `naps` | no | Individual sleeps, which the timeline draws a row each. |
| `growth` | no | Weights, heights and head circumferences. |
| `temperature` | no | Temperature readings. |
| `medication` | no | Doses given. |
| `child` | no | Name, birth date, sex and profile photo. |
| `childName` | no | The same first name, for the import's own wording. |
| `sensors` | no | The Home Assistant entities this child's readings come from. |

Those four required keys have to be present on every child, even when
empty.

### Which child an entry lands on

A child in the archive is matched to a child in the family by `child.id`
first, so a family restoring its own backup puts every child's records
back where they came from, whatever they have since been renamed to.
Failing that, the name is matched. Failing that, the child is created
from the header, except for an archive of a single child that matched
nothing, which lands on whichever child is open: that is the restore
onto a fresh install, where the placeholder from setup is who the
records belong to.

A converted archive has no ids to offer, so its children are matched by
name or created. Give each one a `child` header with at least a
`firstName`.

Times are ISO 8601 with an offset (`2026-09-16T08:37:31-04:00`).
Volumes are millilitres, weight is kilograms, lengths are centimetres,
temperature is Celsius. The app converts to whatever units the phone is
set to. `id` is an integer unique within its own key, and its only job
is to attach a photo; numbering each one from 1 is fine.

### feedings

| Field | Required | Notes |
| --- | --- | --- |
| `id` | yes | Photo key. |
| `start`, `end` | yes | Equal for a bottle logged at a moment. |
| `type` | yes | `breast milk`, `formula`, `fortified breast milk`, `solid food` |
| `method` | yes | `bottle`, `left breast`, `right breast`, `both breasts`, `parent fed`, `self fed` |
| `amount` | no | Millilitres. Bottles only. |
| `leftSeconds`, `rightSeconds` | no | Time on each side of a breast feed. |
| `notes` | no | |

### diapers

| Field | Required | Notes |
| --- | --- | --- |
| `id` | yes | Photo key. |
| `time` | yes | |
| `wet`, `solid` | yes | Both true is a mixed diaper. |
| `blowout` | no | Defaults to false. |
| `color` | no | `black`, `brown`, `green`, `yellow`, `mustard` |
| `notes` | no | |

### sleep

One row per day, holding that day's totals. All four fields are
required, and `day` is local midnight.

| Field | Notes |
| --- | --- |
| `day` | Local midnight of the day the totals cover. |
| `awakeSeconds`, `asleepSeconds`, `deepSeconds` | Deep is counted inside asleep, not beside it. |

### naps

Individual sleeps, which are what the timeline draws a row per. Leave
the section out if the other app only kept daily totals.

| Field | Required | Notes |
| --- | --- | --- |
| `start` | yes | |
| `end` | no | Null means still running, so leave a real end on every historical nap. |
| `manual` | yes | True for a nap a person logged; a sensor never moves those afterwards. |
| `notes` | no | |

### pumping

| Field | Required | Notes |
| --- | --- | --- |
| `id` | yes | Photo key. |
| `start`, `end` | yes | |
| `amount` | no | Millilitres, both sides together. |
| `leftMl`, `rightMl` | no | Per-side split, if the other app kept one. |
| `notes` | no | |

### growth

| Field | Required | Notes |
| --- | --- | --- |
| `id` | yes | Photo key. |
| `date` | yes | |
| `metric` | yes | `weight`, `height`, `headCircumference` |
| `valueMetric` | yes | Kilograms for weight, centimetres for the other two. |
| `notes` | no | |

### temperature

| Field | Required | Notes |
| --- | --- | --- |
| `id` | yes | Photo key. |
| `time`, `celsius` | yes | A row missing either is skipped, since there is no reading to keep. |
| `method` | no | `forehead`, `ear`, `armpit`, `oral`, `rectal` |
| `notes` | no | |

### medication

| Field | Required | Notes |
| --- | --- | --- |
| `id` | yes | Photo key. |
| `time`, `name` | yes | A row missing either is skipped. |
| `doseAmount` | no | |
| `doseUnit` | no | `ml`, `mg`, `drops`, `tablet` |
| `notes` | no | |

### child

Who the entry's records belong to. Every field is optional, but without
a `firstName` there is nothing to match on and nothing to name a minted
child after.

| Field | Notes |
| --- | --- |
| `id` | The child's id in the family that wrote the archive. Match key, see above. |
| `firstName`, `lastName` | |
| `sex` | `girls` or `boys`, which picks the WHO growth curve. |
| `birthDate` | |
| `photoPath` | Path inside the ZIP, `images/<n>/child/1.jpg` by convention. |

### sensors

Which sock and camera are this child's. The connection they are read
through is the family's, under `homeAssistant`. Written by the app's
own export; a converted archive can leave it out.

| Field | Notes |
| --- | --- |
| `sleepStateEntity`, `cameraSleepEntity` | The two sleep sources. |
| `heartRateEntity`, `oxygenEntity` | Vitals. |
| `chargingEntity`, `batteryEntity` | The sock's own state. |
| `trackSleep` | Whether this child's sensor history is rolled up into daily totals. |

### preferences

What the app's own export saves so a restore doesn't reset your
settings. Leave the whole key out of a converted archive. Every field
is optional, and a value the app doesn't recognise is dropped rather
than failing the import.

| Field | Notes |
| --- | --- |
| `liquidUnits`, `growthUnits` | `metric` or `imperial`. |
| `enabledTiles` | Home's buttons, in order: `bottle`, `breast`, `diaper`, `pump`, `sleep`, `measurement`, `temperature`, `medication`. |
| `enabledSummaryPills` | The day strip: `totalMilk`, `breast`, `diaper`, `pump`, `sleep`. |
| `enabledLiveActivityKinds` | Which timers raise a Live Activity: `breast`, `pump`. |
| `enabledNotificationKinds` | Which of a partner's logs notify, named as `enabledTiles` is. |

### homeAssistant

The sensor settings, for the same reason. Leave it out too: the server
URL and the token are sealed into `secrets`, a box only the devices
signed into that iCloud account can open, so copying it between
families achieves nothing.

| Field | Notes |
| --- | --- |
| `enabled` | Whether the integration is on. |
| `secrets` | Base64 of the sealed box holding the server URL and token. |

The entity ids belong to a child rather than a family, so they are
written under that child's `sensors` instead.

## Converting another app's export

1. **Export from the other app.** Most give a CSV, one file or one per
   kind of event.
2. **Map its columns onto the keys above.** A feed usually becomes a
   `feedings` row; a diaper change a `diapers` row with `wet` and
   `solid` flags; a pumping session a `pumping` row. Convert its
   volumes to millilitres, its weights to kilograms and its
   temperatures to Celsius as you go.
3. **Write `data.json`** with one entry in `children` per child, each
   holding those arrays and a `child` header, remembering the four
   keys that must be present even when empty.
4. **Zip it**, with `data.json` at the top of the archive or inside a
   single folder.
5. **Get it onto the phone**, by AirDrop or by saving it to Files, and
   import it.

Steps 2 and 3 are the fiddly ones by hand, and they are exactly what a
modern AI tool is good at. Paste this page and a sample of your export
into ChatGPT, Claude, or whichever you use, and ask it to convert the
whole file to Zoey Baby's `data.json` format. Give it the real export
rather than a description of it, and check the result before importing:
the row counts should match your export, and the first and last dates
should be the ones you expect.

A row that matches an event already stored, by kind and timestamp,
updates that event rather than adding a second one. So a correction is
just a fixed file imported again, and an archive that overlaps history
you already have won't double it up.

If you want a real archive to compare against, export from Zoey Baby
first: **Settings → Privacy & data → Export**. The ZIP it writes is
exactly the shape described here.
