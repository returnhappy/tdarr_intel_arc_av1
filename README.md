# tdarr_intel_arc_av1
Instructions on how to setup tdarr automatic hardware AV1 transcoding with the intel Arc gpus such as the a380.

Windows 10:

Preparation:
Hardware needed:
Intel Arc Gpu with Av1 hardware encoding support, the cheapest being a380 for $150 in the US

Download dependencies:
Intel Graphics Driver
Handbrake 1.6.1 Gui
Handbrake 1.6.1 CLI
Tdarr

Install Dependencies:
1) Install Intel ARC Gpu drivers
2) Install Handbrake GUI
3) Create folder on c:\ drive root directory "tdarr", put in the handbrakeCLI.exe into it
4) Extract tdarr zip and Install tdarr with tdarr_updater.exe, it will create the tdarr server and node folders

Create handbrake preset for AV1 encoding
1) Open handbrake 1.6.1 gui, open a video to test transcoding, in video tab set encoder to "QSV AV1
