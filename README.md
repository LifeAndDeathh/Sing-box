# Sing-box
sing-box(SFI)IOS客户端使用笔记 - xray

SFI 客户端
```
[VLESS-XTLS-uTLS-REALITY]
{
  "log": {
    "disabled": false,
    "level": "info",
    "output": "sing-box.log",
    "timestamp": true
  },
  "dns": {
    "servers": [
      {
        "tag": "alidns",
        "address": "https://dns.alidns.com/dns-query",
        "address_resolver": "cf",
        "address_strategy": "prefer_ipv4",
        "strategy": "ipv4_only",
        "detour": "direct"
      },
      {
        "tag": "cf",
        "address": "https://1.1.1.1/dns-query",
        "strategy": "ipv4_only",
        "detour": "direct"
      },
      {
        "tag": "block",
        "address": "rcode://success"
      }
    ],
    "rules": [
      {
        "geosite": [
          "cn"
        ],
        "domain": [
          "myChinaDnsSite.com"
        ],
        "domain_suffix": [
          ".cn"
        ],
        "server": "alidns",
        "disable_cache": false
      },
      {
        "geosite": [
          "category-ads-all"
        ],
        "server": "block",
        "disable_cache": true
      }
    ],
    "final": "cf",
    "strategy": "",
    "disable_cache": false,
    "disable_expire": false
  },
  "inbounds": [
    {
      "tag": "tun-in",
      "type": "tun",
      "interface_name": "utun",
      "inet4_address": "172.19.0.1/30",
      "auto_route": true,
      "strict_route": true,
      "stack": "gvisor",
      "mtu": 9000,
      "sniff": true,
      "sniff_timeout": "300ms",
      "domain_strategy": "",
      "udp_timeout": 300
    }
  ],
  "outbounds": [
    {
      "tag": "proxy",
      "type": "vless",
      "server": "你的服务器IP",
      "server_port": 443,
      "uuid": "你服务端的用户UUID",
      "flow": "xtls-rprx-vision",
      "network": "tcp",
      "packet_encoding": "xudp",
      "tls": {
        "enabled": true,
        "server_name": "你的服务端设置的serverNames其一",
        "utls": {
          "enabled": true,
          "fingerprint": "chrome"
        },
        "reality": {
          "enabled": true,
          "public_key": "你服务端生成的公钥",
          "short_id": "你服务端shortIds其一，截止发文sing-box必须使用16位的short_id"
        }
      }
    },
    {
      "type": "direct",
      "tag": "direct"
    },
    {
      "type": "block",
      "tag": "block"
    },
    {
      "type": "dns",
      "tag": "dns-out"
    }
  ],
  "route": {
    "geoip": {
      "path": "geoip.db",
      "download_url": "https://github.com/SagerNet/sing-geoip/releases/latest/download/geoip.db",
      "download_detour": "direct"
    },
    "geosite": {
      "path": "geosite.db",
      "download_url": "https://github.com/SagerNet/sing-geosite/releases/latest/download/geosite.db",
      "download_detour": "direct"
    },
    "rules": [
      {
        "protocol": "dns",
        "outbound": "dns-out"
      },
      {
        "domain": [
          "myProxy.com"
        ],
        "outbound": "proxy"
      },
      {
        "domain": [
          "myDirect.com"
        ],
        "outbound": "direct"
      },
      {
        "geosite": [
          "cn",
          "private"
        ],
        "geoip": [
          "cn",
          "private"
        ],
        "domain_suffix": [
          ".cn"
        ],
        "outbound": "direct"
      },
      {
        "geosite": [
          "category-ads-all"
        ],
        "outbound": "block"
      }
    ],
    "auto_detect_interface": true,
    "final": "proxy"
  },
  "experimental": {}
}
```
[VLESS-H2-uTLS-REALITY]
```
{
    "log": {
        "disabled": false,
        "level": "info",
        "output": "sing-box.log",
        "timestamp": true
    },
    "dns": {
        "servers": [
            {
                "tag": "alidns",
                "address": "https://dns.alidns.com/dns-query",
                "address_resolver": "cf",
                "address_strategy": "prefer_ipv4",
                "strategy": "ipv4_only",
                "detour": "direct"
            },
            {
                "tag": "cf",
                "address": "https://1.1.1.1/dns-query",
                "strategy": "ipv4_only",
                "detour": "direct"
            },
            {
                "tag": "block",
                "address": "rcode://success"
            }
        ],
        "rules": [
            {
                "geosite": [
                    "cn"
                ],
                "domain": [
                    "myChinaDnsSite.com"
                ],
                "domain_suffix": [
                    ".cn"
                ],
                "server": "alidns",
                "disable_cache": false
            },
            {
                "geosite": [
                    "category-ads-all"
                ],
                "server": "block",
                "disable_cache": true
            }
        ],
        "final": "cf",
        "strategy": "",
        "disable_cache": false,
        "disable_expire": false
    },
    "inbounds": [
        {
            "tag": "tun-in",
            "type": "tun",
            "interface_name": "utun",
            "inet4_address": "172.19.0.1/30",
            "auto_route": true,
            "strict_route": true,
            "stack": "gvisor",
            "mtu": 9000,
            "sniff": true,
            "sniff_timeout": "300ms",
            "domain_strategy": "",
            "udp_timeout": 300
        }
    ],
    "outbounds": [
        {
            "tag": "proxy",
            "type": "vless",
            "server": "你的服务器IP",
            "server_port": 443,
            "uuid": "你服务端的用户UUID",
            "flow": "",
            "network": "tcp",
            "packet_encoding": "xudp",
            "transport": {
                "type": "http",
                "host": [
                    "h2"
                ]
            },
            "tls": {
                "enabled": true,
                "server_name": "你的服务端设置的serverNames其一",
                "utls": {
                    "enabled": true,
                    "fingerprint": "chrome"
                },
                "reality": {
                    "enabled": true,
                    "public_key": "你服务端生成的公钥",
                    "short_id": "你服务端shortIds其一，截止发文sing-box必须使用16位的short_id"
                }
            }
        },
        {
            "type": "direct",
            "tag": "direct"
        },
        {
            "type": "block",
            "tag": "block"
        },
        {
            "type": "dns",
            "tag": "dns-out"
        }
    ],
    "route": {
        "geoip": {
            "path": "geoip.db",
            "download_url": "https://github.com/SagerNet/sing-geoip/releases/latest/download/geoip.db",
            "download_detour": "direct"
        },
        "geosite": {
            "path": "geosite.db",
            "download_url": "https://github.com/SagerNet/sing-geosite/releases/latest/download/geosite.db",
            "download_detour": "direct"
        },
        "rules": [
            {
                "protocol": "dns",
                "outbound": "dns-out"
            },
            {
                "domain": [
                    "myProxy.com"
                ],
                "outbound": "proxy"
            },
            {
                "domain": [
                    "myDirect.com"
                ],
                "outbound": "direct"
            },
            {
                "geosite": [
                    "cn",
                    "private"
                ],
                "geoip": [
                    "cn",
                    "private"
                ],
                "domain_suffix": [
                    ".cn"
                ],
                "outbound": "direct"
            },
            {
                "geosite": [
                    "category-ads-all"
                ],
                "outbound": "block"
            }
        ],
        "auto_detect_interface": true,
        "final": "proxy"
    },
    "experimental": {}
}
```
服务端 streamSettings增加配置
```
{
    "streamSettings": {
        "httpSettings": {
            "host": [
                "h2"
            ]
        }
    }
}
```
[VLESS-XTLS-uTLS-TLS]
```
{
    "log": {
        "disabled": false,
        "level": "info",
        "output": "sing-box.log",
        "timestamp": true
    },
    "dns": {
        "servers": [
            {
                "tag": "alidns",
                "address": "https://dns.alidns.com/dns-query",
                "address_resolver": "cf",
                "address_strategy": "prefer_ipv4",
                "strategy": "ipv4_only",
                "detour": "direct"
            },
            {
                "tag": "cf",
                "address": "https://1.1.1.1/dns-query",
                "strategy": "ipv4_only",
                "detour": "direct"
            },
            {
                "tag": "block",
                "address": "rcode://success"
            }
        ],
        "rules": [
            {
                "geosite": [
                    "cn"
                ],
                "domain": [
                    "myChinaDnsSite.com"
                ],
                "domain_suffix": [
                    ".cn"
                ],
                "server": "alidns",
                "disable_cache": false
            },
            {
                "geosite": [
                    "category-ads-all"
                ],
                "server": "block",
                "disable_cache": true
            }
        ],
        "final": "cf",
        "strategy": "",
        "disable_cache": false,
        "disable_expire": false
    },
    "inbounds": [
        {
            "tag": "tun-in",
            "type": "tun",
            "interface_name": "utun",
            "inet4_address": "172.19.0.1/30",
            "auto_route": true,
            "strict_route": true,
            "stack": "gvisor",
            "mtu": 9000,
            "sniff": true,
            "sniff_timeout": "300ms",
            "domain_strategy": "",
            "udp_timeout": 300
        }
    ],
    "outbounds": [
        {
            "tag": "proxy",
            "type": "vless",
            "server": "你的域名",
            "server_port": 443,
            "uuid": "你的用户UUID",
            "flow": "xtls-rprx-vision",
            "network": "tcp",
            "packet_encoding": "xudp",
            "tls": {
                "enabled": true,
                "server_name": "你的域名",
                "utls": {
                    "enabled": true,
                    "fingerprint": "chrome"
                }
            }
        },
        {
            "type": "direct",
            "tag": "direct"
        },
        {
            "type": "block",
            "tag": "block"
        },
        {
            "type": "dns",
            "tag": "dns-out"
        }
    ],
    "route": {
        "geoip": {
            "path": "geoip.db",
            "download_url": "https://github.com/SagerNet/sing-geoip/releases/latest/download/geoip.db",
            "download_detour": "direct"
        },
        "geosite": {
            "path": "geosite.db",
            "download_url": "https://github.com/SagerNet/sing-geosite/releases/latest/download/geosite.db",
            "download_detour": "direct"
        },
        "rules": [
            {
                "protocol": "dns",
                "outbound": "dns-out"
            },
            {
                "domain": [
                    "myProxy.com"
                ],
                "outbound": "proxy"
            },
            {
                "domain": [
                    "myDirect.com"
                ],
                "outbound": "direct"
            },
            {
                "geosite": [
                    "cn",
                    "private"
                ],
                "geoip": [
                    "cn",
                    "private"
                ],
                "domain_suffix": [
                    ".cn"
                ],
                "outbound": "direct"
            },
            {
                "geosite": [
                    "category-ads-all"
                ],
                "outbound": "block"
            }
        ],
        "auto_detect_interface": true,
        "final": "proxy"
    },
    "experimental": {}
}
```
