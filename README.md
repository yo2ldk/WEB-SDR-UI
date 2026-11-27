# WEB UI SDR Radio — YO2LDK

A browser-based Software Defined Radio (SDR) receiver interface designed for direct interaction with supported SDR hardware, IQ-over-WebSocket sources, and a built-in demonstration receiver.

> **Project:** WEB UI SDR Radio - YO2LDK  
> **Format:** Single-page HTML application  
> **Primary target:** Chromium-based browsers (Google Chrome / Microsoft Edge)

<img width="1600" height="799" alt="2776be7b-0f50-4614-a252-c78e64d8d47a" src="https://github.com/user-attachments/assets/11475081-c1d8-4cc5-bf69-3cecf2085065" />



## Overview

This project brings a feature-rich SDR receiver interface to the browser. It is delivered as a self-contained HTML page rather than a conventional Windows executable. Open the page in a compatible browser, select a receiver, grant the requested device permissions, and start listening.

The interface includes a live spectrum display, waterfall, dual VFO controls, demodulation modes, receiver-specific hardware controls, audio processing, signal decoders, and optional information panels.

## Supported input sources

- **Demo mode** — simulated IQ source for exploring the interface without connected radio hardware.
- **RTL-SDR** — direct WebUSB access for supported R820T-based devices.
- **Airspy R2 / Mini** — direct WebUSB receiver access.
- **HackRF** — direct WebUSB receiver access, with internal sampling and software decimation handling.
- **RX888 / RX888 mkII** — direct WebUSB access to the 16-bit HF ADC stream, with an in-page DSP worker for digital down-conversion. The page can load the Cypress FX3 firmware image when a device is in boot mode.
- **FUNcube Dongle Pro / Pro+** — WebHID control combined with stereo audio IQ capture through the browser audio subsystem.
- **WebSocket IQ** — network IQ input for compatible `rtl_tcp`, HackRF, RSP1, or bridge servers. The endpoint and sample format are configurable.

Hardware access depends on browser support, operating-system drivers, device permissions, and the specific receiver model. Direct USB/HID access is generally available in Chrome and Edge, not in every browser.

## Receiver and tuning features

- Start / Stop receiver control and selectable SDR input.
- AM, USB, LSB, CW, NFM, WFM, and DSB demodulation modes.
- Dual VFO presentation with selectable active VFO and frequency tuning controls.
- Frequency entry, tuning-step selection, band shortcuts, and frequency calibration.
- Upconverter frequency offset and spectrum inversion options.
- Configurable sample rate and software decimation where supported.
- Receiver-specific gain, attenuation, amplifier, bias-tee, ADC, and related hardware controls where exposed by the selected device.

## Spectrum, waterfall, and display

- Real-time spectrum and waterfall visualizations.
- Adjustable FFT size, averaging, window function, spectrum floor and range.
- Zoom controls and peak hold.
- Custom spectrum background: solid colours or a selected image, with sizing, positioning, dimming, fit, and fill controls.
- Resizable spectrum / waterfall split and adjustable auxiliary panel.
- Optional movable or docked S-meter display.
- UTC date and time display.

## Audio and signal tools

- Audio output level control.
- Noise blanker (NB), audio noise reduction (NR), and auto-notch filtering (ANF).
- Audio spectrum / spectrogram panel.
- Built-in decoder panel with selectable modes:
  - CW
  - RTTY
  - PSK31
  - APRS 1200
  - FT8
  - SSTV
  - WEFAX
- RDS decoder for WFM reception.
- Decoder-specific controls where applicable, including RTTY shift / baud rate, FT8 band selection, SSTV mode and image saving, and WEFAX speed / IOC / phasing controls.
- Save decoded SSTV and WEFAX images as PNG where supported by the interface.

Decoder performance depends on signal quality, audio path, sample rate, timing, and the capabilities of the browser and computer.

## Information ticker

An optional configurable information ticker can display:

- Space-weather information (including solar indices and alerts).
- A rough HF propagation estimate.
- Weather information and forecast-derived warnings for a selected or automatically resolved location.
- Earthquake information from nearby and worldwide events.

