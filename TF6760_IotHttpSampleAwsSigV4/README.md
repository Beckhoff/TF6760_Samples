# TF6760 HTTPS/REST Sample – AWS Signature Version 4

## Overview
This TwinCAT 3 sample demonstrates how to call an AWS service REST API that requires [AWS Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) request signing. Using the `Tc3_IotBase` library, it signs an HTTP `GET` request to the Amazon EC2 API with an access key and secret key and evaluates the XML response.

## What this sample demonstrates
- Signing an HTTP request with AWS Signature Version 4 using `FB_IotHttpAwsSig4HeaderFieldMap`
- Providing the signing parameters (service, region, access key, secret key, signed headers) via `SetParameter()`
- Sending a signed `GET` request to the Amazon EC2 API (`FB_TestHTTP_Get_AwsSigV4`)
- Reading the raw XML response body with `GetContent()`

## Prerequisites
- TwinCAT 3 XAE engineering environment installed on Windows
- A TwinCAT 3 XAR runtime or compatible target system that can run the PLC project
- TF6760 TC3 IoT HTTPS/REST license installed on your target system (7-day trial license possible)
- The `Tc3_IotBase` library installed on your target system
- An AWS account with an IAM user/role and an access key / secret key that is allowed to call the targeted EC2 action
- Internet access from the target system to the AWS service endpoint on port `443`

## Getting Started
1. Create or obtain an AWS access key ID and secret access key for an IAM identity with permission to call the EC2 API.
2. Open the solution file [IotHttpSampleAwsSigV4.sln](IotHttpSampleAwsSigV4.sln).
3. In `FB_TestHTTP_Get_AwsSigV4`, enter your credentials and adjust the request parameters:
   - `sAccessKey`, `sSecretKey` – your AWS credentials (replace the `censored` placeholders)
   - `sService`, `sRegion` – the AWS service and region (default `ec2` / `us-east-1`)
   - `sRequestUrl` – the query string of the EC2 action to call. The default action `RunInstances` launches an EC2 instance and may incur AWS charges. For a read-only test, use the commented-out `DescribeInstances` action instead.
4. In `MAIN`, make sure `sHostName` matches the service and region (default `ec2.us-east-1.amazonaws.com`).
5. Build, activate, and start the PLC application.
6. Set `bGetAWSSigV4 := TRUE` to send the signed request. Inspect the response in `fbHttpGetAWSSigV4.sContent`.

## Notes & Troubleshooting
- The access key and secret key are shown as `censored` placeholders in the sample and must be replaced with your own credentials. For demonstration purposes they are declared as initial values in the POU, which stores them in plain text in the project source. In a real application, keep the secret key out of the source code and assign `sSecretKey` at runtime instead, for example from a configuration file on the target system, from a `VAR PERSISTENT` variable, or from an operator input in the HMI. Use an IAM identity whose permissions are limited to the actions the application actually needs, and rotate the access key if it has been disclosed.
- The IoT driver does not evaluate URL redirects, so connect directly to the exact regional endpoint (e.g. `ec2.us-east-1.amazonaws.com`) rather than to `ec2.amazonaws.com`.
- The query parameters of the request URL must be sorted alphabetically, otherwise the HTTP server returns an error. Sorting them is the responsibility of the application; the signed headers are sorted internally by the driver.
- `stTLS.bNoServerCertCheck := TRUE` disables server certificate validation. This is convenient for testing, but it should not be used in production. Deploy the appropriate CA certificate and enable certificate checking instead.
- Signature Version 4 relies on a correct request timestamp. Make sure the target system clock is accurate; otherwise AWS rejects the request with a signature/expiry error.
- If a request fails, inspect `fbRequest.bError`, `fbRequest.nStatusCode`, and the `nErrCount` counter. AWS returns error details in the XML response body.

## Support
For questions about this sample, contact your local Beckhoff support team. Contact information is available on the official Beckhoff website at https://www.beckhoff.com/contact/.

## Further information
- [TF6760 documentation](https://infosys.beckhoff.com/content/1031/tf6760_tc3_iot_https_rest/index.html) in the Beckhoff Information System
- [Overview of all samples](../README.md) in this repository
