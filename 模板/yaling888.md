# 来源
- https://github.com/Dreamacro/clash/pull/2518#issue-1566875446

```yaml
proxy-groups:
  - name: All
    type: select
    use: &Use
      - Subscription-1
      - Subscription-2

  - name: Geo-HK
    type: url-test
    interval: 600
    tolerance: 100
    url: http://www.gstatic.com/generate_204
    filter: "HongKong|HK|香港"
    use: *Use

  - name: Geo-TW
    type: select
    filter: "Taiwan|TW|台湾"
    use: *Use

  - name: Geo-SG
    type: fallback
    interval: 600
    tolerance: 100
    url: http://www.gstatic.com/generate_204
    filter: "Singapore|SG|新加坡"
    use: *Use

  - name: Geo-JP
    type: load-balance
    interval: 600
    tolerance: 100
    url: http://www.gstatic.com/generate_204
    filter: "Japan|JP|日本"
    use: *Use

  - name: Geo-UK
    type: relay
    filter: "Britain|England|UK|英国"
    use: *Use

  - name: Proxy
    type: select
    use: *Use
    proxies: &Proxy
      - All
      - Geo-HK
      - Geo-TW
      - Geo-SG
      - Geo-JP
      - Geo-UK
      - DIRECT
      - REJECT

  - name: YouTube
    type: select
    use: *Use
    proxies: *Proxy

proxy-providers:
  Subscription-1:
    type: http
    url: "URL1"
    path: ./providers/Subscription-1.yaml
    interval: 10800
    health-check:
      enable: false
      url: http://www.gstatic.com/generate_204

  Subscription-2:
    type: file
    path: ./providers/Subscription-2.yaml
    health-check:
      enable: false
      url: http://www.gstatic.com/generate_204
```
