---
layout: page
title: "Zoey Baby - Home Assistant"
breadcrumb: "Home Assistant"
parent: "Zoey Baby"
parent_url: /zoey-baby
---

<style>
  #content table { border-collapse: collapse; margin: 1rem 0; width: 100%; }
  #content table th, #content table td { border: 1px solid rgba(0,0,0,.12); padding: .5rem .75rem; text-align: left; vertical-align: top; }
  #content table th { background: rgba(23,151,99,.08); }
  #content h2 { margin-top: 2rem; }
  #content h3 { margin-top: 1.5rem; font-size: 1.15rem; }
</style>

Zoey Baby can read live sensor values out of your own Home Assistant
instance. A smart sock gives heart rate, oxygen, skin temperature and
sleep state; a baby monitor camera gives asleep or awake. Readings show
on the Home screen, and with sleep tracking on, the sensor's history is
rolled up into daily awake, asleep and deep totals, with naps drawn on
the timeline that nobody had to log.

Your phone talks to your Home Assistant directly. Nothing routes
through a gxlabs server, and the credentials stay on your devices.

## What you need

### 1. A Home Assistant instance

If you don't run one yet, start with the
[Home Assistant installation guide](https://www.home-assistant.io/installation/).
Zoey Baby needs no add-on and no custom component of its own, just an
instance you can sign in to.

### 2. Your baby monitor camera and/or smart sock, set up in Home Assistant

Either one on its own is enough, and the app uses both together if you
have both.

Zoey Baby reads entities, it doesn't talk to the devices. Getting your
camera or sock into Home Assistant is a Home Assistant job: an official
[integration](https://www.home-assistant.io/integrations/), a community
integration, or an add-on, whichever one covers your hardware. Once the
device's entities report a value in Home Assistant, Zoey Baby can read
it.

The app only lists `sensor.` and `binary_sensor.` entities.

### 3. A URL that works away from the house

On your home wifi a local address is fine, and the app will take one:
`http://homeassistant.local:8123` connects and reads sensors like any
other. It stops the moment you leave, because that address only
resolves on your own network, and the readings stop with it.

So if you want vitals and naps while you are out, which is most of the
point, give the app an address that reaches your instance from
anywhere. In practice that means an `https://` one:

* [Home Assistant Cloud](https://www.nabucasa.com/) hands you a
  `https://….ui.nabu.casa` address with nothing to configure.
* Or set up [remote access](https://www.home-assistant.io/docs/configuration/remote/)
  yourself, with your own domain and certificate.

Either one works at home too, so it goes in the app once and you can
forget about it.

### 4. A long-lived access token

In Home Assistant, open your user profile, go to the **Security** tab,
and create a long-lived access token at the bottom of the page. Copy it
while it's on screen, because Home Assistant won't show it again. The
[authentication docs](https://www.home-assistant.io/docs/authentication/)
walk through it, or go straight to
[your profile's Security tab](https://my.home-assistant.io/redirect/profile_security/).

The token is what the app authenticates with. Deleting it in Home
Assistant is how you revoke the app's access.

Zoey Baby keeps the token in the iOS keychain rather than in ordinary
settings, and an exported backup carries it encrypted: the ZIP holds
the URL and the token sealed under a key in your iCloud keychain, so
they open on your own devices and read as blank on anyone else's. Your
entity names go in as plain text, since they name sensors rather than
secrets.

## Setting it up in the app

1. Open **Settings**, then **Sensors**.
2. Turn **Enabled** on.
3. Paste your URL and your long-lived access token.
4. Tap **Connect**. The app checks the token, then loads your entities.
5. Pick the entity behind each reading.

Entity names get a look on connect, so a sock whose entities are named
the usual way arrives with most slots already filled. Anything the app
guesses wrong you can change, and a slot you picked yourself is never
overwritten.

| Slot | Entity | What it reads |
| --- | --- | --- |
| Heart rate | [`sensor`][sensor] | a number, in bpm |
| Oxygen | [`sensor`][sensor] | a number, in % |
| Sleep state | [`sensor`][sensor] | text: `awake`, `light_sleep`, `deep_sleep` |
| Camera sleep | [`binary_sensor`][binary] | [on or off][binary] |
| Charging | [`binary_sensor`][binary] | [on or off][binary], so time on the base doesn't read as the sensor dropping out |
| Battery % | [`sensor`][sensor] | a number, 0 to 100 |
| Skin temp | [`sensor`][sensor] | a number, in °F or °C |

[sensor]: https://www.home-assistant.io/integrations/sensor/
[binary]: https://www.home-assistant.io/integrations/binary_sensor/

Each picker is filtered to entities of the right shape and shows what
they read right now, alongside how long ago, so you can tell a good
pick from a wrong one before you leave the screen. Nothing else is
required: fill in only the slots you have a sensor for.

Sensor settings live on the family record, so a partner who joined your
iCloud share picks up the same URL, token and entity choices without
typing any of it again.

## Sleep tracking

With at least one sleep sensor picked, **Track sleep** appears under
Sensors. Turn it on and the app pulls the sensor's history and keeps it
up to date:

* Each day gets awake, asleep and deep totals.
* Each stretch of sleep becomes a nap on the Home timeline.
* Where the sock is off the baby, the camera covers the gap.
* A nap you log or end by hand stays exactly as you left it. The
  sensor pass won't move it or stretch it back out.
* Logging a bottle, a breast feed or a diaper ends the nap that's
  running, so a feed at 3am closes the night stretch for you.

**Reload sleep history** under Sensors rebuilds the last two weeks from
Home Assistant, which is the fix if a day looks wrong after the sensor
was offline.

## Alerts

Under **Settings**, then **Notifications**, the Sleep section can ping
you when the sensor reports your baby falling asleep, going into deep
sleep, coming back to light sleep, or waking up.

Nap prediction learns your baby's rhythm from the naps on record and
can warn you a set number of minutes before the next predicted sleep or
wake.

## If something isn't working

**"Home Assistant rejected the token (401)."** Long-lived tokens stop
working if they're deleted in Home Assistant, if the account's password
changes, or if Home Assistant is restored from a backup older than the
token. The expiry date shown in Home Assistant is not the problem.
Create a fresh token and paste it in.

**"Home Assistant is reachable and the token works, but it returned no
sensors."** The connection is fine and the device isn't in Home
Assistant. Check the integration for your camera or sock is set up and
that its entities aren't disabled.

**It works at home and not out.** Open the same URL in Safari on your
phone with wifi off. A local address won't load, by design, and that is
the thing to swap for a public one. If a public address doesn't load
either, fix it in Home Assistant before coming back to the app.

**Readings go blank or stale.** A sock on its base reports nothing
useful, and the app drops a zero heart rate rather than showing it.
Values come back when the sock is back on the baby.

Anything else, email
[zoey-baby-support@gxlabs.co](mailto:zoey-baby-support@gxlabs.co).
