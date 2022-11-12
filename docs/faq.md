---
title: FAQ
---

# FAQ

## General

### Is it posible to daemonize Linspector?

<p>No. Linspector is an interactive software which provides a terminal based frontend called Lish for maintenance tasks. Daemonizing makes no sense. Maybe daemonizing will be added in the future. Just start Linspector in a <a href="https://www.gnu.org/software/screen/" class="ext">screen</a> or <a href="http://tmux.sourceforge.net/" class="ext">tmux</a> session and detach. This is how we are doing it.</p>

### Will Linspector provide a RPC interface for management?

<p>Yes. Linspector will provide a <a href="http://en.wikipedia.org/wiki/JSON-RPC" class="ext">JSON-RPC</a> based interface.</p>

### Is a web interface planned in Linspector?

<p>No. Linspector will not provide a web interface for management. BTW, there will be a seperate project called <a href="/projects/weblin/">WebLin</a> which provides a web interface for Linspector. WebLin will make use of the JSON-RPC interface provided by Linspector.</p>

## Errors</h3>

### I got "error: can't start new thread" in Lish. What happened?

<p>The Error looks like this:</p>

<pre>Exception in thread APScheduler:
Traceback (most recent call last):
  File "/usr/lib64/python2.7/threading.py", line 808, in __bootstrap_inner
    self.run()
  File "/usr/lib64/python2.7/threading.py", line 761, in run
    self.__target(*self.__args, **self.__kwargs)
  File "/usr/lib64/python2.7/site-packages/apscheduler/scheduler.py", line 581, in _main_loop
    next_wakeup_time = self._process_jobs(now)
  File "/usr/lib64/python2.7/site-packages/apscheduler/scheduler.py", line 547, in _process_jobs
    self._threadpool.submit(self._run_job, job, run_times)
  File "/usr/lib64/python2.7/site-packages/apscheduler/threadpool.py", line 105, in submit
    self._adjust_threadcount()
  File "/usr/lib64/python2.7/site-packages/apscheduler/threadpool.py", line 58, in _adjust_threadcount
    self._add_thread(self.num_threads &lt; self.core_threads)
  File "/usr/lib64/python2.7/site-packages/apscheduler/threadpool.py", line 65, in _add_thread
    t.start()
  File "/usr/lib64/python2.7/threading.py", line 743, in start
    _start_new_thread(self.__bootstrap, ())
error: can't start new thread</pre>

<p>If you are getting this error message and all scheduled jobs are shutting down, you have reached the maximum allowed number of processes running for your user. Please refer to the documentation of your OS for setting this value higher.</p>
