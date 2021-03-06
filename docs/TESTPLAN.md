# Test plan for this add-on
Refer to the full test-plan that can be found [here](https://docs.google.com/document/d/1ArgeAlG-pF4erstCaldwXl7_gq9p1kVY5JQvI789TE0/edit).

## Manual / QA TEST Instructions
Start by downloading Firefox 59 (Release).

- Navigate to `about:config` and set the following preferences. (If a preference does not exist, create it be right-clicking in the white area and selecting New -> String or Integer depending on the type of preference)
- Set `extensions.legacy.enabled` to `true`. This permits the loading of the embedded Web Extension since new versions of Firefox are becoming restricted to pure Web Extensions only.
- Set `shield.test.variation` to `test` or `control`.
- Set any useful preference from the section below, before the add-on is installed.
- [Download](TBD) and install the latest signed XPI

Please note that, every time the study is uninstalled, the test preferences get removed.

## The local server
Overriding the `extensions.icqstudyv1.endpoint` allows testing the add-on on the local network.

- Generate a 4MB file in the directory were the server is going to be executed. This can be done in a Linux shell (or `bash` on Windows) using the following command: `head -c 4M </dev/urandom >test_file.ext`.
- The [cors_server.py](../test/cors_server.py) can be executed with `python cors_server.py --latency 200 --port 3785` for spawning a CORS-enabled server that servers the requests with a 200ms latency.
- The `extensions.icqstudyv1.endpoint` pref can be set to `http://127.0.0.1:3785/test_file.ext`.

A rate limiting software can be used to throttle the bandwidth allocated for the process running `cors_server.py`, simulating varying networking conditions.

## Useful preferences
The following is a list of preferences that can be overridden to simplify testing.

- `extensions.icqstudyv1.initDelayMs` (*default: 6000*, or 60 seconds), how long (in ms) until we can start measurements after the browser started up.
- `extensions.icqstudyv1.startDateMs`, a string that stores the date the study was started on (e.g. from `Date.now()`).
- `extensions.icqstudyv1.idleWindowS` (*default: 300*, or 5 minutes), how long (in seconds) user must be idle before we can consider measuring the speed of the connection.
- `extensions.icqstudyv1.endpoint`, the URI to use for evaluating the connection quality.
- `extensions.icqstudyv1.lastMeasurementMs`, the last time (in ms) a measurement was completed (e.g. from `Date.now()`).
- `extensions.icqstudyv1.delayBetweenMeasurementsMs` (*default: 25200000*, or 7 hours), the time (in ms) between two consecutive measurements.
- `extensions.icqstudyv1.numPerformedMeasurements`, the number of measurements performed throughout the lifetime of the study.

Additionally, the following SHIELD specific preferences can be changed.

- `shield.testing.logging.level`, controls the log level for the SHIELD study; setting this to the integer number *10* enables trace level output in the `Browser Console`; this is particularly useful to shed light on odd behaviors in the code.
- `shield.test.variation`, forces the variation of the study; if not set, the variation will be automatically assigned when the study starts.