The ticker can be paused by hovering, its speed can be adjusted, and individual information categories can be enabled or disabled. The page identifies its information providers in the Info panel (NOAA SWPC, Open-Meteo, USGS, and IP-based location lookup). Forecast-derived warnings are not official emergency alerts, and propagation estimates are indicative only.

## Why a web application instead of an EXE?

A browser-based SDR interface offers several practical advantages over a traditional, platform-specific executable:

1. **No conventional installation** — the application is opened as an HTML page; there is no installer or Windows setup wizard.
2. **Cross-platform potential** — the same page can run on supported desktop operating systems wherever a compatible browser and required device APIs are available.
3. **Easy updates** — replace the HTML file with a newer release; there is no separate executable installation process.
4. **Portable and easy to share** — one page can be stored locally, placed on a web server, or published in a Git repository.
5. **Accessible interface** — the UI runs in a familiar browser window and can be used alongside other browser-based radio tools.
6. **Direct hardware access** — supported browsers can communicate with USB/HID receivers through WebUSB and WebHID after the user grants permission.
7. **Network receiver support** — WebSocket IQ input makes it possible to use a remote or locally hosted IQ server without building a separate native client into the page.
8. **No mandatory backend for local operation** — the demo and supported direct-device modes execute in the browser. Optional online information features naturally require internet access.
9. **Simpler distribution** — a release can be downloaded from GitHub and opened without managing a separate application package.

### Browser application vs. native executable

| Area | Browser-based HTML page | Native EXE |
|---|---|---|
| Installation | Usually open the page directly | Usually download / install or unpack |
| Updates | Replace the page | Install a new build |
| OS portability | Depends on browser and Web API support | Usually built per operating system |
| Hardware access | WebUSB / WebHID permissions and browser support | Native drivers and application APIs |
| Network IQ | Can use browser WebSocket | Depends on application implementation |
| Distribution | One HTML file / GitHub release | Executable and often supporting files |
| System integration | Limited to browser permissions and APIs | Typically deeper OS integration |

A web page is not universally superior to an EXE. Native software can provide broader hardware-driver support, lower-level device access, background operation, and deeper operating-system integration. This project chooses the browser model for convenient distribution, portability, and a unified UI.

## Running the application

1. Download `WEB_UI_SDR_Radio_YO2LDK.html` from this repository.
2. Open it in a recent version of **Google Chrome** or **Microsoft Edge**.
3. Select **Demo** to explore the interface, or choose the SDR hardware / IQ source you intend to use.
4. For direct USB/HID receivers, connect the device and press **Start**. Select the device in the browser permission dialog when prompted.
5. For WebSocket IQ, open **Hardware**, enter the WebSocket endpoint and select the matching IQ sample format before starting.
6. Use the **Band**, **Receive**, **Tools**, **Spectrum**, **Hardware**, and **Info** menus to configure the receiver and interface.

### Important browser requirements

- WebUSB and WebHID are supported by Chromium-based browsers on supported platforms. Firefox and Safari may not expose the APIs required by all direct-device modes.
- Browser security rules may restrict powerful device APIs when a page is opened from an insecure remote origin. For hosted use, serve the page over HTTPS. Local-file behavior can vary by browser and API.
- Device drivers and permissions still matter. For example, the RX888 may require a suitable WinUSB driver on Windows.
- FUNcube audio capture requires the correct stereo audio input to be selected and configured in the operating system. A 2-channel, 192 kHz input is recommended for full bandwidth when the device and audio driver support it.
- WebSocket mode requires a compatible IQ server reachable from the browser. The page does not itself install or launch `rtl_tcp` or another server.
- Some information ticker features contact third-party online data providers and therefore require internet access.

## Privacy and permissions

The browser asks for access to USB, HID, audio input, or other capabilities only when the corresponding feature is used. Device permissions are managed by the browser. The optional location lookup uses an IP-based location service unless a location or locator is entered manually. Review the browser's site permissions and the external providers' policies when using hosted deployments.


## Disclaimer

This is a receive-oriented SDR interface. Actual capabilities depend on the connected hardware, firmware, browser, operating system, and IQ source. Always comply with local radio regulations and use appropriate RF safety practices. The information ticker is informational and should not be used as an emergency-warning service.

---

**WEB UI SDR Radio - YO2LDK**  
*Browser-based SDR reception and signal-analysis interface.*
