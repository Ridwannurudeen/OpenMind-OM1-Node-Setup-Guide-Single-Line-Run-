# OpenMind-OM1-Node-Setup-Guide-Single-Line-Run-
This guide shows how to set up and run an OpenMind OM1 Node on Ubuntu in a single command, including dependencies, environment setup, and starting the node.

Prerequisites
Ubuntu 22.04 / 24.04 LTS (recommended)
Root or sudo access
OpenMind API Key (from OpenMind Portal)
screen installed (sudo apt install -y screen if missing)
Note: This guide assumes you are on a cloud VPS. Audio input errors may appear if no physical microphone is present, but the node will still run.


One-Line Command to Set Up & Run the Node

Replace your_api_key_here with your actual OpenMind API key.

ssudo apt update -y && sudo apt install -y git curl screen portaudio19-dev python3-all-dev ffmpeg alsa-utils python3-pip pulseaudio && \
curl -LsSf https://astral.sh/uv/install.sh | sh && \
git clone https://github.com/OpenMind/OM1.git ~/OM1 && cd ~/OM1 && git submodule update --init && \
uv venv && (echo "OM_API_KEY=your_api_key_here" > .env) && \
pulseaudio --start && \
pactl load-module module-null-sink sink_name=om_sink && \
pactl load-module module-remap-source master=om_sink.monitor source_name=om_mic && \
pactl set-default-sink om_sink && pactl set-default-source om_mic && \
screen -dmS openmind uv run src/run.py conversation


#Optional: Persistent Virtual Microphone (VPS Only)

If you want live audio input, set up a virtual microphone:

sudo apt install -y pulseaudio alsa-utils
pulseaudio --start
pactl load-module module-null-sink sink_name=om_sink
pactl load-module module-remap-source master=om_sink.monitor source_name=om_mic
pactl set-default-sink om_sink
pactl set-default-source om_mic

#Verify Node is Running
openmind node logs

#Useful Commands for the Screen Session

#Reattach to session:

screen -r openmind


#Detach from session (keep it running):

Ctrl + A, then D


#Kill the session:

screen -S openmind -X quit
