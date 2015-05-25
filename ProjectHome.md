## Synopsis ##
SocksiPy - A Python SOCKS client module. It provides a socket-like interface that supports connections to any TCP service through the use of a SOCKS4, SOCKS5 or HTTP proxy.

The original version was developed by Dan Haim and can be downloaded from Sourceforge: https://sourceforge.net/projects/socksipy/

This is an unofficial branch created by Mario Vilas to address some open issues, as the original project seems to have been abandoned circa 2007.

## Using SocksiPy ##
### via wrapmodule() ###
Using SocksiPy is easy. In most cases, you can simply wrap standard modules with SocksiPy. To do this, simply:
  1. Import SocksiPy and the intended modules
  1. Set the proxy information by calling `setdefaultproxy()`
  1. Wrap the target module by calling `wrapmodule()`

```
# Import Target Modules
import ftplib
import telnetlib
import urllib2

# Import SocksiPy
import socks

# Set the proxy information
socks.setdefaultproxy(socks.PROXY_TYPE_SOCKS5, 'localhost', 9050)

# Route an FTP session through the SOCKS proxy 
socks.wrapmodule(ftplib)
ftp = ftplib.FTP('cdimage.ubuntu.com')
ftp.login('anonymous', 'support@aol.com')
print ftp.dir('cdimage')
ftp.close()

# Route a telnet connection through the SOCKS proxy
socks.wrapmodule(telnetlib)
tn = telnetlib.Telnet('achaea.com')
print tn.read_very_eager()
tn.close()

# Route an HTTP request through the SOCKS proxy 
socks.wrapmodule(urllib2)
print urllib2.urlopen('http://www.whatismyip.com/automation/n09230945.asp').read()
```

### via socksocket ###
It is just as easy to use SocksiPy in new code. You can use the `socksocket` in just the same ways you do a normal socket. Just:
  1. Import SocksiPy
  1. Instantiate a `socks.socksocket`. This takes the same arguments as a `socket.socket`.
  1. Set the proxy information by calling `setproxy()`
  1. Connect and use the socket like normal

```
import socks
s = socks.socksocket()
s.setproxy(socks.PROXY_TYPE_SOCKS5, 'localhost', 9050)
s.connect(('www.whatismyip.com', 80))
s.send('GET /automation/n09230945.asp HTTP/1.1\r\n\r\n')

data = ''
buf = s.recv(1024)
while len(buf):
    data += buf
    buf = s.recv(1024)
s.close()

print("Connected from %s." % (data))
```