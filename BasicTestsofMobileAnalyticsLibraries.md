#Basic Tests of Mobile Analytics Libraries
Author: Julian Harty

Started: 16th Aug 2015

Status: work-in-progress, feedback is welcome as issues on this github repository.

##Context
I’d like to establish a basic set of tests to apply to each Mobile Analytics Library so we have some consistency when assessing each library and don’t miss out relevant topics.

##Connectivity
Connectivity can be in one of several states:

1. Catatonic: no connection beyond the device, Wi-Fi disabled, ditto mobile network. 
   1. Network Roaming disabled for data may be a variation of either 1 or 2, or a distinct state.
1. Non-internet: e.g. on a local Wi-Fi network that doesn’t have a route to the Internet such as the RPI network provided by a RACHEL server (not the one on the Internet, a local one).
2. Internet access: with reach to the M.A. network endpoint.
3. Proxied: Presumably where the proxy connects to the Internet, however we could also use a proxy on a non-Internet Wi-Fi, etc. for instance to capture the traffic and network behaviours in these conditions.

###Connectivity Tests
In each case the desirable behaviour is that events reach the network endpoint as soon as practical, and are stored if the endpoint cannot be reached. Potentially, the library may discard old data if some (as yet unknown) criteria are reached, for instance the number of records, the delay e.g. > 24 hours, etc.

####Static Analysis Checks of Connectivity
On Android, if an app does not require check-network-state (`android.permission.ACCESS_NETWORK_STATE`) it may be vulnerable to losing records when an internet connection is not available. See http://developer.android.com/training/basics/network-ops/managing.html

####Connectivity Protocols
The choices of connectivity protocols can materially affect the connectivity and transmission of data from the device to the network endpoint. Likely protocols include: UDP, TCP,HTTPS, web-sockets, …? 

##Timezones
Users are likely to be spread across various timezones in the world, and they may travel from one timezone to another. Devices may autoupdate their time and date settings, and do so several times while records are queued on the device (where queuing is supported and effective). What are the effects on the records written & subsequently transmitted to the MA endpoint? 

###Timezone Tests
Are records aligned to a consistent timezone e.g. UTC, Pacific Time, etc? How are changes in TZ handled, especially as TZs can change on different dates in different regions. Get testing done in places with ½ hour variations e.g. India. How are delayed records handled? What about users who travel across TZ’s e.g. flying from Europe to USA where the device has queued records and uploads them on arrival?

* Crossing dateline
* Crossing several timezones, including flip-flop changes, while the app is a) in-use b) queuing transmissions.

And BTW: how much would variations affect the validity of tests for the app (rather than for the M.A. library) derived from the flawed records.

##Completeness
For automatically instrumenting libraries, such as AppPulse Mobile, how completely do they cover functionality of the app? for instance, AppPulse Mobile doesn’t seem to capture the ReadAloud page action in Kiwix. What isn’t captured may be assumed to be unused, possibly an unsafe assumption.

###Completeness of meta-data
Information such as the model, locale, active language on device, available storage, etc. can help improve the quality of analysis and testing. I probably need to establish what ‘adequacy’ of such data would be i.e. what do I believe is adequate, and then use this as a baseline. 

###Granularity
How granular, how detailed, should the gathered data be?

###Completeness Tests

###Sufficiency
Completeness in terms of capturing pertinence, sufficient information to be able to use productively. Conversely, what’s missing that we’d like to use in order to improve the testing?

Rather than testing sufficiency, I’ll start by trying to (re)construct what happened from the information reported in Mobile Analytics Reports, and any processed data.

##Efficiency
Efficiency of the transmissions is an important acceptance criteria, data volumes, etc. need to be recorded and understood and deemed acceptable.

###Efficiency measures

* Efficiency in the additional code added to the app (assuming the MA library is integrated rather than running as an independent service on the device).
* Efficient use of storage on the device, when events, messages, etc. are queued
* Efficient use of network connectivity: including transmission units (acknowledged as discrete items so they don’t need to be retransmitted).

##Performance
There are various performance considerations and therefore tests that may be relevant. These include:

* Added latency when the app starts, particularly if the libraries are initialised on the main UI thread.
* CPU, RAM and local storage (for instance if mobile analytics events are buffered to the file system, including a local database).
* Use of the network when transmitting data between the library and its upstream data collection server.

##Security

* Confidentiality
* Integrity
* Availability

###Topics to consider
* SSL pinning
* Use of encryption.
* Faking information (probably quite easy to do). Could someone send fake data on behalf of another app? Digital signing of data?

##Further reading
* [SafeDK](http://www.safedk.com/) provide products to help manage third-party libraries in a mobile app. They also provide perspectives on how to assess and measure various characteristics, or behaviours, of SDKs, including mobile analytics libraries.
* [Twitter's Answers](https://answers.io/) service includes various interesting blog articles, particularly relevant is the following https://answers.io/blog/handling-five-billion-sessions-a-day-in-real-time which describes how they designed and implemented the system to cope with large volumes of sessions.
