🍺 Brew Control
A homebrewing dashboard built for the RAPT Pill hydrometer, designed to run full-screen on a dedicated iPad or tablet. Brew Control gives you a real-time view of your fermentation, automates key decisions, and guides you through the entire brewing process from pitch to transfer.

Features
Live RAPT Pill Data
Brew Control connects directly to the RAPT API and fetches your hydrometer data every 10 minutes. The dashboard displays:

Gravity (SG) – current specific gravity with trend (rising/falling/stable)
Temperature – current fermenter temperature
Alcohol (ABV) – estimated alcohol by volume, calculated from your OG and current SG
Velocity – gravity points per day, showing how fast fermentation is progressing

The velocity card changes color automatically:

🟢 Green – active fermentation (velocity above 0.1)
🟡 Yellow – fermentation slowing (velocity below 0.1)
🔴 Red blinking – fermentation complete / cold crash mode

Batch Management
When you start a new batch, you fill in information from your fresh wort kit sheet:

Batch name
Original Gravity (OG)
Target Final Gravity (FG)
Expected ABV %
Fermentation pressure in PSI (optional)

A batch info bar is displayed at the top of the dashboard showing the batch name, all key values, and a live timer counting up from when the batch was started.
Brewing Calculator
Enter your OG and the dashboard automatically calculates:

Current estimated ABV
Target FG vs current FG
Maximum possible ABV
A progress bar showing how far along fermentation is

Automated Process Flow
Brew Control guides you through the full fermentation process with automatic triggers and timers:
1. Diacetyl Rest
When your SG gets within 4 gravity points of your target FG, Brew Control detects this and raises an alert. If you have a RAPT Temperature Controller connected, the temperature is raised to 3°C automatically to allow the yeast to clean up any diacetyl before fermentation finishes.
2. Dry Hopping (optional)
If you choose dry hopping when setting up your batch, Brew Control will alert you when velocity drops below 0.1 (fermentation essentially complete). A banner prompts you to add dry hops. After you confirm:

Measurements are paused for 30 minutes to avoid false readings from opening the fermenter
A 72-hour countdown timer starts
After 72 hours, cold crash begins automatically

3. Cold Crash
If you are not dry hopping, cold crash triggers after velocity has been below 0.1 for 48 hours. If the RAPT Temperature Controller is connected, the temperature is lowered to 2°C automatically.
A large banner appears when it is time to start cold crash, with a button to confirm. Once started, a timer counts up to show how long the cold crash has been running.
4. Transfer to Keg
After 48 hours of cold crash, a green banner appears:

🚰 READY FOR TRANSFER TO KEG

This is your signal that the beer is clear and ready for kegging.
Checklist
A small always-visible checklist at the top of the dashboard shows your progress through the brew:
StepTrigger⬜ → 🟡 Fermentation startedWhen you start a new batch⬜ → 🟠 Diacetyl restAutomatic – SG within 4 points of target FG⬜ → 🟡 Dry hops addedWhen you confirm dry hop addition⬜ → 🟡 Fermentation completeAutomatic – velocity low for required time⬜ → 🔵 Cold crash startedWhen you press Start Cold Crash⬜ → 🟢 Ready for transferAutomatic – after 48 hours of cold crash
RAPT Temperature Controller (optional)
When enabled for a batch, the temperature card shows heating and cooling status in color:

🔴 Warm red – heating up
🔵 Cool blue – cooling down
White – at target temperature

Brew Control will command the controller to automatically adjust temperature at key stages.
Fermentation Pressure (optional)
If you ferment under pressure (e.g. with a FermZilla), you can set your target pressure in PSI when starting a batch. The pressure is displayed as a fifth metric card. A blinking red alarm triggers if pressure drops below 5 PSI – active only after the first 48 hours to avoid false alarms at the start of fermentation.
Batch History
Previous batches are saved locally and accessible via the History button. Each entry shows:

Batch name and start date
OG, final SG, and actual ABV achieved
Total duration in days
Whether dry hopping was used

New Batch
The New Batch button resets all timers, OG, and batch data. Before resetting, the current batch is automatically saved to history.

Setup
Requirements

A RAPT Pill hydrometer connected to your WiFi
A RAPT account at rapt.io
An API Secret key created under My Account → Api Secrets on rapt.io

Running the Dashboard
The dashboard is a single HTML file. To use it:

Host the file on a web server. The easiest free option is GitHub Pages:

Create a free GitHub account
Create a new public repository
Upload the file as index.html
Go to Settings → Pages → select main branch → Save
Your dashboard will be available at https://yourusername.github.io/repositoryname


Open the URL in Safari on your iPad
Tap the Share button → Add to Home Screen for a full-screen app experience
Log in with your RAPT email and API Secret

All login data, batch information, and timers are stored locally in your browser only. Nothing is sent anywhere except directly to the RAPT API.

Note: Always open the dashboard from the Home Screen icon, not from Safari directly. The two use separate local storage, so data saved in one will not appear in the other.


Compatible Hardware
DeviceStatusNotesRAPT Pill Hydrometer✅ Full supportGravity, temperature, velocity, batteryRAPT Temperature Controller✅ SupportedAuto temperature control at key stagesRAPT Digital Regulator & Spunding Valve✅ SupportedPressure display and auto-regulationInkbird ITC-308 WiFi⚠️ Not supportedClosed API, no reliable integration possible

Process Summary
Pitch yeast → Fermentation starts
       ↓
SG within 4 points of target FG → Diacetyl rest (temp raised to 3°C)
       ↓
Velocity drops below 0.1
       ↓
  [Dry hop?]
  YES → Dry hop alert → Add hops → 30 min pause → 72 hour timer → Cold crash
  NO  → Wait 48 hours → Cold crash
       ↓
Cold crash (temp lowered to 2°C) → 48 hour timer
       ↓
Ready for transfer to keg 🚰🍺

Technical Notes

Data is fetched from the RAPT API every 10 minutes via corsproxy.io to handle browser CORS restrictions
The RAPT authentication token is automatically refreshed every 50 minutes
All state (timers, batch info, history) is stored in browser localStorage
The dashboard is a single self-contained HTML file with no external dependencies beyond Google Fonts and the CORS proxy


Acknowledgements
Built for homebrewers who ferment under pressure using the KegLand / RAPT ecosystem. Inspired by the lack of a single dashboard that combines gravity, temperature, pressure, and process automation in one place.
