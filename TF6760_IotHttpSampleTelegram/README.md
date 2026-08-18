# TF6760 HTTPS/REST Sample – Telegram

## Overview
This TwinCAT 3 sample demonstrates how to send messages from a TwinCAT PLC to a [Telegram](https://telegram.org/) chat using the `Tc3_IotBase` library. With the help of a so-called *Telegram bot*, it sends an HTTP `GET` request to the Telegram Bot API to deliver a text message to a specific chat.

## What this sample demonstrates
- Configuring `FB_IotHttpClient` for a TLS connection to the Telegram Bot API
- Building a request URI dynamically with `FB_FormatString` (bot token, chat ID, message text)
- Sending an HTTP `GET` request to the Telegram `sendMessage` method (`FB_TestHTTP_Get_Telegram`)
- Evaluating the HTTP status code and parsing the JSON response with `FB_JsonDomParser`. The Bot API answers with a JSON document that arrives in the PLC as a plain string and cannot be assigned to PLC variables directly. `GetJsonDomContent()` turns it into a navigable document, from which single members — such as the `ok` flag or the message ID — can be looked up by name and converted to the matching IEC 61131-3 data type, without any string handling in the application code. The sample only verifies that the document could be parsed — add your own member lookups at the marked place in the code.

## Prerequisites
- TwinCAT 3 XAE engineering environment installed on Windows
- A TwinCAT 3 XAR runtime or compatible target system that can run the PLC project
- TF6760 TC3 IoT HTTPS/REST license installed on your target system (7-day trial license possible)
- The `Tc3_IotBase`, `Tc3_JsonXml`, and `Tc2_Utilities` libraries installed on your target system
- A Telegram account and a Telegram bot (see Getting Started)
- Internet access from the target system to reach `api.telegram.org` on port `443`

## Getting Started
1. In Telegram, contact the BotFather to create a new bot. The BotFather returns an API token that authenticates requests to the Telegram Bot API. See the [Telegram Bot API documentation](https://core.telegram.org/bots/api) for details.
2. Determine the chat ID of the chat you want to send messages to. To do this, send a message to your bot (via the Telegram app or browser) and then call the `getUpdates` method of the Bot API (for example with Postman) to read the chat ID from the response.
3. Open the solution file [IotHttpSampleTelegram.sln](IotHttpSampleTelegram.sln).
4. In `FB_TestHTTP_Get_Telegram`, enter your credentials (replace the `censored` placeholders):
   - `sBotApiKey` – the API token returned by the BotFather
   - `sChatId` – the target chat ID
5. Optionally adjust the message text in `MAIN` (`sMessage`).
6. Build, activate, and start the PLC application.
7. Set `bGetTelegram := TRUE` to send the message. The message should appear in the target Telegram chat.

## Notes & Troubleshooting
- Requests to the Telegram Bot API follow the scheme `https://api.telegram.org/bot<token>/<METHOD_NAME>`. The sample assembles the URI for the `sendMessage` method at runtime with `FB_FormatString`.
- `stTLS.bNoServerCertCheck := TRUE` disables server certificate validation. This is convenient for testing, but it should not be used in production. The repository includes a root certificate (`Certificate/TelegramRoot.cer`) that can be deployed to the target so that certificate checking can be enabled instead.
- The bot API token is a credential: anyone who knows it can control the bot. For demonstration purposes the sample declares it as an initial value in the POU, which stores it in plain text in the project source. In a real application, keep the token out of the source code and assign `sBotApiKey` at runtime instead, for example from a configuration file on the target system, from a `VAR PERSISTENT` variable, or from an operator input in the HMI. If a token has been disclosed, revoke it with the BotFather command `/revoke` and generate a new one.
- The bot can only send messages to a chat after the user has started a conversation with it. Use the `getUpdates` method to obtain the correct chat ID.
- Message text sent as a URL query parameter must be URL-encoded if it contains special characters.
- If a request fails, inspect `fbRequest.bError`, `fbRequest.nStatusCode`, and the `nErrCount` counter.

## Support
For questions about this sample, contact your local Beckhoff support team. Contact information is available on the official Beckhoff website at https://www.beckhoff.com/contact/.

## Further information
- [TF6760 documentation](https://infosys.beckhoff.com/content/1031/tf6760_tc3_iot_https_rest/index.html) in the Beckhoff Information System
- [Overview of all samples](../README.md) in this repository
