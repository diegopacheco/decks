# How Troubleshoo NodeJS

Good Resources

https://nodesource.com/blog/diagnostics-in-NodeJS-1
https://nodesource.com/blog/diagnostics-in-NodeJS-2
https://nodesource.com/blog/diagnostics-in-NodeJS-3/

Clinic JS
https://clinicjs.org/

Good article on profiling
https://medium.com/voodoo-engineering/node-js-and-cpu-profiling-on-production-in-real-time-without-downtime-d6e62af173e2

FlameGraphs
https://nodejs.org/en/docs/guides/diagnostics-flamegraph/

Why Node is Running
https://stackoverflow.com/questions/5916066/node-js-tool-to-see-why-process-is-still-running

Heap Dump with code
1. install npm instal heapdump --save
2. Add the head-dump endpoint in express
```
app.get('/heap-dump', (req, res) => {
    var heapdump = require('heapdump');
    var filename = '/' + Date.now() + '.heapsnapshot';
    heapdump.writeSnapshot(filename);
    heapdump.writeSnapshot(function (err, filename) {
        console.log('dump written to', filename);
    });
    res.send(filename + " written!");
});
```
3. load in Chrome with - (A) run the app with node --inspect src/app.js
4. Goto chrome and type: about:inspect
5. Load the headump file in the Chrome V8 Inspector tool
More on: https://nodejs.org/en/docs/guides/diagnostics/memory/using-heap-snapshot

Heapdump on CLI
1. run app with node --heapsnapshot-signal=SIGUSR2 src/app.js
2. get the app pid ps aux | grep app.js
3. take the heapdump kill -USR2 $PID
4. will get a file like: Heap.20231117.210811.245053.0.001.heapsnapshot

