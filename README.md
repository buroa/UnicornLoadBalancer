![](https://raw.githubusercontent.com/UnicornTranscoder/Logo/master/transparent.png)
## UnicornLoadBalancer

This software is a part of __UnicornTranscoder__ project, it's the LoadBalancer that will catch Plex requests and send them to a __UnicornTranscoder__.

## UnicornTranscoder Project

* [UnicornTranscoder](https://github.com/UnicornTranscoder/UnicornTranscoder)
* [UnicornLoadBalancer](https://github.com/UnicornTranscoder/UnicornLoadBalancer)
* [UnicornFFMPEG](https://github.com/UnicornTranscoder/UnicornFFMPEG)

## Support us!

The UnicornTranscoder project is an open source software, maintained by @drouarb and @Maxou44. If you want to support us, you can tip us on Ko-fi: https://ko-fi.com/unicorntranscoder ☕

## Dependencies

* Plex Media Server
* NodeJS
* RedisCache (Optionnal)
* Postgresql (Optionnal)

## Setup

### 1. Installation

* Clone the repository
* Install with `npm install`
* Setup some environnement variables to configure the *UnicornLoadBlancer*
* Type `npm start` to launch the load-balancer

| Name | Description | Type | Default |
| ----------------- | ------------------------------------------------------------ | ------| ------- |
| **SERVER_PORT** | Port used by the *UnicornLoadBalancer* | `int` | `3001` |
| **SERVER_PUBLIC** | Public url where the *UnicornLoadBalancer* can be called, **with** a slash at the end | `string` | `http://127.0.0.1:3001/` |
| **PLEX_HOST** | Host to access to Plex | `string` | `127.0.0.1` | 
| **PLEX_PORT** | Port used by Plex | `int` | `32400` | 
| **PLEX_PATH_USR** | The Plex's path | `string` | `/usr/lib/plexmediaserver/` | 
| **PLEX_PATH_SESSIONS** | The path where Plex store sessions (to grab external subtitles) | `string` | `/var/lib/plexmediaserver/Library/Application Support/Plex Media Server/Cache/Transcode/Sessions` | 
| **DATABASE_MODE** | Kind of database to use with Plex, can be `sqlite` or `postgresql` | `string` | `sqlite` |
| **DATABASE_SQLITE_PATH** | The path of the Plex database | `string` | `/var/lib/plexmediaserver/Library/Application Support/Plex Media Server/Plug-in Support/Databases/com.plexapp.plugins.library.db` |
| **DATABASE_POSTGRESQL_HOST** | Host of the Postgresql server | `string` | ` ` |
| **DATABASE_POSTGRESQL_DATABASE** | Name of the postgresql database | `string` | ` ` |
| **DATABASE_POSTGRESQL_USER** | User used by the Postgresql database| `string` | ` ` |
| **DATABASE_POSTGRESQL_PASSWORD** | Password used by the Postgresql database | `string` | ` ` |
| **DATABASE_POSTGRESQL_PORT** | Port used by the Postgresql database | `int` | `5432` |
| **REDIS_HOST** | The host of the redis database | `string` `undefined` | `undefined` | 
| **REDIS_PORT** | Port used by Redis | `int` | `6379` |
| **REDIS_PASSWORD** | The password of the redis database | `string` | ` ` | 
| **REDIS_DB** | The index of the redis database | `int` | `0` | 
| **CUSTOM_SCORES_TIMEOUT** | Seconds to consider a not-pinged server as unavailable | `int` | `10` | 
| **CUSTOM_IMAGE_PROXY** | URL of an alternative server to convert images, this proxy must be compliant with the *images.weserv.nl* API (original or self-hosted). Eg supported value: `https://images.weserv.nl/` | `string` | ` ` | 
| **CUSTOM_DOWNLOAD_FORWARD** | Enable or disable 302 for download links and direct play, if enabled, transcoders need to have access to media files | `bool` | `false` | 
| **CUSTOM_SERVERS_LIST** | Transcoder servers set by default, **with** a slash at the end, separate servers with a **comma** | `string array` | `[]` | 

* Configure Plex Media Server access address
 * In Settings -> Server -> Network
 * Set `Custom server access URLs` to the address to access the UnicornLoadBalancer
* Run with npm start

### 2. Notes

All requests to the Plex Media Server should pass through the *UnicornLoadBalancer*, if someone reach the server directly he will not be able to start a stream, since FFMPEG binary has been replaced. To solve this problem it is recomended to configure an iptable rule to drop direct access from port **32400**. You also need to disable the relay feature in Plex settings ***AND*** remove the **Plex Relay** binary.  
It is also recomended to setup a nginx reverse proxy in front of the *UnicornLoadBalancer* to setup a SSL certificate.

```
#Example iptable: Deny 32400 port
iptables -A INPUT -p tcp --dport 32400 -i [your-network-interface] -j DROP
```

After a 32400 port drop, you must define your server url and the port used (443 or 80) in Plex settings, if it's not configured you will not be able to access to your server from https://app.plex.tv

### Disclamer

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

__Pull Requests are welcome 😉__
