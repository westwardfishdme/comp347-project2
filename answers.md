# Finch's answer key
**IP in scope:** 172.16.27.17

## Format answered:

0. Question will be numbered like so.
> answers will be provided as quotations.
```
or as code blocks as seen here.
```
Then an explanation of how I found the answer will be given below.

# Actual answers
1. What is the MAC address of the device being watched?:
> 7c:f1:7e:3b:39:19

Found in packet 3, just copied the SRC MAC address.

2. What is the MAC address of the gateway device?
> ba:db:ee:ff:f9:48

Found in the second `ARP` packet; (packet 18)

Filtered on `ARP`.

3. What was the IP of the first device that was pinged?
> 172.16.27.129

Found in the first `ICMP` packet (packet 20).

Filtered on `ICMP`

4. What DNS servers is being used to resolve names to IPs?
> 172.16.27.1

Found in the first `DNS` packet (packet 67).

Filtered on `DNS`.

5. According to DNS in the capture, what IPv4 address hosts the duckduckgo.com website?
> 40.89.244.232

Found as the STD query response inside of packet 72; type A address from the DNS server.

Filtered on `DNS`.

6. What account was used to log into the router device via HTTP?
> sstevenson:pineapple2

Found in packet 4572 after filtering on `HTTP`.

7. What account was used to log into the local mail server via TELNET?
> root:mango1

Found in packets 4961, 4963, 4966, 4969 and 4983. User input is sent in single packets
because any remote shell session sends a livestream of packets from the client to the
server.

Filtered on `telnet` and followed the `TCP` stream.

8. At the time of the capture, how long had the local mail server been
   up (uptime found in TELNET)?
```
16:04:17 up  1:32,  3 users,  load average: 0.00, 0.00, 0.00`
```
Found in packet 5021.

9. Who did the target send e-mail as and to during the interaction via
   SMTP and what was the content of the message? (provide the from and
   to email addresses)
```
MAIL FROM:<steve.m.stevenson@aol.com>
RCPT TO:<help@security-firm.co.uk>

I would like to get a quote on your identity protection package.
-steve
```

Filtered on `SMTP` and found in packets 4869, 4883, and 4898.

10. What version of the Linux kernel is the local mail server running,
    which can be found via TELNET interactions? (it is of the format
    "Linux 6.x.y-z-amd64")
> Linux 6.1.0-40-amd64

Found as a part of the MOTD on login; (packet 4992)

11. According to the HTTP2 GET requests, embedded in the HEADERS, what
    was the first, complete search request sent by the user, via
    www.duckduckgo.com (they require the use of the ssl keys provided)?

```
2906 -> https://duckduckgo.com/ac/?q=what+is+quantum+cryptography&kl=wt-wt
2913 -> https://duckduckgo.com/?origin=funnel_home_website&t=h_&q=what+is+quantum+cryptography
```
Found in packets 2906, 2913 on `HTTP2`

Filtered on `http2.response_in`

12. What file was downloaded from the FTP server using the RETR command
    by the device being watched?
> [2024-sstevenson-1040.pdf](./important/2024-sstevenson-1040.pdf)

Found on filtering for `FTP` on packet 5743; Item was exported and included as a part of the report.

13. What information was provided on the via the HTTP session to the
    local web server?
> other than the credentials for login, there was a few [images](./images), there was a credential dump
stored on the web server in the path [/vault.html](./important/vault.html) of all login details for sstevenson
across several email accounts, and several shell logins for numerous servers. 

This information was found on filtering for HTTP (to the server IP on the local network) and in packet 5619.

14. What did the user change on the local mail server via TELNET using
    the pico command (you should provide the details on what was added,
    and to which file it was added?
> The file modified was `/etc/postfix/sasl_passwd`

```sh
pico /etc/postfix/sasl_passwd
```
The information entered was: 
```structured-text
[smtp.aol.com]:587 steve.m.stevenson@aol.com:"Il1k3tr@ff1cl1ght$"
[127.0.0.1]:587 steve.stevenson@proton.me:"Il1k3tr@ff1cl1ght$"
[smtp.gmail.com]:587 steve.m.stevenson@gmail.com:"Il1k3tr@ff1cl1ght$" # this line here was appended.
```
Found in packets 5518, and 5527; Filtered on `TELNET` and followed the TCP stream.

15. What vendor made the local router and what model is it, which can be
    found via HTTP interactions (exporting HTTP objects and reviewing
    the ones from the local router, in particular the status items, will
    be helpful)?
> Linksys E2530 v3

Found in the 10KiB exported object `status-data.jsx?_http_id=TIDd60f245957fb603a`
on line 36. Labelled here as [status-data.jsx](./important/router_pages/status-data.jsx)
```jsx
nvram = {
  // ...
  't_model_name':'Linksys E2530 v3',
}
```
This was sent on packet 4714.

