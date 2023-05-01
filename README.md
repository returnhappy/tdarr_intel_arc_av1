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
5) Encode the video to test that the hardware encoding works, open task manager and check that the Intel Arc GPU is being utilized at least 50%. The AV1 encoding fps for 1080p video should be around 120fps on the a380, 2160p is around 30fps.
6) In top menu: Presets: Add preset, choose desired audio and subtitle settings, click Add. Export the preset to file, save to the c:\tdarr directory

Configure tdarr node:
1) In the tdarr installation directory > configs, edit Tdarr_Node_config.json
2) In "handbrakePath": replace "" with "C:/tdarr/HandBrakeCLI.exe", make sure to use forward slashes "/" or double "\\" for path. If you run the server on the same pc leave other settings the same, otherwise change the server ip.

Configure the tdarr server:
1) In the tdarr installation directory > Tdarr_Server folder Run the Tdarr_Server.exe, open your ip with the port (replace 0.0.0.0 with your ip, in windows cli run ipconfig) 0.0.0.0:8265 address in your browser.
2) In the tdarr installation directory > Tdarr_Node folder Run the Tdarr_Node.exe
3) In the tdarr webui go to tab Plugins: search for "075a" then on the result "Video Transcode Customizable" click copy to local, click the local tab on the top.
4) Click main tab Libraries, create a library, define source, click options >  Scan fresh.
5) In the library settings go to transcode tab, click  on and disable all plugins except the "new file size check" and the "Video transcode Customizable" plugins.
6) Click "Video transcode Customizable" plugin and enable it, in the inputs: codecs to exclude "av1", cli "handbrake", transcode arguments '--preset-import-file "C::\tdarr\qsv-av1-30.json" -Z "qsv-av1" --all-audio --all-subtitles' (replace the "qsv-av1-30.json" with your handbrake preset file name, and the "qsv-av1" with the name of the preset that you saved as in handbrake), output container ".mkv"
7) In main tab Tdarr: Under Nodes: click your node to the right of the "All" button, click options, change "Any (nvenc,qsv,vaapi)" to "qsv", enable "Allow all GPU workers to do CPU tasks"
8) Add 3 GPU workers to the right of the "Transcode:", add 1 CPU worker to health check.
9) The transcoding should begin now, the progress bar should go showly up for each job, if it completes instantly something is wrong, on the bottom of the screen should be "Transcode: success/not required" number rising.
10) If working correctly, in library settings > output folder > enable delete source file
