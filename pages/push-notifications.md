title: "REST is not enough: Using Push Notifications to better support your mobile clients"


###Current Model
* Polling
* Smartphone use around the world isn't growing, it's exploding
* Most social networks are now mobile first
    * Much higher mobile usage on a lot of social networks
* Polling model is not efficient and it is even worse with mobile devices
    * CPU limitations
    * Battery
    * Network

###What's a push notifications?
* Displays a short text message
* Play a brief sound
* iOS: set a number in a badge on the app's icon
* Android: display a banner on the notification bar

###General Architecture

Provider -> PN Service -> Device -> Client App

* Sent as JSON

###Downsides
* Not reliable
    * No guarantee the push will be delivered
* Stale information
    * Expirations

###What you need to get started with Apple Push Notifications
* iOS app
* Provisioning profile
* Certificate
* Device tokens
* Payload (aka your messages)

###Obtaining the certificate
* Similar to any other SSL cert
* Generate CSR
* Upload it to Apple
* Link it to your App ID
* Enable push notifications
* Generate certificate

###Best way to do APNs on Python
Use PyAPNS
* On PyPI, pip install apns
* https://github.com/djacobs/PyAPNs

##Google Cloud Messaging (GCM)

###What you need to get started
* Android app
* Sender Id
* Application ID (aka Package name)
* Registration ID
* Google User Account
* Sender Auth Token
* Payload (aka your messages)

###2 Implementations
* GCM HTTP
    * Uses HTTP Post
    * Downstream only
    * Easy to implement
* GCM Cloud Connection Server (CCS)
    * Based on XMPP
    * 2 way protocol
    * Device can also send messages back to the server

### SleekXMPP
* On PyPI, pip install sleekxmpp

###3rd party alternatives
* Parse
    * Does more than push notifications
    * Cross platform SDKs
    * Uses a very simple REST API for sending PNs
    
###Downsides with Parse
* No official Python SDK
    * 3rd party "recommendeded" SDK
* Issues with mobile SDK
    * Specfically on Android
    
###Urban Airship
* Push is still their main product
* Simple REST API
* Cross platform SDKs


