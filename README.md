# About this repository
This repository contains official sample code for the product TF6760|TC3 IoT HTTPS/REST offered by [Beckhoff Automation](https://www.beckhoff.com). The sample code is provided as-is under the Zero-Clause BSD license.

# Samples overview

Each sample is a self-contained TwinCAT solution with a single PLC project that uses the `FB_IotHttpClient` and `FB_IotHttpRequest` function blocks of the `Tc3_IotBase` library. Open the solution of the sample you are interested in, then build and activate it — only that sample is downloaded to the target system. A sample stays idle until you set the corresponding trigger variable in its `MAIN` program.

| Folder | Description |
|---|---|
| [TF6760_IotHttpSamplePostman](TF6760_IotHttpSamplePostman/) | Communicates with the Postman Echo test web service. Contains samples for HTTP `GET`, `POST`, and `PUT`, plus a `GET` request with HTTP header authentication. |
| [TF6760_IotHttpSampleOpenWeatherMap](TF6760_IotHttpSampleOpenWeatherMap/) | Queries current weather data from the OpenWeatherMap REST API via HTTP `GET` and parses the JSON response. |
| [TF6760_IotHttpSamplePhilipsHue](TF6760_IotHttpSamplePhilipsHue/) | Controls a Philips Hue light through the local Hue Bridge REST API using HTTPS `PUT`, including a color-changing "blinking mode". |
| [TF6760_IotHttpSampleTelegram](TF6760_IotHttpSampleTelegram/) | Sends messages from the PLC to a Telegram chat via a Telegram bot using the Telegram Bot API. |
| [TF6760_IotHttpSampleAwsIotCore](TF6760_IotHttpSampleAwsIotCore/) | Communicates with AWS IoT Core over HTTPS using mutual TLS: publishes to an MQTT topic and reads a Device Shadow. |
| [TF6760_IotHttpSampleAwsSigV4](TF6760_IotHttpSampleAwsSigV4/) | Calls an AWS service REST API (Amazon EC2) with an HTTP `GET` request signed using AWS Signature Version 4. |

# How to get support
Should you have any questions regarding the provided sample code, please contact your local Beckhoff support team. Contact information can be found on the official Beckhoff website at https://www.beckhoff.com/contact/.

# Further information
Further information about this sample code can be found on the [Beckhoff Information System](https://infosys.beckhoff.com) in the [TF6760 documentation](https://infosys.beckhoff.com/content/1031/tf6760_tc3_iot_https_rest/index.html).
