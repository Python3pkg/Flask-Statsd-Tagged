# Flask-Statsd-Tagged

This module is meant for use with the statsd plugin in Telegraf, which then report data to InfluxDB.

# Install
```bash
pip install flask-statsd-tagged
```

FlaskStatsdTagged(app)
```

# Send data
```python
@route('/app/download')
def download():
    pass

@route('/app/<device>/stats')
def device_download(device):
    pass
```

* Example data submitted via statsd protocol for a request to `/app/download`

    ```
    flaskrequest,status_code=200,server=vagrant-ubuntu-trusty-64,path=/app/download:1|c
    flaskrequest,status_code=200,server=vagrant-ubuntu-trusty-64:0.467062|ms
    ```

* Example data submitted via statsd protocol for a request to `/app/android/stats`

    ```
    flaskrequest,status_code=200,server=vagrant-ubuntu-trusty-64:1,path=/app/android/stats|c
    flaskrequest,status_code=200,server=vagrant-ubuntu-trusty-64:0.467062,path=/app/android/stats|ms
    ```


# Using it in your Flask application
```python
from flask import Flask
from flask.ext.statsd_tagged import FlaskStatsdTagged

app = Flask(__name__)
FlaskStatsdTagged(app=app, extra_tags={'bazinga':'sheldon')
```

Any tags added to the dictionary g.statsd_tags will also be added as tags to the metrics. E.g

```python
from flask import g
from flask.ext.statsd_tagged import incr, timing, gauge, tag

@route('/download')
def download():
    # Note that the new interface still uses flask.g object underneath
    
    # settings tags with legacy interface:
    g.statsd_tags = {'filename':'somename.tgz', 'worker': 'Herkules'}  
    # or better (equivalently) in new way:
    tag('filename', 'somename.tgz')
    tag('worker', 'Herkules')
    
    timing('compression_time', 500)
    incr('disk_reads', 1)
    gauge('current_temperature', 15)
```

# Thankyou

This is a fork of https://github.com/gfreezy/Flask-Statsd. Thankyou! :-)



