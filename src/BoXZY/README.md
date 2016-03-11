
# [BoXZY Rapid-Change FabLab: Mill, Laser Engraver, 3D Printer](http://www.boxzy.com/)

Based on comparison, the "official" BoXZY firmware source code appears to have derived from [repetier/Repetier-Firmware@28aad2de1c626401eae26ab5ded23de4e3fdec9b](https://github.com/repetier/Repetier-Firmware/commit/28aad2de1c626401eae26ab5ded23de4e3fdec9b). 
However, I can find no publically available repository for the BoXZY customizations.  I only found this [BoXZY Firmware Snapshot](http://boxzy.dozuki.com/Guide/Files/33) posted in their Troubleshooting Resources, which has been imported here.

The primary customizations appear to be centered around the BoXZY specific configuration settings and add-on enhancements to support the Etching Laser control.

This [fork](https://github.com/dewhisna/Repetier-Firmware) and "BoXZY" branch have been setup to both track additional BoXZY firmware releases from Team BoXZY (as they make them available) and to compare against the main Repetier Firmware so that future enhancements can be easily diff'd and merged.
If an official BoXZY repo is discovered with the Team BoXZY changes (let me know if you find one), it can be imported into this branch.  But, as far as I know, they didn't use a version control system, or at least not a public facing one.

Another branch, called "BoXZY-update", will be created from this "BoXZY" branch and used to merge changes from the Repetier "master" as I find to be useful for my BoXZY hardware.

