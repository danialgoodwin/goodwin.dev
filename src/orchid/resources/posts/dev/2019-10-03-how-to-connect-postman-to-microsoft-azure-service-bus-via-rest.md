---
title: How to connect Postman to Microsoft Azure Service Bus via REST
tags:
- microsoft
- azure
- azure service bus
- rest
- postman
---

The title says it all, so let's jump right in!

1. Create SAS token
    1. Create a new C# project with the following code:
    
            using System;
            using System.Globalization;
            using System.Security.Cryptography;
            using System.Text;
            using System.Web;
            
            class MainClass
            {
            
                static readonly string queueUrl = "TODO"; // Format: "https://<service bus namespace>.servicebus.windows.net/<topic name or queue>/messages";
                static readonly string signatureKeyName = "TODO";
                static readonly string signatureKey = "TODO";
                static readonly TimeSpan timeToLive = TimeSpan.FromDays(1);
            
                public static void Main(string[] args)
                {
                    //var sastoken = GetSasToken(queueUrl, "RootManageSharedAccessKey", "dfyxdAsxSBBKnGpLsMKGe9X8Bism9u0cpuNfgdty1EY=", TimeSpan.FromDays(1));
                    var token = GetSasToken(queueUrl, signatureKeyName, signatureKey, timeToLive);
                    Console.WriteLine("Authorization: " + token);
                }
            
                public static string GetSasToken(string resourceUri, string keyName, string key, TimeSpan ttl)
                {
                    var expiry = GetExpiry(ttl);
                    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
                    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
                    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
                    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}",
                    HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
                    return sasToken;
                }
            
                private static string GetExpiry(TimeSpan ttl)
                {
                    TimeSpan expirySinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1) + ttl;
                    return Convert.ToString((int)expirySinceEpoch.TotalSeconds);
                }
            
            }
            
    2. In the code, update the fields:
        - `queueUrl`
        - `signatureKeyName`
        - `signatureKey`
    3. Run the code. Save the output for the next step 'Setup Postman'
2. Setup Postman
    1. REST type: POST
    2. URL: `https://<service namespace>.servicebus.windows.net/<topic name or queue>/messages` (Ex: `https://myservicebus.servicebus.windows.net/my-topic/messages`)
    3. Headers:
        - Authorization: `SharedAccessSignature sr=https%3a%2f%2f<service namespace>.servicebus.windows.net%2f<topic name or queue>%2fmessages&sig=<signature hash>&se=<expiry time>&skn=<signature key name>` (Ex: `SharedAccessSignature sr=https%3a%2f%2fmyservicebus.servicebus.windows.net%2fmy-topic%2fmessages&sig=z4C.....3d&se=1570147127&skn=RootManageSharedAccessKey`)
        - Content-Type: application/xml
    4. Body: Set to 'raw' and add something similar to the following: `<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">This is message.</string>`
3. In Postman, click 'Send'!
    - The response status will be '201 Created' if completed successfully





Helpful resources:
- https://stackoverflow.com/questions/50914924/send-msg-to-azure-service-bus-que-via-rest


