# SDR Remote Access Implementation Plan

## Project Goals
- RSP DX centrally located, accessible from multiple devices
- Support for Windows laptop, Mac laptop, Linux box, Android devices
- Enable trunked police system monitoring (DSDplus Fastlane capability)
- Multi-user access for friends
- Web-based access option

## Current State Assessment
- **Hardware:** Raspberry Pi 4 with RSP DX
- **Current Software:** SDRconnect Server, Pi-hole
- **Network:** Ethernet connected Pi

## Phase 1: Pi Configuration Verification

### 1.1 System Check
```bash
# Check OS version and architecture
cat /etc/os-release
uname -a

# Check available resources
free -h
df -h

# Verify RSP DX detection
lsusb | grep -i sdr
```

### 1.2 Required Pi Setup
- **OS:** Raspberry Pi OS 64-bit (Bullseye or newer)
- **Memory:** 4GB+ recommended
- **Storage:** 32GB+ SD card + USB drive for recordings
- **Network:** Ethernet preferred for stability
- **Desktop:** VNC-enabled desktop environment (optional but recommended)

### 1.3 Essential Packages
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential cmake git libusb-1.0-0-dev pkg-config
sudo apt install -y realvnc-vnc-server realvnc-vnc-viewer  # For GUI access
```

## Phase 2: Software Architecture Decision

### Option A: VirtualHere + Native Software (Recommended)
**Pros:** Full Windows software compatibility, DSDplus Fastlane works
**Cons:** Licensing cost, Windows/Mac focused

**Pi Components:**
- VirtualHere USB Server
- Optional: DSDplus on Pi for 24/7 monitoring
- VNC server for remote GUI access

**Client Access:**
- Windows: VirtualHere Client → SDRuno + DSDplus Fastlane
- Mac: VirtualHere Client → CubicSDR
- Linux: rtl_tcp or direct SSH
- Android: OpenWebRX web interface

### Option B: Multi-Protocol Server
**Pros:** Free, supports all platforms natively
**Cons:** No DSDplus Fastlane, more complex setup

**Pi Components:**
- rtl_tcp server
- OpenWebRX for web access
- SoapySDR with network module
- Trunk-recorder for automated trunked monitoring

## Phase 3: Installation Checklist

### 3.1 Pi Baseline Setup
- [ ] Verify 64-bit OS installation
- [ ] Enable SSH access (`sudo systemctl enable ssh`)
- [ ] Enable VNC for GUI access (`sudo raspi-config`)
- [ ] Configure static IP address
- [ ] Test RSP DX detection with SDRplay API
- [ ] Verify network connectivity from client devices

### 3.2 VirtualHere Server Setup (Option A)
```bash
# Download and install VirtualHere USB Server
curl https://raw.githubusercontent.com/virtualhere/script/main/install_server | sudo sh

# Configure as system service
sudo systemctl enable vhusbdpin
sudo systemctl start vhusbdpin
```

### 3.3 Additional Services (Optional)
- [ ] Install Icecast2 for audio streaming (only if you want to stream audio)
- [ ] Set up automated startup scripts

## Phase 4: Client Configuration

### 4.1 Windows Laptop Setup
- [ ] Install VirtualHere Client
- [ ] Connect to Pi VirtualHere server
- [ ] Verify RSP DX appears in Device Manager
- [ ] Install SDRuno, configure USB Bulk mode
- [ ] Install DSDplus Fastlane
- [ ] Test trunked system decoding

### 4.2 Mac Laptop Setup
- [ ] Install VirtualHere Client (Mac version)
- [ ] Install CubicSDR or SDR++
- [ ] Test basic reception and scanning

### 4.3 Linux Box Setup
- [ ] Install SDR++ or Gqrx
- [ ] Configure rtl_tcp connection to Pi
- [ ] Test remote SDR access

### 4.4 Android/Mobile Setup
- [ ] Test OpenWebRX web interface
- [ ] Configure RF Finder or SDR Touch apps
- [ ] Verify audio streaming works

## Phase 5: Network Setup (Keep It Simple)

### 5.1 Port Requirements (For Reference Only)
- **VirtualHere:** 7575 (TCP)
- **rtl_tcp:** 1234 (TCP, customizable)
- **OpenWebRX:** 8073 (HTTP)
- **VNC:** 5900 (TCP)

*Note: Most home routers will handle this automatically on local network*

## Phase 6: Testing and Validation

### 6.1 Functionality Tests
- [ ] Basic SDR reception from each client type
- [ ] Scanning functionality (Windows via VirtualHere)
- [ ] Trunked system decoding and audio streaming
- [ ] Multi-user simultaneous access
- [ ] Network stability under load

### 6.2 Performance Optimization
- [ ] Bandwidth usage monitoring
- [ ] Audio quality tuning
- [ ] Buffer size optimization
- [ ] Error rate monitoring

## Implementation Priority (Simplified)
1. **Phase 1:** Check if Pi is ready
2. **Phase 3.1:** Basic Pi setup (VNC, static IP)
3. **Phase 3.2:** Install VirtualHere server
4. **Phase 4.1:** Test Windows laptop + DSDplus
5. **Phase 4.2-4.4:** Add other devices as needed
6. **Phase 6.1:** Make sure everything works reliably

## Success Criteria
- [ ] Can access RSP DX from laptop outside house
- [ ] DSDplus Fastlane works for trunked monitoring
- [ ] Multiple device types can connect simultaneously
- [ ] Web access works for casual listening
- [ ] System runs reliably 24/7

## Troubleshooting Notes
- VirtualHere requires USB Transfer Mode = "Bulk" in SDRuno
- WiFi clients may need stronger signal for stable operation
- Some Android browsers may not support OpenWebRX fully
- DSDplus may need buffer adjustments for network operation

## Cost Considerations
- VirtualHere Server: ~$49 one-time license
- Additional storage: ~$20-30 for USB drive
- No ongoing costs for basic setup

---
*Document Version: 1.0*
*Last Updated: June 2025*