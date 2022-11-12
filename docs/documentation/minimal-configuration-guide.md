---
title: Minimal Configuration Guide
---

<h2>Minimal Configuration Guide</h2>

<ul>
  <li><i>Added: 2013.09.30 by Johannes Findeisen (hanez)</i></li>
  <li><i>Last update: 2013.11.03 by Johannes Findeisen (hanez)</i></li>
  <li><i>License: <a href="http://creativecommons.org/licenses/by-sa/3.0/" class="ext">CC Attribution-Share Alike 3.0 Unported</a></i></li>
</ul>

<h3>Introduction</h3>

<p>This is step by step guide on howto setup Linspector for monitoring two services on two hosts. Be sure to always write <a href="http://en.wikipedia.org/wiki/JSON" class="ext">JSON</a> compatible syntax, e.g. add colons, commas etc. the right places or Linspector will fail to parse the configuration at startup.</p>

<h3>The base configuration</h3>

<p>At first we need a minimal json config file:</p>

<pre class="prettyprint linenums">{
  "members":{
  },
  "tasks":{
  },
  "periods": {
  },
  "hostgroups":{
  },
  "layouts":{
  },
  "core":{
  }
}</pre>

<h3>Hostgroups and Hosts</h3>

<p>Now we need to add a minimum of one hostgroup. Hosts needs always to be nested in a hostgroup. So if you just want to monitor one host, you need add one hostgroup too. We will add two hosts to that hostgroup (demo1.example.org and demo2.example.org):</p>

<pre class="prettyprint linenums">{
  "members":{
  },
  "tasks":{
  },
  "periods": {
  },
  "hostgroups":{
    "demogroup1":{
      "hosts": ["demo1.example.org", "demo2.example.org"]
    }
  },
  "layouts":{
  },
  "core":{
  }
}</pre>

<h3>Services</h3>

<p>We want to monitor the availability of port 80 (http) and 25 (smtp) now so we need to add two services of class "tcpconnect":</p>

<pre class="prettyprint linenums">{
  "members":{
  },
  "tasks":{
  },
  "periods": {
  },
  "hostgroups":{
    "demogroup1":{
      "hosts": ["demo1.example.org", "demo2.example.org"],
      "services":[
      {
        "class": "tcpconnect",
        "args": { "port": 80 },
        "periods": ["normal"],
        "threshold": 10
      },
      {
        "class": "tcpconnect",
        "args": { "port": 25 },
        "periods": ["normal"],
        "threshold": 10
      }
      ]
    }
  },
  "layouts":{
  },
  "core":{
  }
}</pre>

<h3>Periods / Polling Intervals</h3>

<p>Since we have a period (polling interval) set to "normal" we need to add a peroid to our base configuration. This peroid is an interval peroid which polls every 2 minutes. Other peroid types are cron based which accepts cron style parameters and date based which accepts a date and executes on that date.</p>

<pre class="prettyprint linenums">{
  "members":{
  },
  "tasks":{
  },
  "periods": {
    "normal": { "seconds": 120 }
  },
  "hostgroups":{
    "demogroup1":{
      "hosts": ["demo1.example.org", "demo2.example.org"],
      "services":[
      {
        "class": "tcpconnect",
        "args": { "port": 80 },
        "periods": ["normal"],
        "threshold": 10
      },
      {
        "class": "tcpconnect",
        "args": { "port": 25 },
        "periods": ["normal"],
        "threshold": 10
      }
      ]
    }
  },
  "layouts":{
  },
  "core":{
  }
}</pre>

<h3>Members and Tasks</h3>

<p>As you can see we have set a "threshold" to 10, so what is that? The threshold indicates after how many fails of polling a service an alert will be executed. We will come to that next. Alerting means that there must be performed some action when the threshold is overriden. For that Linspector has it's members in the base configuration. We will add a member which accepts alerts via email so we are adding a task of type "mail" for that. So let's add a member and it's task:</p>

<pre class="prettyprint linenums">{
  "members": {
    "demo": {
      "name": "Demo Admin",
      "tasks": [
        { "class":"mail", "type": "mail", "args": { "rcpt": "demo@example.org" }}
      ]
    }
  },
  "tasks":{
  },
  "periods": {
    "normal": { "seconds": 120 }
  },
  "hostgroups": {
    "demogroup1": {
      "hosts": [ "demo1.example.org", "demo2.example.org" ],
      "services": [
      {
        "class": "tcpconnect",
        "args": { "port": 80 },
        "periods": [ "normal" ],
        "threshold": 10
      },
      {
        "class": "tcpconnect",
        "args": { "port": 25 },
        "periods": [ "normal" ],
        "threshold": 10
      }
      ]
    }
  },
  "layouts":{
  },
  "core":{
  }
}</pre>

