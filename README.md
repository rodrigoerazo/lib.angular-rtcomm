lib.angular-rtcomm
==================
This repository contains the Rtcomm Angular.js module. The Rtcomm Angular module exposes a set of real-time communication features in the form of a service and a number of Angular directives and controllers. 
This module is built on the [rtcomm.js](https://github.com/WASdev/lib.rtcomm.clientjs) open source JavaScript library which wraps [WebRTC](http://www.webrtc.org/) with call 
signaling and a number of advanced real-time communications capabilities like presence and chat.

##Requirements
Rtcomm utilizes MQTT for call signaling so at a minimum you will need an MQTT message broker that supports web sockets. There are many open source and productized versions of MQTT message brokers available. A great option is to download the WebSphere Liberty Server which includes an embedded MQTT broker and a number of backend Rtcomm services such as call queueing. The WebSphere Liberty server is free for development and the beta version that includes support for MQTT can be downloaded from [here](https://developer.ibm.com/wasdev/downloads/liberty-profile-beta/).

##Install
To install the angular-rtcomm module for use in an AngularJS application, you can use bower:

**bower install angular-rtcomm**

##Build
To build the angular-rtcomm module, you can use npm:

This is useful if you want to modify the angular-rtcomm templates.

##Test
This repository relies on Karma for testing. To run the test framework you can type the following in the root lib.angular-rtcomm directory:

**karma start**

Note that the test framework assumes the MQTT broker is running on localhost:9080. There is also currently a dependency on the WebSphere Liberty server (which includes an embedded MQTT broker) for some of the Rtcomm features. The link to download a version of WebSphere Liberty for development and testing can be found at the top of the page. 

We are working on an npm install of the development environment so stay tuned.

A sample page is also included in the test directory along with a sample config file that can be used to run the test. 

##Usage

###Initialization
To use the module, you will need to include the following js and css files with your project:

First the angular-rtcomm related files:
```html
	<script	src="bower_components/angular-rtcomm/dist/angular-rtcomm.js"></script>
	<link href="bower_components/angular-rtcomm/dist/css/angular-rtcomm.css" rel="stylesheet">
```
	
See the dist directory for minified versions.
	
In addition, the angular-rtcomm module relies on the following additional JavaScript libraries that can be accessed via various CDNs:

```html
	<link href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.1/css/bootstrap.min.css" rel="stylesheet">
	<link href="//cdn.rawgit.com/wix/angular-tree-control/master/css/tree-control-attribute.css" rel="stylesheet" type="text/css">
	
	<script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
	<script	src="//maxcdn.bootstrapcdn.com/bootstrap/3.3.1/js/bootstrap.min.js"></script>
	<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.3.5/angular.js"></script>
	<script	src="//cdnjs.cloudflare.com/ajax/libs/angular-ui-bootstrap/0.12.0/ui-bootstrap-tpls.min.js"></script>
	<script src="//cdn.rawgit.com/wix/angular-tree-control/master/angular-tree-control.js" type="text/javascript"></script>

  <script src="bower_components/bower-mqttws/mqttws31.js"></script>
  <script src="bower_components/webrtc-adapter/adapter.js"></script>
  <script src="bower_components/rtcomm/dist/rtcomm.js"></script>
``` 


**Note**  
The above dependencies can be accessed via CDN as above or via your bower_component install location.
We are also working to reduce the number of dependencies (or to at least provide more flexibility in when these dependencies are required).	


###Module injection
AngularJS applications inject the angular-rtcomm module as follows:

var sampleModule = angular.module('sampleModule', ['angular-rtcomm']);

###RtcommService
The RtcommService is the singleton that controls all aspects of the Rtcomm module. Its the only service AngularJS applications need to inject when using the angular-rtcomm module. 

The RtcommService broadcasts all its related events to the $rootScope. The events published by the RtcommService are a combination of events generated by the angular-rtcomm module and the rtcomm.js library described above that this module wraps. Here is the list of events:

* queueupdate
* newendpoint
* session:started
* session:alerting
* session:trying
* session:ringing
* session:queued
* session:failed
* session:stopped
* webrtc:connected
* webrtc:disconnected
* chat:connected
* chat:message
* destroyed
* rtcomm::init
* endpointActivated
* noEndpointActivated
* rtcomm::iframeUpdate

And the methods exposed by the RtcommService include the following:

* isInitialized 
* setConfig
* getPresenceMonitor    
* publishPresence
* addToPresenceRecord
* removeFromPresenceRecord
* setPresenceRecordState
* getEndpoint 
* destroyEndpoint
* register
* unregister 
* joinQueue
* leaveQueue
* getQueues
* sendChatMessage	
* getChats
* isWebrtcConnected
* getSessionState
* setAlias
* setUserID
* setPresenceTopic
* getIframeURL
* putIframeURL
* placeCall
* getSessions
* setActiveEndpoint
* getActiveEndpoint
* getRemoteEndpoint
* setDefaultViewSelector
* setViewSelector
* setVideoView


###List of AngularJS directives
Here is a complete list of all the currently supported directives.

####rtcomm-register:
Directive used to register an endpoint. Registration implies creation of a user record as a retained message on the configured presence topic which can be accessed by any endpoint or service that has permissions to subscribe on the registration topic. This is registration is required after initialization. If the user name is specified in advance, for example via a cookie, the userid can be specified when initializing the RtcommService: 
```html
<rtcomm-register></rtcomm-register>
```
No init parameters.

####rtcomm-queues:
Directive used to view and join any available call queue. Can be used by helpdesk agents (only availble when working with WebSphere): 
```html
<rtcomm-queues></rtcomm-queues>
```

####rtcomm-presence:
Directive used to view presence information: 
```html
<rtcomm-presence></rtcomm-presence>
```

####rtcomm-chat:
Directive used to display a chat window: 
```html
<rtcomm-chat></rtcomm-chat>
```

####rtcomm-video:
Directive that contains a WebRTC video window (both self and remote views): 
```html
<rtcomm-video></rtcomm-video>
```

####rtcomm-endpoint-status:
Directive that displays status about the active endpoint: 
```html
<rtcomm-endpoint-status></rtcomm-endpoint-status>
```

####rtcomm-sessionmgr:
Directive that manages multiple endpoints and their associated sessions. For instance, used by a helpdesk agent to manage multiple sessions: 
```html
<rtcomm-sessionmgr></rtcomm-sessionmgr>
```

###List of AngularJS Controllers

This controller can read configuration from a specified JSON file:
**RtcommConfigController**

This controller displays the alerting modal which allows the user to accept or reject an inbound call:
**RtcommAlertModalController**

This controller displays an outbound call modal:
**RtcommCallModalController** 

This doc is a work in progress. Still need to document all the init parameteres for each directive, more details on testing, etc.
