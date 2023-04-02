# Nmapsilent

Convert Nmap output for integration with other [Project Discovery](https://projectdiscovery.io/#/) tools.

> This is a simple script, don't get too excited, OK?

## About

I spent some time to see if I could replace [Nmap](https://nmap.org/) with [Naabu](https://github.com/projectdiscovery/naabu).
What I found is that I like the Naabu `-silent` operator, and how it allows me to chain together a pipeline of commands:

```
$ naabu -silent -host 1.1.1.1 | httpx -probe
```

However, most of the features I would use with Naabu seems to be a reimplementation of Nmap functionality, sometimes even directly integrating with Nmap (such as leveraging NSE scripts or other Nmap enumeration techniques using `-nmap-cli`).
Overall, I didn't find Naabu faster or more functional than Nmap for my use cases (notably, I'm not in the habit of scanning entire ASNs).

> This is not criticism for Naabu.
> The Project Discovery team is amazing, but Naabu isn't a good fit for me.

Instead of switching to Naabu for other Project Discovery tool integration, I wrote a script to convert the Nmap output to the style that Naabu uses in the pipeline.
Using this script allows me to get the advantages that Naabu's `-silent` operator offers, without having to use Naabu.

## Examples

### Chaining with httpx

Scan www.google.com, passing the results to [httpx](https://github.com/projectdiscovery/httpx) to verify HTTP services.

```
$ nmap www.google.com | nmapsilent | httpx

    __    __  __       _  __
   / /_  / /_/ /_____ | |/ /
  / __ \/ __/ __/ __ \|   /
 / / / / /_/ /_/ /_/ /   |
/_/ /_/\__/\__/ .___/_/|_|
             /_/

		projectdiscovery.io

[INF] Current httpx version v1.2.9 (latest)
https://www.google.com
http://www.google.com
```

### Chaining with TLS-Scan

Scan a `$RANDO` IP range in AWS, use [TLS-Scan](https://github.com/prbinu/tls-scan) to extract subject details from website certificates.

```
$ sudo nmap -p 443 100.24.166.0/27 | nmapsilent | tls-scan --cacert=/opt/local/ca-bundle.crt | jq '.certificateChain[0].subject'
"CN=*.us-east-1.es.amazonaws.com"
"CN=*.slycedata.io"
"CN=classicdiving.com"
"CN=lollicash.com"
"CN=*.exacteditions.com"
"CN=webmotors.com.br"
"CN=*.us-east-1.es.amazonaws.com"
"CN=adobess.com; O=Adobe Inc.; L=San Jose; ST=California; C=US"
 pid: 64597 | ciphers: (0) |host-count: 8 |network-error: 0 |dns-errcount: 0 |remote-close-error: 0 |unknown-error: 0 |connect-error: 0 |timeout-error: 0 |tls-handshake: 8 |gross-tls-handshake: 8 |elapsed-time: 3.811843 secs
```

## Comments/Questions/Concerns?

If you like this project, go ahead and smash that Star button.

Otherwise, open a ticket. PR's gratefully received.
