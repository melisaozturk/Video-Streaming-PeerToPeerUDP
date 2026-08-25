# Video streaming iOS application - PeerToPeerUDP

A peer-to-peer video streaming iOS application that enables real-time video capture, sharing, and playback between iOS devices over a local network using UDP communication.

## Overview

PeerToPeerUDP allows iOS devices to:
- **Host live video streams** from the device camera
- **Discover nearby peers** using Bonjour/mDNS service discovery
- **Join video sessions** hosted by other devices
- **Stream video** over UDP with minimal latency
- **Record and save** video sessions to the photo library

## Features

- Real-time video capture at 1920x1080 resolution
- H.264 video encoding with AAC audio
- UDP-based peer-to-peer communication
- Automatic peer discovery on local network
- Front and rear camera support
- Camera switching during recording
- Video playback with AVPlayer
- Save recordings to photo library

## Technical Architecture

### Networking
- **Protocol**: Custom UDP framing protocol over Apple's Network framework
- **Port**: 55555 for hosting, 5555 for peer connections
- **Discovery**: Bonjour service (`_videoStream._udp`)
- **Host**: Default hardcoded to 192.168.4.1

### Key Components

| Component | Purpose |
|-----------|---------|
| `ViewController` | Main UI controller managing hosting, discovery, and playback |
| `PeerListener` | UDP server for hosting video streams |
| `PeerBrowser` | Discovers available video streams via Bonjour |
| `PeerConnection` | Manages UDP connections between peers |
| `VideoProtocol` | Custom network framing protocol implementation |
| `StreamController` | Handles camera capture, encoding, and recording |

### Media Processing
- **Video**: H.264 codec, 1920x1080, MP4 container
- **Audio**: MPEG4 AAC, 44.1kHz, 192kbps
- **Framework**: AVFoundation for capture and playback

## Requirements

- iOS 13.0+
- Xcode 11.0+
- Swift 5.0+
- Physical iOS device (camera access required)
- Devices must be on the same local network

## Permissions Required

The app requires the following permissions (configured in Info.plist):
- Camera access
- Microphone access
- Photo library access (for saving recordings)

## Usage

### Hosting a Video Stream

1. Launch the app on the host device
2. Enter a session name in the "Host A Video" text field
3. Tap the "HOST" cell
4. Grant camera and microphone permissions if prompted
5. Video capture will begin automatically
6. Other devices can now discover and join your stream

### Joining a Video Stream

1. Launch the app on a client device
2. Available video streams will appear in the "Join A Video" section
3. Tap on a discovered peer name to connect
4. The video stream will begin playing automatically

### Stopping

- Tap the host cell again to stop hosting
- Navigate away or close the app to disconnect from a stream

## Network Configuration

The application currently uses hardcoded network settings:

- **Listener Port**: 55555
- **Connection Port**: 5555
- **Default Host**: 192.168.4.1


## Project Structure

```
PearToPearNetwork/
├── Network/
│   ├── PeerListener.swift          # UDP server for hosting
│   ├── PeerBrowser.swift           # Bonjour service discovery
│   ├── PeerConnection.swift        # Peer-to-peer UDP connections
│   └── VideoProtocol.swift         # Custom network protocol
├── ViewController.swift             # Main UI controller
├── StreamController.swift           # Video capture and encoding
├── TableViewCell.swift              # Peer list cells
├── HostTableViewCell.swift          # Host session input cell
├── AppDelegate.swift                # App lifecycle
├── SceneDelegate.swift              # Scene lifecycle
└── Main.storyboard                  # UI layout

```

## How It Works

### Hosting Workflow

1. `PeerListener` creates UDP listener on port 55555
2. Service is advertised via Bonjour with session name
3. `StreamController` starts camera capture and recording
4. Video is encoded to MP4 format
5. File URL is sent to connected peers via UDP
6. Peers receive URL and stream video content

### Discovery Workflow

1. `PeerBrowser` browses for `_videoStream._udp` services
2. Discovered peers are displayed in table view
3. User selects a peer to connect
4. `PeerConnection` establishes UDP connection
5. Video URL is received and passed to `AVPlayer`
6. Video playback begins in UI

## Known Limitations

- Hardcoded network host (192.168.4.1)
- UDP-based streaming (no packet loss recovery)
- Single video stream per host
- Requires local network connectivity
- No authentication or encryption

## Troubleshooting

**Peers not appearing:**
- Ensure devices are on the same local network
- Check that Bonjour/mDNS is not blocked by firewall
- Verify network settings match on both devices

**Video not streaming:**
- Check camera permissions are granted
- Verify the hardcoded host IP matches your network
- Ensure UDP ports 5555 and 55555 are not blocked

**Camera not working:**
- Must run on physical iOS device (not simulator)
- Verify camera permissions in Settings app
- Check that another app isn't using the camera

## Dependencies

This project uses only Apple's native frameworks:
- Network.framework
- AVFoundation.framework
- AVKit.framework
- UIKit.framework
- Photos.framework

No external package managers (CocoaPods, SPM, Carthage) required.

