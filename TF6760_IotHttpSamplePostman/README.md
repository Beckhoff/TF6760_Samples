# TF6760 HTTPS/REST Sample – Postman Echo

## Overview
This TwinCAT 3 sample demonstrates the basic HTTP request methods of the `Tc3_IotBase` library against the public [Postman Echo](https://learn.postman.com/docs/developer/echo-api/) test service (`postman-echo.com`). It sends `GET`, `POST` and `PUT` requests over TLS, performs an authenticated request using an HTTP `Authorization` header, and parses the JSON responses returned by the echo server.

## What this sample demonstrates
- Configuring a single `FB_IotHttpClient` with keep-alive and a connection timeout
- Sending an HTTP `GET` request with query parameters (`FB_TestHTTP_Get`)
- Sending an HTTP `POST` request with a string body (`FB_TestHTTP_Post`)
- Sending an HTTP `PUT` request with a string body (`FB_TestHTTP_Put`)
- Adding an `Authorization` header via `FB_IotHttpHeaderFieldMap` for HTTP Basic authentication (`FB_TestHTTP_HeaderAuth`)
- Evaluating the HTTP status code and parsing JSON responses with `FB_JsonDomParser` (`GetJsonDomContent`). The echo service answers with a JSON document that arrives in the PLC as a plain string and cannot be assigned to PLC variables directly. The DOM parser turns it into a navigable document, so single members can be looked up by name and converted to the matching IEC 61131-3 data type, without any string handling in the application code.

## Prerequisites
- TwinCAT 3 XAE engineering environment installed on Windows
- A TwinCAT 3 XAR runtime or compatible target system that can run the PLC project
- TF6760 TC3 IoT HTTPS/REST license installed on your target system (7-day trial license possible)
- The `Tc3_IotBase` and `Tc3_JsonXml` libraries installed on your target system
- Internet access from the target system to reach `postman-echo.com` on port `443`

## Getting Started
1. Open the solution file [IotHttpSamplePostman.sln](IotHttpSamplePostman.sln).
2. Build the solution and activate the configuration on your target system.
3. Log in to and start the PLC application.
4. In the `MAIN` program, set one of the trigger variables to `TRUE` to send a request:
   - `bGet` – HTTP GET
   - `bPost` – HTTP POST
   - `bPut` – HTTP PUT
   - `bHeaderAuth` – authenticated GET
5. Inspect the request/response counters and parsed values inside the corresponding function block instance (e.g. `fbHttpGet.sResultValue`, `nValidResCount`).

## Notes & Troubleshooting
- `stTLS.bNoServerCertCheck := TRUE` disables server certificate validation. This is convenient for testing, but it should not be used in production. Instead, deploy the CA certificate that signed the server certificate to the target and enable certificate checking.
- Make sure the target system has outbound internet access to `postman-echo.com` on port `443`. Check firewall and proxy settings if the connection fails.
- The Basic authentication header value `cG9zdG1hbjpwYXNzd29yZA==` is the Base64 encoding of the Postman Echo demo credentials `postman:password`. Replace it with your own credentials for other services.
- The samples parse the response with `GetJsonDomContent()`. Each function block also contains a commented-out `GetContent()` call for reading the raw response body instead.
- If a request fails, inspect `fbRequest.bError`, `fbRequest.nStatusCode`, and the `nErrCount` counter of the corresponding function block.

## Support
For questions about this sample, contact your local Beckhoff support team. Contact information is available on the official Beckhoff website at https://www.beckhoff.com/contact/.

## Further information
- [TF6760 documentation](https://infosys.beckhoff.com/content/1031/tf6760_tc3_iot_https_rest/index.html) in the Beckhoff Information System
- [Overview of all samples](../README.md) in this repository
