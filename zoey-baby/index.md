---
layout: default
title: "Zoey Baby"
---

<style>
  .app-header { gap: 1.25rem; }
  .app-header img.app-icon { width: 128px; height: 128px; border-radius: 22%; flex: 0 0 auto; }
  .shot-grid img { width: 100%; height: auto; border-radius: 14px; border: 1px solid rgba(0,0,0,.08); }
  .shot-grid .shot { max-width: 320px; margin: 0 auto; }
  @media (max-width: 575.98px) {
    .app-header img.app-icon { width: 96px; height: 96px; }
  }
</style>

<div class="d-flex flex-column flex-sm-row align-items-center align-items-sm-start text-center text-sm-left app-header mb-4">
    <img src="/media/zoey-baby-app-256.png" srcset="/media/zoey-baby-app-256.png 1x, /media/zoey-baby-app-512.png 2x" class="app-icon" alt="Zoey Baby app icon">
    <div>
        <h3 class="mt-0"><a href="/zoey-baby">Zoey Baby</a></h3>
        <p>A fast, private baby tracker. Feedings, diapers, pumping, and growth, synced between you and your partner over iCloud.</p>
        <ul class="text-left">
            <li>Log bottles, breast feeds, diapers, pumping, naps, growth, temperature and medicine, with a photo on anything you log.</li>
            <li>Live breast feed and pump timers that either parent can start, switch sides on, or stop, from whichever phone is nearest.</li>
            <li>Sync with your partner over iCloud, with a ping when they log something. No accounts, no third-party servers, no ads.</li>
            <li><a href="/zoey-baby/home-assistant">Home Assistant integration</a> for camera and smart sock readings: heart rate, oxygen, skin temperature, and sleep state.</li>
            <li>Naps written down from the sensor and rolled up into daily awake, asleep, and deep totals. A nap you log or end by hand stays exactly as you left it.</li>
            <li>Sleep alerts when the sensor reports your baby falling asleep, going deep, or waking up.</li>
            <li>Nap prediction off the wake windows your baby actually keeps, with an alert before the next likely sleep or wake.</li>
            <li>Photos that blur themselves whenever you are away from home, on the kinds of event you choose.</li>
            <li>Siri support: log feeds, ask when the last diaper was, all in your Settings unit.</li>
            <li>Home Screen widgets you configure per baby, an Apple Watch app for logging from your wrist, and Live Activities on the Lock Screen and Dynamic Island.</li>
            <li>Trends over 7 days, 30 days, or all time, and growth charts against WHO percentile curves.</li>
            <li>Pump reminders at the times you set.</li>
            <li>Export a whole history, photos included, as a single file you can import again later, or bring one over from Nara.</li>
        </ul>
        <p>Download on the <a href="https://apps.apple.com/us/app/id6786016688">iOS App Store</a>.</p>
        <p>Setting up the sensors? See the <a href="/zoey-baby/home-assistant">Home Assistant guide</a>.</p>
    </div>
</div>

<h3 class="mt-5 mb-3">Screenshots</h3>
<div class="row row-cols-1 row-cols-sm-2 row-cols-lg-3 shot-grid">
    <div class="col mb-4">
        <figure class="shot mb-0">
            <img src="/zoey-baby/screenshots/01-home.png" loading="lazy" alt="Home screen with vitals, quick actions, today's totals and the day's timeline">
        </figure>
    </div>
    <div class="col mb-4">
        <figure class="shot mb-0">
            <img src="/zoey-baby/screenshots/02-glance.png" loading="lazy" alt="Lock Screen controls, a breast feed timer in the Dynamic Island, widgets and notifications">
        </figure>
    </div>
    <div class="col mb-4">
        <figure class="shot mb-0">
            <img src="/zoey-baby/screenshots/03-caregivers.png" loading="lazy" alt="Invite code for sharing a baby's records with another caregiver">
        </figure>
    </div>
    <div class="col mb-4">
        <figure class="shot mb-0">
            <img src="/zoey-baby/screenshots/04-trends.png" loading="lazy" alt="The last seven days compared against the week before, and feeds per day">
        </figure>
    </div>
    <div class="col mb-4">
        <figure class="shot mb-0">
            <img src="/zoey-baby/screenshots/05-growth.png" loading="lazy" alt="Weight and height plotted against WHO percentile curves">
        </figure>
    </div>
    <div class="col mb-4">
        <figure class="shot mb-0">
            <img src="/zoey-baby/screenshots/06-siri.png" loading="lazy" alt="Siri phrases for logging a diaper, starting a breast feed, and asking when the last feed was">
        </figure>
    </div>
    <div class="col mb-4">
        <figure class="shot mb-0">
            <img src="/zoey-baby/screenshots/07-privacy.png" loading="lazy" alt="Photo grid with photos blurred while away from home">
        </figure>
    </div>
</div>
