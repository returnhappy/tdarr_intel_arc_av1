# tdarr_intel_arc_av1
Instructions on how to setup tdarr automatic hardware AV1 transcoding with the intel Arc gpus such as the a380.

Windows 10:

Preparation:
Hardware needed:
1) Intel Arc Gpu with Av1 hardware encoding support, the cheapest being a380 for $150 in the US
2) The Arc GPU needs to be preferably in PCIE slot 1 on the motherboard, and the RESIZABLE BAR needs to be enabled in the bios

Download dependencies:
Intel Graphics Driver
Handbrake 1.6.1 Gui
Handbrake 1.6.1 CLI
Tdarr

Install Dependencies:
1) Install Intel ARC Gpu drivers
2) Install Handbrake GUI
3) Create folder on c:\ drive root directory "tdarr", extract and put in the HandBrakeCLI.exe into it
4) Extract tdarr zip and Install tdarr with tdarr_updater.exe, it will create the tdarr server and node folders

Create handbrake preset for AV1 encoding
1) Open handbrake 1.6.1 gui, open a video to test transcoding.
2) Optional: Summary tab: set format to mkv
3) Optional: Dimensions tab: set resolution limit one
4) Video tab: set video encoder to "AV1 10-bit (Intel QSV)", Constant quality: 30 ICQ, Framerate: constant, Encoder preset: Quality, Encoder Profile: Auto, Encoder level: auto, Save as: pick location for test.
5) In top menu: Presets: Add preset, choose desired audio and subtitle settings, click Add. Export the preset to file, save to the c:\tdarr directory

Configure tdarr node:
1) In the tdarr installation directory > configs, edit Tdarr_Node_config.json
2) In "handbrakePath": replace "" with "C:/tdarr/HandBrakeCLI.exe", make sure to use forward slashes "/" or double "\\" for path. If you run the server on the same pc leave other settings the same, otherwise change the server ip.

Configure the tdarr server:
