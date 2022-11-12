---
title: Installation Guide
---

<h2>Installation Guide</h2>

<ul>
  <li><i>Added: 2013.09.30 by Johannes Findeisen (hanez)</i></li>
  <li><i>Last update: 2013.11.03 by Johannes Findeisen (hanez)</i></li>
  <li><i>License: <a href="http://creativecommons.org/licenses/by-sa/3.0/" class="ext">CC Attribution-Share Alike 3.0 Unported</a></i></li>
</ul>

<h3>Introduction</h3>

<p>Here we show you how easy it is to install Linspector.</p>

<h3>Get Linspector</h3>

<p>Go to the Linspector <a href="/download/">download page</a> and download the latest package as zip or tar.gz file and extract it.</p>

<p>Then switch to the new linspector directory:</p>

<pre>cd linspector-VERSION</pre>

<h3>Requirements</h3>

<p>You need at least Python 2.7.* and the APScheduler library to run Linspector. We recommend to install Python using your package manager. Libraries are very easy to install using the Python package manager pip. Just install pip from your systems package manager and run the following command:</p>

<pre>pip install apscheduler</pre>

<p>All dependencies are being resolved and everything needed will be installed.</p>

<p>To install all libraries Linspector is using - even the optionally libraries - you can do this by running the following command from the root of the Linspector source tree:</p>

<pre>pip install -r requirements.txt</pre>

<p>"requirements" is a text file provided by Linspector. When doing it this way you can be sure installing working versions of the libraries.</p>

<h3>Run Linspector</h3>

<p>Running Linspector is very easy. </p>

<p><b>Since Linspector currently is a little bit broken you need to set the $PYTHONPATH to make sure imports of libs are working:</b></p>

<pre>export PYTHONPATH=`pwd`</pre>

<p>Then you only need to run the following command:</p>

<pre>./bin/linspector path/to/config.json</pre>

<p>Since we have not created a config file yet you need to configure Linspector to really get it up running. Read the <a href="/documentation/minimal-configuration-guide/">Minimal Configuration Guide</a> for that.</p>
