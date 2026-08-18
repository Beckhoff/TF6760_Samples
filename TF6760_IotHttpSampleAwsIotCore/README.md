# TF6760 HTTPS/REST Sample – AWS IoT Core

## Overview
This TwinCAT 3 sample demonstrates how to communicate with [AWS IoT Core](https://docs.aws.amazon.com/iot/) over its HTTPS interface using the `Tc3_IotBase` library. It authenticates with mutual TLS (X.509 device certificate and private key), publishes a message to an MQTT topic via the HTTP publish endpoint, and reads a Device Shadow document.

## What this sample demonstrates
- Configuring `FB_IotHttpClient` for mutual TLS using a CA certificate, a device certificate, and a private key (`stTLS.sCA`, `stTLS.sCert`, `stTLS.sKeyFile`)
- Publishing a message to an MQTT topic through the AWS IoT HTTP endpoint `POST /topics/<topic>?qos=1` (`FB_TestHTTP_Post_awsIot`)
- Building a JSON payload with `FB_JsonSaxWriter`
- Reading an AWS IoT Device Shadow via `GET /things/<thingName>/shadow` (`FB_TestHTTP_Get_awsIotShadow`)
- Evaluating the HTTP status code and parsing JSON responses with `FB_JsonDomParser`. AWS IoT Core answers with a JSON document that arrives in the PLC as a plain string and cannot be assigned to PLC variables directly. `GetJsonDomContent()` turns it into a navigable document, from which single members can be looked up by name and converted to the matching IEC 61131-3 data type, without any string handling in the application code. The publish sample evaluates the `message` member of the response; the shadow sample only verifies that the document could be parsed — add your own member lookups at the marked place in the code.

## Prerequisites
- TwinCAT 3 XAE engineering environment installed on Windows
- A TwinCAT 3 XAR runtime or compatible target system that can run the PLC project
- TF6760 TC3 IoT HTTPS/REST license installed on your target system (7-day trial license possible)
- The `Tc3_IotBase` and `Tc3_JsonXml` libraries installed on your target system
- An AWS account with an AWS IoT Core "Thing", a device certificate, and a policy that allows the `iot:Publish` and `iot:GetThingShadow` actions
- The Amazon Root CA certificate, the device certificate, and the private key deployed to the target system
- Internet access from the target system to your AWS IoT Core endpoint on port `8443`

## Getting Started
1. In the [AWS IoT Core console](https://console.aws.amazon.com/iot/), create a Thing, generate a device certificate and key pair, and attach a suitable policy.
2. Copy the Amazon Root CA (`AmazonRootCA1.pem`), the device certificate, and the private key to your target system.
3. Open the solution file [IotHttpSampleAwsIotCore.sln](IotHttpSampleAwsIotCore.sln).
4. In the `MAIN` program, adjust the client configuration to match your environment:
   - `sHostName` – your account-specific AWS IoT endpoint (e.g. `xxxxxxxx-ats.iot.<region>.amazonaws.com`)
   - `stTLS.sCA`, `stTLS.sCert`, `stTLS.sKeyFile` – the file paths of your certificates and key on the target
   - `nHostPort` – leave it at `8443`, the AWS IoT HTTPS port for client certificate authentication (see Notes)
5. Adjust the request URIs to match your environment:
   - `FB_TestHTTP_Post_awsIot` – replace `mytopic` in `/topics/mytopic?qos=1` with the MQTT topic you want to publish to
   - `FB_TestHTTP_Get_awsIotShadow` – replace `thingName` in `/things/thingName/shadow` with the name of your Thing
6. Build, activate, and start the PLC application.
7. Set `bPostAwsIot := TRUE` to publish a message, or `bGetAwsIotShadow := TRUE` to read the Device Shadow.

## Notes & Troubleshooting
- The sample connects on port `8443` (`nHostPort := 8443`). With X.509 client certificate authentication, AWS IoT Core offers the HTTPS interface on port `8443` without further TLS extensions. Port `443` also supports client certificates, but only if the client announces the ALPN protocol name `x-amzn-http-ca` during the TLS handshake. See [Protocols, port mappings, and authentication](https://docs.aws.amazon.com/iot/latest/developerguide/protocols.html#protocol-mapping).
- Authentication with AWS IoT Core requires a valid device certificate, private key, and the Amazon Root CA. Make sure the file paths in `MAIN` point to the correct locations on the target system.
- The endpoint host name is account- and region-specific. Retrieve it from the AWS IoT console (Settings → Device data endpoint) or via `aws iot describe-endpoint`.
- The attached AWS IoT policy must grant the actions used by the sample (`iot:Publish`, `iot:GetThingShadow`) for the corresponding resources — that is, the topic ARN used in the publish request and the Thing ARN used in the shadow request.
- If a request fails, inspect `fbRequest.bError`, `fbRequest.nStatusCode`, and the `nErrCount` counter of the corresponding function block. A `403` typically indicates a policy or certificate problem.

## Support
For questions about this sample, contact your local Beckhoff support team. Contact information is available on the official Beckhoff website at https://www.beckhoff.com/contact/.

## Further information
- [TF6760 documentation](https://infosys.beckhoff.com/content/1031/tf6760_tc3_iot_https_rest/index.html) in the Beckhoff Information System
- [Overview of all samples](../README.md) in this repository
