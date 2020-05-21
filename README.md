KALITORRIFIC TRAINWRECK. Keeping this code base around for some ideas, but building a solid version of this instead. 

DO NOT CLONE AND USE THIS - IT'S BUGGY AND WILL ALMOST CERTAINLY BREAK YOUR MACHINE AFTER A FEW USES. YOU'VE BEEN WARNED. 


Original Author Notes:

## Install

### Install dependencies:
```bash
sudo apt update && sudo apt full-upgrade -y

sudo apt install tor -y
```

### Install kalitorify and reboot:
```bash
git clone https://github.com/brainfucksec/kalitorify

cd kalitorify/

sudo make install

sudo reboot
```

---

## Security

**kalitorify is produced independently from the Tor anonimity software and carries no guarantee from the Tor Project about quality, suitability or anything else,** please read these documents to know how to use the Tor network safely:

[Tor General FAQ](https://www.torproject.org/docs/faq.html.en)

[Whonix Do Not recommendations](https://www.whonix.org/wiki/DoNot)

**kalitorify provides transparent proxy management on Tor but does not provide 100% anonymity.**

From [Arch Linux Wiki](https://wiki.archlinux.org/index.php/Tor) about Transparent Torification: Using iptables to transparently torify a system affords comparatively strong leak protection, but it is not a substitute for virtualized torification applications such as Whonix, or TorVM.
Applications can still learn your computer's hostname, MAC address, serial number, timezone, etc. and those with root privileges can disable the firewall entirely. In other words, transparent torification with iptables protects against accidental connections and DNS leaks by misconfigured software, it is not sufficient to protect against malware or software with serious security vulnerabilities.

For this, you should change at least the hostname and the MAC address:

[Setting the Hostname on Debian](https://debian-handbook.info/browse/stable/sect.hostname-name-service.html)

[Changing MAC Address on Linux](https://en.wikibooks.org/wiki/Changing_Your_MAC_Address/Linux)

### Checking for leaks:

After starting kalitorify you can use [tcpdump](https://www.tcpdump.org/) to check if there are any internet activity other the Tor:

First, get your network interface:
```bash
ip -o addr
```

or

```bash
tcpdump -D
```

We'll assume its `eth0`.

Next you need to identify the Tor guard IP, you can use `ss`, `netstat` or `GETINFO entry-guards` through the tor controller to identify the guard IP.

Example with `ss`:
```bash
ss -ntp | grep "$(cat /var/run/tor/tor.pid)"
```

With the interface and guard IP at hand, we can now use `tcpdump` to check for possible non-tor leaks. Replace IP.TO.TOR.GUARD with the IP you got from the `ss` output.
```bash
tcpdump -n -f -p -i eth0 not arp and not host IP.TO.TOR.GUARD
```

You are not supposed to see any output other than the first two header lines. You can remove `and not host IP` to see how it would look like otherwise.

Source: https://trac.torproject.org/projects/tor/wiki/doc/TransparentProxy#Checkingforleaks

---

## Usage

Please, before starting kalitorify make sure you have read the section about [Security](https://github.com/brainfucksec/kalitorify#security).
The use of program is very simple, the syntax follow the order `<program name> <option>`, you can show the help menu at any time with the `--help` option:

**kalitorify [option]**

### Options

**-t, --tor**

    start transparent proxy through tor

**-c, --clearnet**

    reset iptables and return to clearnet navigation

**-s, --status**

    check status of program and services

**-i, --ipinfo**

    show public IP

**-r, --restart**

    restart tor service and change IP

---

