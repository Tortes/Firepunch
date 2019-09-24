## Choose your VPS services
### With credit card
- [AWS](https://aws.amazon.com) or [AWS China](https://amazonaws-china.com/cn/)
> Lightsail is recommanded to build as a shadowsocks server for low price($3.5 per month) and abundant public ip  
> We could follow the [guide](Lightsail.md) to set up Lightsail VPS.
- [Linode](linode.com)
> The Tokyo1 site is recommanded, which is sold out.
### Without credit card
- [Conoha](https://www.conoha.jp/)
> Conoha is a japan service provider, which supports Alipay and Paypal.
- [DigitalOcean](https://www.digitalocean.com)
> Paypal
- [Somagu](https://www.somagu.com)
> Korean service provider. Support Alipay and Paypal.

### Turn off the ping service
Edit `/etc/sysctl.conf`
- `vi /etc/sysctl.conf`
- add `net.ipv4.icmp_echo_ignore_all=1`
- `sysctl -p`