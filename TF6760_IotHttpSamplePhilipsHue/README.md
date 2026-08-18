# TF6760 HTTPS/REST Sample – Philips Hue

## Overview
This TwinCAT 3 sample demonstrates how to control [Philips Hue](https://www.philips-hue.com/) smart home devices from the PLC using the `Tc3_IotBase` library. It sends HTTPS `PUT` requests to the REST API of a Philips Hue Bridge to change the state of a light (on/off, saturation, brightness, and color), including a "blinking mode" that cyclically changes the color.

## What this sample demonstrates
- Configuring `FB_IotHttpClient` for a TLS connection to a Philips Hue Bridge in the local network
- Building a JSON payload with `FB_JsonSaxWriter` (`on`, `sat`, `bri`, `hue`)
- Sending an HTTPS `PUT` request to change a light's state (`FB_TestHTTP_Put_PhilipsHue`)
- Reading the JSON status response with `FB_JsonDomParser`. The bridge answers with a JSON document that arrives in the PLC as a plain string and cannot be assigned to PLC variables directly. `GetJsonDomContent()` turns it into a navigable document, from which single members can be looked up by name and converted to the matching IEC 61131-3 data type, without any string handling in the application code. The sample only verifies that the document could be parsed — add your own member lookups at the marked place in the code.
- Cyclically toggling the request from `MAIN` to implement a color-changing "blinking mode"

## Prerequisites
- TwinCAT 3 XAE engineering environment installed on Windows
- A TwinCAT 3 XAR runtime or compatible target system that can run the PLC project
- TF6760 TC3 IoT HTTPS/REST license installed on your target system (7-day trial license possible)
- The `Tc3_IotBase` and `Tc3_JsonXml` libraries installed on your target system
- A Philips Hue Bridge reachable in your network with at least one connected light
- A Philips Hue Bridge API user name (see Getting Started)
- Network access from the target system to the Hue Bridge on port `443`

## Getting Started
1. Determine the IP address of your Hue Bridge in your local network and create an API user name for the bridge. The Philips ["Get started" tutorial](https://developers.meethue.com/develop/get-started-2/) explains how to find the bridge and generate a user name.
2. Open the solution file [IotHttpSamplePhilipsHue.sln](IotHttpSamplePhilipsHue.sln).
3. In the `MAIN` program, set `sHostName` to the IP address of your Hue Bridge (the sample contains the example address `192.168.0.100`).
4. In `FB_TestHTTP_Put_PhilipsHue`, replace the `censored` placeholder and the light number in the request URI (`/api/<user-name>/lights/<id>/state`) with your own values.
5. Build, activate, and start the PLC application.
6. Set `bPutPhilipsHue := TRUE` to send a single state update, or set `bBlinkingMode := TRUE` to cyclically change the light color.

## Notes & Troubleshooting
- In blinking mode, `MAIN` uses a 500 ms timer to increment the color value `nColor` (wrapping between `0` and `65000`) and re-trigger the request.
- The Hue Bridge REST API is only available over HTTPS, so the sample connects on port `443` (`nHostPort := 443`).
- `stTLS.bNoServerCertCheck := TRUE` disables server certificate validation. The bridge presents a certificate that is not issued by a publicly trusted CA, so the check would otherwise fail. If you want to enable certificate checking, retrieve the certificate of your bridge and deploy it to the target system as a trusted CA certificate (`stTLS.sCA`).
- The API user name is generated once during the bridge setup process and must be included in every request URI. Keep it — it acts as your access credential to the bridge.
- Make sure the light number in the request URI matches an existing light on your bridge.
- If a request fails, inspect `fbRequest.bError`, `fbRequest.nStatusCode`, and the `nErrCount` counter. The bridge also returns error objects in its JSON response body.

## Support
For questions about this sample, contact your local Beckhoff support team. Contact information is available on the official Beckhoff website at https://www.beckhoff.com/contact/.

## Further information
- [TF6760 documentation](https://infosys.beckhoff.com/content/1031/tf6760_tc3_iot_https_rest/index.html) in the Beckhoff Information System
- [Overview of all samples](../README.md) in this repository
