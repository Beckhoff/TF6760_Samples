# TF6760 HTTPS/REST Sample – OpenWeatherMap

## Overview
This TwinCAT 3 sample demonstrates communication with an HTTPS/REST API using the `Tc3_IotBase` library. It sends HTTPS `GET` requests to the public [OpenWeatherMap](https://openweathermap.org/api) REST API to query current weather data and parses the JSON response with `FB_JsonDomParser`.

## What this sample demonstrates
- Configuring `FB_IotHttpClient` for a TLS connection with keep-alive and a connection timeout
- Sending an HTTP `GET` request with query parameters (location and application ID) (`FB_TestHTTP_Get_openWeatherMap`)
- Parsing a nested JSON response and reading a numeric member (`coord.lon`) with `FB_JsonDomParser`. The weather data arrives in the PLC as a plain JSON string and cannot be assigned to PLC variables directly. The DOM parser turns it into a navigable document, so single members can be looked up by name — even inside nested objects — and converted to the matching IEC 61131-3 data type, without any string handling in the application code.

## Prerequisites
- TwinCAT 3 XAE engineering environment installed on Windows
- A TwinCAT 3 XAR runtime or compatible target system that can run the PLC project
- TF6760 TC3 IoT HTTPS/REST license installed on your target system (7-day trial license possible)
- The `Tc3_IotBase` and `Tc3_JsonXml` libraries installed on your target system
- A free [OpenWeatherMap account](https://openweathermap.org/) to obtain an application ID (API key)
- Internet access from the target system to reach `api.openweathermap.org` on port `443`

## Getting Started
1. Create an OpenWeatherMap account and obtain your application ID (API key).
2. Open the solution file [IotHttpSampleOpenWeatherMap.sln](IotHttpSampleOpenWeatherMap.sln).
3. In `FB_TestHTTP_Get_openWeatherMap`, replace the `APPID` value in the request URI with your own application ID and adjust the `lat`/`lon` location parameters as needed.
4. Build, activate, and start the PLC application.
5. Set `bGetOpenWeatherMap := TRUE` to query the weather data. Inspect the parsed values and counters in `fbHttpGetOpenWeatherMap`.

## Notes & Troubleshooting
- The request URI combines the application ID with a location identifier. Besides latitude/longitude, OpenWeatherMap also accepts a city ID or city name. See the [OpenWeatherMap API documentation](https://openweathermap.org/api) for further use cases such as forecasts.
- `stTLS.bNoServerCertCheck := TRUE` disables server certificate validation. This is convenient for testing, but it should not be used in production. Deploy the appropriate CA certificate and enable certificate checking instead.
- A newly created OpenWeatherMap application ID can take some time to become active. Until then the API returns a `401 Unauthorized`.
- The response validation in the sample compares the returned longitude against a fixed value. Adjust or remove this check when you query a different location.
- If a request fails, inspect `fbRequest.bError`, `fbRequest.nStatusCode`, and the `nErrCount` counter.

## Support
For questions about this sample, contact your local Beckhoff support team. Contact information is available on the official Beckhoff website at https://www.beckhoff.com/contact/.

## Further information
- [TF6760 documentation](https://infosys.beckhoff.com/content/1031/tf6760_tc3_iot_https_rest/index.html) in the Beckhoff Information System
- [Overview of all samples](../README.md) in this repository
