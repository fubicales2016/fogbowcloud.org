Title: Fogbow Dashboard
url: fogbow-dashboard
save_as: fogbow-dashboard.html
section: usage
index: 1

Fogbow Dashboard
==========

##Installation

``` bash
git clone https://github.com/fogbow/fogbow-dashboard.git
cd fogbow-dashboard
./run_tests.sh (to download libraries)
```

##Configure
After the installation, open local_settings.py and edit its contents:

``` bash
FOGBOW_MANAGER_ENDPOINT = "http://localhost:5000"
```

##RUN
``` bash
./run_tests.sh --runserver localhost:9000
```
