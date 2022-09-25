# GeoIPASN

GeoIP query tool with AS Number

GeoIP data from [geoip-lite](https://www.npmjs.com/package/geoip-lite) using [MaxMind's GeoLite2 database](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data)

ASN data from APNIC [(data-raw-table)](https://thyme.apnic.net/current/data-raw-table) [(data-used-autnums)](https://thyme.apnic.net/current/data-used-autnums)

Update GeoLite2 database to better results

```
cd node_modules/geoip-lite
npm run-script updatedb license_key=[KEY]
```

You can get free license key in [MaxMind](https://www.maxmind.com/en/accounts/current/license-key)

## Table of Content

- [Installation](#installation)
- [Usage](#usage)
  - [GeoIP Info](#geoip-info)
  - [AS Number](#as-number)
- [API](#api)
  - [geoip](#geoip)
  - [asn](#asn)

## Installation

```
npm i geoipasn
```

## Usage

### GeoIP Info

ESM

```js
import geoip from 'geoipasn';

const data = geoip('12.34.56.78');
console.log(data);
```

CJS

```js
const geoip = require('geoipasn').default;

const data = geoip('12.34.56.78');
console.log(data);
```

<details><summary>Result</summary>

```js
{
  ip: '12.34.56.78',
  country: { code: 'US', name: 'United States of America' },
  region: 'Ohio',
  city: 'Dayton',
  coordinate: { latitude: 39.6438, longtitude: -84.1743, range: 5 },
  timezone: 'America/New_York',
  time: 2022-09-24T02:47:59.000Z,
  as: { number: 7018, name: 'ATT-INTERNET4' },
  service: ''
}
```

</details>

### AS Number

```js
// Get AS number of IP
const data = geoip.asn('12.34.56.78');
console.log(data);
```

<details><summary>Result</summary>

```js
{ ip: '12.34.56.78', number: 7018, name: 'ATT-INTERNET4' }
```

</details>

### Command Line

```sh
cd node_modules/geoipasn
npm run query [ip]
```

Example

```sh
npm run query 12.34.56.78
```

<details><summary>Result</summary>

```
Running geoipasn at 2022-09-24T13:15:21.226Z

query   : 12.34.56.78
dns     : 8.8.8.8
ip      : 93.184.216.34

scan    : 1024 ports
open    : [80,443]
reset   : []
close   : [1119,1935]
filtered: 1020 ports

80      open    http
443     open    https
1119    close   bnetgame
1935    close   macromedia-fcs
```

</details>

## API

### geoip

```js
geoip(ip);
```

target: `string [host]:<port|ports>`<br>
<br>
target ports:<br>
`:80`: target port 80<br>
`:80,90,100`: target port 80, 90 and 100<br>
`:80-100`: target port in range 80 to 100<br>
`:22,80-100,443`: target port 22, port in range 80 to 100 and port 443<br>
`:@`: target most used 1024 ports<br>
`:*`: target all ports (same as `:1-65535`)<br>
<br>
options: `Object`<br>
&nbsp;&nbsp;options.protocol: `string <tcp|udp>`<br>
&nbsp;&nbsp;options.filterBogon: `boolean`<br>
&nbsp;&nbsp;options.dnsServer: `string <server>`<br>

### ping

```js
ping.tcp(target, options);
```

```js
ping.udp(host, port, options);
```

host: `string [host]`<br>
port: `number <port>`<br>
options: `Object`<br>
&nbsp;&nbsp;options.timeout: `number <miliseconds>`<br>
&nbsp;&nbsp;options.filterBogon: `boolean`<br>
&nbsp;&nbsp;options.dnsServer: `string <server>`<br>

### scan

```js
ping.scan(host, ports, options);
```

host: `string [host]`<br>
ports: `Array <ports>`<br>
options: `Object`<br>
&nbsp;&nbsp;options.timeout: `number <miliseconds>`<br>
&nbsp;&nbsp;options.chunk: `number <chunk>`<br>
&nbsp;&nbsp;options.protocol: `string <tcp|udp>`<br>
&nbsp;&nbsp;options.filterBogon: `boolean`<br>
&nbsp;&nbsp;options.dnsServer: `string <server>`<br>
