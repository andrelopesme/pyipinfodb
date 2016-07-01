# pyipinfodb.py

This is a simple python wrapper around [IPInfoDB](http://ipinfodb.com/)'s
free IP address geolocation [API](http://ipinfodb.com/ip_location_api.php).   
In order to use it, you need to get an [api key](http://ipinfodb.com/register.php).

*Note: This repo was originally forked from 
(https://github.com/mossberg/pyipinfodb). I've added a timeout capability so
the API does not get in your way and block your code.

## getting started

This wrapper is super easy to use.
If you just want to try it out: download this repo as a zip, unzip, and change
to that directory in your terminal. Then in a python interpreter: import it,
instantiate an IPInfo object using your api key, and you're ready to go!
In this example, replace `<apikey>` with your api key.

```bash
$ curl -L -O https://github.com/andrelopesme/pyipinfodb/archive/master.zip
$ unzip master.zip
$ cd pyipinfodb-master
$ python
>>> import pyipinfodb
>>> ip_lookup = pyipinfodb.IPInfo('<apikey>')
>>> ip_lookup.get_country('8.8.8.8')
{u'countryName': u'UNITED STATES', u'ipAddress': u'8.8.8.8', u'countryCode': u'US', u'statusMessage': u'', u'statusCode': u'OK'}
```

## installation

If you want to use this in a real project, you need to install the package.

```bash
$ pip install git+git://github.com/andrelopesme/pyipinfodb.git
```

Or you can manually run the `setup.py` file in the zip file.

```bash
$ python setup.py install
```

## documentation

#### `get_country(self, ip=None, timeout=None)`

Return a dictionary with country information based on ip address passed as
parameter.  
Example output:

    {u'countryName': u'UNITED STATES', u'ipAddress': u'74.125.45.100', u'countryCode': u'US', u'statusMessage': u'', u'statusCode': u'OK'}

If no parameters are passed, returns information about the client ip making the request.

#### `get_city(self, ip=None, timeout=None)`

Return a dictionary with detailed country, city, and timezone information
based on ip address passed as parameter.  
Example output:

    {u'countryCode': u'US', u'cityName': u'MOUNTAIN VIEW', u'zipCode': u'94043', u'longitude': u'-122.079', u'countryName': u'UNITED STATES', u'latitude': u'37.406', u'timeZone': u'-07:00', u'statusCode': u'OK', u'ipAddress': u'74.125.45.100', u'statusMessage': u'', u'regionName': u'CALIFORNIA'}

If no parameters are passed, returns information about the client ip making
the request.

Avoid using this function if you don't need this level of detail, it helps
keep the load lower on IPInfoDB's servers :)

#### `get_ip_info(self, baseurl, ip=None, timeout=None)`

This is the backend that powers the above functions. It lets you specify a
base url to use for your requests. You probably won't need to use this
function.

## extra example with 2 seconds timeout

    import pyipinfodb
    
    ...
    
    try:
		ip_lookup = pyipinfodb.IPInfo('your-key-here')
		geo_data = ip_lookup.get_city(get_client_ip(request), timeout=2)
		lat = geo_data['latitude']
		lon = geo_data['longitude']
		country_code = ip_lookup.get_country(get_client_ip(request), timeout=2)['countryCode']
	except:
		lat = u'38.87392854'
		lon = u'-8.20678711'
	  	country_code = ""
    ...
    
## contributing

Feel free to contribute! Just fork this repo, clone the fork to your local
machine and install dependencies using:

```bash
$ pip install -r requirements.txt
```

Then make a branch for whatever you're working on and when you're ready, submit
a pull request. I recommend using [virtualenv](http://www.virtualenv.org/)
to create an isolated python environment for development.

### tests

Edit the `secrets.py.example` file and replace `<apikey>` with your key. Then
rename the file to `secrets.py`. Now you can simply run tests with:

```bash
$ nosetests
```