<h3>Core configuration for Tasks</h3>

<p>Since we are making use of the mail task we need to set some configuration variables now. This will be the hostname of the SMTP server etc. For that we will add a task section containing a mail section directly in the root of the configuration:</p>

<pre class="prettyprint linenums">{
  "members": {
    "demo": {
      "name": "Demo Admin",
      "tasks": [
        { "class":"mail", "type": "mail", "args": { "rcpt": "demo@example.org" }}
      ]
    }
  },
  "tasks": {
    "mail":{
      "host": "localhost",
      "port": 25,
      "from": "linspector@example.org",
      "username": "",
      "password": ""
    }
  },
  "periods": {
    "normal": { "seconds": 120 }
  },
  "hostgroups": {
    "demogroup1": {
      "hosts": [ "demo1.example.org", "demo2.example.org" ],
      "services": [
      {
        "class": "tcpconnect",
        "args": { "port": 80 },
        "periods": [ "normal" ],
        "threshold": 10
      },
      {
        "class": "tcpconnect",
        "args": { "port": 25 },
        "periods": [ "normal" ],
        "threshold": 10
      }
      ]
    }
  },
  "layouts":{
  },
  "core":{
  }
}</pre>

<h3>Add member to Hostgroup</h3>

<p>For now we have add a hostgroup, added two hosts to that group which each will be monitored for availability of two ports. They will be polled every 2 minutes by a peroid called "normal" and the alert threshold is set to 10 for both services. No we need to add the member to the hostgroup so the member "demo" will accept alerts for the hostgroup "demogroup1":</p>

<pre class="prettyprint linenums">{
  "members": {
    "demo": {
      "name": "Demo Admin",
      "tasks": [
        { "class":"mail", "type": "mail", "args": { "rcpt": "demo@example.org" }}
      ]
    }
  },
  "tasks": {
    "mail":{
      "host": "localhost",
      "port": 25,
      "from": "linspector@example.org",
      "username": "",
      "password": ""
    }
  },
  "periods": {
    "normal": { "seconds": 120 }
  },
  "hostgroups": {
    "demogroup1": {
      "hosts": [ "demo1.example.org", "demo2.example.org" ],
      "members": ["demo"],
      "services": [
      {
        "class": "tcpconnect",
        "args": { "port": 80 },
        "periods": [ "normal" ],
        "threshold": 10
      },
      {
        "class": "tcpconnect",
        "args": { "port": 25 },
        "periods": [ "normal" ],
        "threshold": 10
      }
      ]
    }
  },
  "layouts":{
  },
  "core":{
  }
}</pre>

<p>We just added the string '"members": ["demo"],' (line 16) to the hostgroup. Thats it. If you want more members to be informed about alerts just add a new member to the members section and the add that meber to the list of members in the hostgroup.</p>

<h3>Layouts</h3>

<p>Now we can finalize the configuration but there a still one step for making monitoring work with this configuration. We need to enable the hostgroup for monitoring in the layouts section of the configuration. This makes it possible to add hostgroups for testing or to organize your hosts in groups and then just enable the hostgroups in the layout you really want to monitor or you can configure downtimes for some hostgroups by just adding more layouts with hostgroups disabled for monitoring.</p>

<pre class="prettyprint linenums">{
  "members": {
    "demo": {
      "name": "Demo Admin",
      "tasks": [
        { "class":"mail", "type": "mail", "args": { "rcpt": "demo@example.org" }}
      ]
    }
  },
  "tasks": {
    "mail":{
      "host": "localhost",
      "port": 25,
      "from": "linspector@example.org",
      "username": "",
      "password": ""
    }
  },
  "periods": {
    "normal": { "seconds": 120 }
  },
  "hostgroups": {
    "demogroup1": {
      "hosts": [ "demo1.example.org", "demo2.example.org" ],
      "members": ["demo"],
      "services": [
      {
        "class": "tcpconnect",
        "args": { "port": 80 },
        "periods": [ "normal" ],
        "threshold": 10
      },
      {
        "class": "tcpconnect",
        "args": { "port": 25 },
        "periods": [ "normal" ],
        "threshold": 10
      }
      ]
    }
  },
  "layouts":{
    "main":{
      "hostgroups": [ "demogroup1" ],
      "enabled": true
    }
  },
  "core":{
  }
}</pre>

<h3>Finished Configuration</h3>

<p>We are done! This is a working example of a very minimal Linspector configuration file for monitoring two services on two hosts. Save the above file in the root of your Linspector installation as demo.json for later usage.</p>

<h3>Start Linspector</h3>

<p>If everything was setup right you should be able to start Linspector for monitoring your hosts. Just run the following command in the root directory of your Linspector installation:</p>

<pre>bin/linspector ./demo.json</pre>

<p>That schould be all.</p>
