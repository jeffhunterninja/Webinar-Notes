## Offboarding Devices Discussion

**Neal Rollins:**  
How is everyone else offboarding devices? I'm trying to automate the killing of all of our security tools, RMM tools, other items. How is everyone else doing this? All manual?

**Luke Edwards:**  
I'm offboarding with a mix – have an "offboarding" policy that I set manually on the device with some scripts to uninstall apps. Some have to be removed from their app-specific dashboards, but it does save some time and prevents auto-install of these apps by the regular policy.

**Ben Filippelli:**  
We have an offboarding policy that we move devices into which detects our apps, and then uninstalls, removes our LAPS, and deletes our MSP folder.

**Neal Rollins:**  
We have an "Offboarding Devices" Org that has its own policies applied to do some automation for killing off our applications and not reinstalling. Otherwise, I'm having to heavily rely on API & Rewst for killing off tools, and letting vendors know we no longer wish to pay for X device @ Y company. Putting the heat back on the vendor since it seems like so many tools lack in the API department.  
We are using Tags to drive our automations via a CI/CD pipeline that applies tags based on tools we've already deployed by using org docs.
