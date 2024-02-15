**How to install and run Planetside 2 on Linux**

This guide assumes you have basic linux knowledge like how to install packages, and what your distribution is. And remember: Keep calm and google it.

&#x200B;

**Known issues**

Currently I have encountered 3 issues remotely related to running planetside on Linux

1. Gamescope does not work, at least not on non-steam deck hardware
2. Possible crash when out of focus
3. Sometimes lutris/steam will not be happy trying to launch recursion in the same wine/proton prefix as planetside.

Starting with Gamescope on non-steam deck devices: Currently the game returns a G25 error when trying to run in Gamescope on anything other than a brand new wine/proton prefix. I have an open bug report with valve that can be [found here](https://github.com/ValveSoftware/gamescope/issues/1131)

As for the crash when the game is not focused, I have not narrowed down what is causing this but occasionally when the game is fully background it seems that Kwin (or all Wayland compositors) will stop rendering the game which might be causing it to crash. I don't know if this affects X-11 users or if this is just a me issue.

&#x200B;

**Installing Planetside 2 on linux in 2 ways**

Before following any of these steps, check with your distro documentation on how to get the right graphics drivers installed for your GPU. AMD GPUs *should* just work since their drivers are upstreamed into the kernel but might have additional installs like lib32 stuff that needs to be added, mostly an arch issue. Nvidia drivers are a bit more complicated and you probably want to be using the proprietary drivers from nvidia, again check with your distro because most of them package the drivers for install.

*Extra installs that help both steam and lutris*

1. Gamemode - made by feral interactive, sets hardware performance profiles for better gaming
2. Gamescope - Valve's custom compositor layer, helps smooth out stutters in most games but is bugged for planetside *read above in known issues*
3. Mango hud - who doesn't like performance overlays?
4. Goverlay - GUI configurator for mangohud and VKbasalt.

Most other dependencies should get pulled down automatically when installing steam or lutris but check with your distro if you are having issues.

*Steam/proton*

1. install steam
   1. actual steps here differ from distro to distro so check your distribution's instructions but on Ubuntu/Debian systems simply download and run the [.deb](https://cdn.cloudflare.steamstatic.com/client/installer/steam.deb). For arch/fedora/suse/gentoo, check with your distribution documentation or use the flatpak from flathub.
      1. On Ubuntu and Ubuntu derivatives DO NOT USE THE SNAP PACKAGE, Canonical decided to repackage steam for snap and seems to have broken some stuff and steam support will not do anything for you if you are using the snap
2. (optional) Install ProtonUp-QT
   1. you might have to enable flatpaks for this to be visible
3. (optional) using ProtonUp-QT install the latest versions of GE-Proton and SteamTinkerLaunch
4. Go to settings -> compatibility -> and make sure "Enable Steam Play for supported titles" and "Enable Steam Play for all other titles" are both enabled
   1. (optional) set run other titles with GE-proton if you want to set that globally
5. While you are in the steam settings go to Downloads and disable "Enable Shader Pre-Caching"
   1. This *might* help some older PCs but it mostly just slows down launch times and uses up disk space.
6. Download Planetside like normal
7. In Library, right click Planetside 2 and open properties.
   1. In launch options I recommend adding `gamemoderun %command%` to launch planetside with gamemode enabled
   2. add `mangohud` in front of the above if you also want the overlay
   3. If you find the launcher is running at 1FPS consider adding `WINEDLLOVERRIDES=libglesv2=d` to your launch options as well to disable the GPU on the launcher
8. Launch the game and profit
   1. You can test out different versions of proton to see if any net you better performance by using the selector in the compatibility menu in the game properties window where you set launch options

*Lutris/wine-GE*

This option uses the installer script from [lutris.net](https://lutris.net) that I updated to fix some issues, if you are having issues with it please let me know. I am on the planetside 2 discord as \[GOTR\] Zip

1. Install Lutris and all the needed dependencies
   1. instructions vary by distro but all major ones will have Lutris packaged from their package manager which will also pull down anything lutris depends on
2. Launch Lutris and run through any setup needed.
3. Click the plus button in the upper left of the window (or where ever the add games button is moved to in the future
4. Select "Search the Lutris Website for installers" or equivalent to find installers hosted on [lutris.net](https://lutris.net)
5. Search for Planetside 2
6. Select "PlanetSide 2 - Nanite Systems Operatives" or the non-test version if that name changes
7. Click install on the wine Standalone installer that shows up
   1. Follow the instructions until you can click install, the exe file will be automatically downloaded for you by the script
   2. Let the script do its work, It will install some winetricks and then it will launch the setup exe like on windows
   3. Follow the setup steps until you get the DirectX window, click cancel on that.
      1. we installed directX as part of the winetricks so its not needed and will error, but likely wont break the install
   4. Once you get to launch pad sign into your planetside account
   5. close the launcher when it starts downloading to finish the lutris install
8. relaunch planetside through lutris
9. profit.

The install script should automatically add a few overrides to fix common issues but you can right click and configure the game in lutris to change settings. Under the system tab you can enable stuff like Mangohud if you want that or GameMode.

If you are on a system like a laptop with an IGPU and an Nvidia graphics card, make sure to enable Nvidia Prime Render Offload if it is not enabled already.

&#x200B;

**Recursion Stat Tracker**

Installing RST is pretty simple. If you are using Steam, this [guide is well written](https://www.reddit.com/r/Planetside/comments/11vao54/recursion_overlay_working_on_linux/)

For Lutris

1. Download the [recursion installer](https://recursiontracker.com/?p=get)
2. In Lutris, click add game (the plus in the upper left corner)
3. select "install a windows game from an executable"
4. Set game name to Recursion, Recursion Stat Tracker, or what ever you want really
5. Click install
6. Click install the next screen
7. Using the three dots change the installation directory to the same directory you installed planetside to
   1. This is usually $HOME/Games/planetside-2 where $HOME is the path to your home directory.
      1. for me it is `/home/arched/Games/planetside-2`
   2. Lutris will show a warning, it is safe to ignore this.
8. click continue
9. using the 3 dots select the RTST msi file you downloaded in step 1
10. click install
11. follow the usual recursion install steps
12. Log into recursion and let it update if needed
13. exit out of recursion to let the lutris installer finish
14. Go to configure on recursion in lutris and make sure that the executable is set under game options
15. Check to make sure the runner is the same as planetside under runner options.
16. profit

to launch recursion I recommend first launching planetside in Lutris and then launching recursion. Occasionally lutris might hang when doing this but that fine, just wait or close lutris and re-launch recursion if it didn't launch the first time.

&#x200B;

**Benchmark numbers**

Last time I wrote this I was asked if I had any benchmark or performance test data so I went and got some. Just as a note: Planetside is not an easily benchmarked game, and its mostly your faults. Lack of consistent AI makes test unrepeatable so I didn't even really try.

On both windows and Linux I used the same [useroptions.ini](https://pastebin.com/Wh5L6SPH). I found the largest fight I could for \~10AM on Emerald which happened to be Nason's. I found a kinda out of the way spot where I could look at the fight and logged FPS for \~2 mins.

I used MangoHud on Linux and MSI Afterburner/RivaTuner on Windows. Blame the games journalism industry that there is no cross-platform benchmarking tools yet.

&#x200B;

|Tracked Value|Windows|Wine|
|:-|:-|:-|
|0.1% Low|90.6|91.5|
|1% Low|108.5|101.5|
|Average|138.9|124.1|

The numbers are pretty comparable windows to wine but experimentally running the game through wine felt noticeably smoother. I was unable to track frametimes on windows since I don't really know how to use RivaTuner but on linux my average frametimes were:

&#x200B;

|Average|8.0568|
|:-|:-|
|Min|6.68|
|Max|12.373|

&#x200B;

Comments and criticisms welcome. I will likely be using this as the last Linux post I make unless something major changes like the devs deciding to break the anti-cheat or something REALLY major updates on the wine side.
