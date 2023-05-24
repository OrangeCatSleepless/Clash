# 来源

- https://github.com/Toperlock/Clash/blob/main/ClashforWindows.yaml

```
# @ConfigName                自用Clash配置文件 https://github.com/Toperlock/Clash/blob/main/ClashforWindows.yaml
# @Author                    @ddgksf2013, @iKeLee, @Repcz
# @Function                  支持手动选择、自动测速、广告屏蔽、兜底分流、按国家分组节点等等
# @ClashDownload             https://github.com/Fndroid/clash_for_windows_pkg/releases
# @Clash_Chinese_Patch       https://github.com/BoyceLig/Clash_Chinese_Patch/releases
# @Clash.Meta Core           https://github.com/MetaCubeX/Clash.Meta/releases
# @ddgksf2013 GitHub link    https://github.com/ddgksf2013/Profile/blob/master/ClashforWindows.yaml
# @iKeLee Gitlab link        https://gitlab.com/lodepuly/vpn_tool/-/tree/main/Tool/Clash/Config
# @Repcz GitHub link         https://github.com/Repcz/Open-Proflies/tree/main/Clash
# @Thanks                    @blackmatrix7, @Fndroid, @BoyceLig, @Anti
# @Attention                 𝐏𝐥𝐞𝐚𝐬𝐞 𝐮𝐬𝐞 𝐭𝐡𝐞 𝐥𝐚𝐭𝐞𝐬𝐭 𝐯𝐞𝐫𝐬𝐢𝐨𝐧 𝐨𝐟 𝐂𝐥𝐚𝐬𝐡𝐟𝐨𝐫𝐖𝐢𝐧𝐝𝐨𝐰𝐬
# @ConfigVersion             1.0.8
# @Note                      开启TUN Mode需要下载Service Mode，不能分流尝试修改TUN Mode里TUN Stack模式。下载Meta内核替换clash内核

port: 7890                   # HTTP 代理端口
socks-port: 7891             # SOCKS5 代理端口
#redir-port: 7892            # Linux 和 macOS 的 redir 代理端口
mixed-port: 7893             # 混合端口 HTTP和SOCKS5用一个端口
allow-lan: true              # 允许局域网的连接（可用来共享代理）
bind-address: "*"            # 仅在将allow-lan设置为true时适用
                             # #"*": 绑定所有IP地址
ipv6: false                  # 开启 IPv6 总开关，关闭阻断所有 IPv6 链接和屏蔽 DNS 请求 AAAA 记录
mode: rule                   # 规则模式：rule（规则） / global（全局代理）/ direct（全局直连）/ script (脚本)
log-level: info              # 设置日志输出级别 (5 个级别：silent / error / warning / info / debug）
external-controller: 127.0.0.1:9090      #外部控制器,可以使用 RESTful API 来控制你的 clash 内核

dns:
  enable: true               # 关闭将使用系统 DNS
  ipv6: false                # IPV6解析开关；如果为false，将返回ipv6结果为空
  enhanced-mode: fake-ip     # 模式：redir-host或fake-ip
  listen: 0.0.0.0:53         # DNS监听地址
  default-nameserver:        # 解析非IP的dns用的dns服务器,只支持纯IP
    - 223.5.5.5
    - 119.29.29.29
  fake-ip-range: 198.18.0.1/16           # Fake-IP解析地址池
  fake-ip-filter:            # fake ip 白名单列表,以下地址不会下发fakeip映射用于连接
    - '*.lan'
    - localhost.ptlogin2.qq.com
    - '+.srv.nintendo.net'
    - '+.stun.playstation.net'
    - '+.msftconnecttest.com'
    - '+.msftncsi.com'
    - '+.xboxlive.com'
    - 'msftconnecttest.com'
    - 'xbox.*.microsoft.com'
    - '*.battlenet.com.cn'
    - '*.battlenet.com'
    - '*.blzstatic.cn'
    - '*.battle.net'
    - '*.mcdn.bilivideo.cn'
  nameserver:                # 默认DNS服务器，支持udp/tcp/dot/doh/doq
    - 119.29.29.29
# 腾讯阿里DoH2
    - https://1.12.12.12/dns-query
    - https://223.5.5.5/dns-query
  fallback:                  # 解析国外域名的DNS服务器
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
  fallback-filter:           # 配置 fallback 使用条件
    geoip: true              # 配置是否使用 geoip
    geoip-code: CN           # 当 nameserver 域名的 IP 查询 geoip 库为 CN 时，不使用 fallback 中的 DNS 查询结果
    ipcidr:                  # 在这个网段内的 IP 地址会被考虑为被污染的 IP
      - 240.0.0.0/4
    domain:                  # 这些域名被视为已污染,匹配到这些域名,会直接使用fallback解析,不去使用nameserver
      - '+.google.com'
      - '+.facebook.com'
      - '+.youtube.com'


# interval 代表间隔时间，单位为 s；
# tolerance 代表容差时间，单位为 ms；
# 由于分流规则引用了 GitHub 文件，如果你的本地网络无法访问 GitHub，你将无法让 Clash 正常工作；
# 你需要先使用翻墙工具让本地网络出墙，再启动 Clash 才能正常下载分流规则并正常启动，待 Clash 启动成功并更新完订阅资源和分流规则之后，方可关闭你之前打开的翻墙工具；
# 请使用节点订阅转换服务将服务商的订阅链接转换为 Clash 的订阅，并输出为 Node List 格式；
# 请使用节点订阅转换工具提供的保留节点和过滤节点功能，生成不同地区的订阅链接，并填入下方 proxy-providers 区域；
# 其他内容具体见注释；
# 节点订阅转换网址 https://acl4ssr-sub.github.io/
# URL在线编码工具 https://www.urlencoder.org

# 节点订阅
# 默认每6小时更新一次节点订阅，每 30分钟 检测一次健康度。


###建立锚点

# 策略组引用相关
pr:
  &pr
  type: select
  proxies:
    - ♻️ 自动选择
    - 🚀 手动切换
    - 🇭🇰 香港节点
    - 🇨🇳 台湾节点
    - 🇺🇲 美国节点
    - 🇯🇵 日本节点
    - 🇸🇬 狮城节点
    - 🇰🇷 韩国节点
    - ✈️ 其他节点
    - DIRECT

# 订阅更新和延迟测试相关
p:
  &p
  type: http
  interval: 86400
  health-check:
    enable: true
    url: http://www.gstatic.com/generate_204
    interval: 1800

# 自动选择相关
auto:
  &auto
  type: url-test
  lazy: true
  url: http://www.gstatic.com/generate_204
  interval: 900
  use:
    - Subscribe

# 手动选择相关
use:
  &use
  type: select
  use:
    - Subscribe

# classical规则相关
c:
  &c
  type: http
  behavior: classical
  interval: 86400

# domain规则相关
d:
  &d
  type: http
  behavior: domain
  interval: 86400

# ipcidr规则相关
i:
  &i
  type: http
  behavior: ipcidr
  interval: 86400

###结束

# 请把中文替换你的编码后的订阅链接，多链接用|分割 例如：链接1|链接2|链接3...
proxy-providers:
  Subscribe:
    <<: *p
    url: url: https://sub.xeton.dev/sub?target=clash&new_name=true&url=中文&list=true&udp=false
    path: ./proxy_providers/tmp.yaml
    interval: 21600
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 1800

proxies: null

proxy-groups:
# 分流分组
  - {name: 🚀 手动切换, <<: *use}
  - {name: ♻️ 自动选择, <<: *auto}
  - {name: 🆎 AdBlock, type: select, proxies: [REJECT, DIRECT]}
  - {name: 🎬 国际媒体, <<: *pr}
  - {name: 🛍️ 亚马逊, <<: *pr}
  - {name: 🍎 苹果服务, <<: *pr}
  - {name: 🌌 谷歌服务, <<: *pr}
  - {name: 📟 电报消息, <<: *pr}
  - {name: 🐦 推特消息, <<: *pr}
  - {name: 🤖 OpenAI, type: select, proxies: [🇺🇲 美国节点, 🇸🇬 狮城节点]}
  - {name: 🤖 New Bing, <<: *pr}
  - {name: 📺 哔哩哔哩, type: select, proxies: [DIRECT, 🇭🇰 香港节点, 🇨🇳 台湾节点]}
  - {name: 🎮 游戏平台, <<: *pr}
  - {name: 🌏 全球加速, <<: *pr}
  - {name: 🐟 兜底分流, <<: *pr}
# 节点地区分组
  - {name: 🇭🇰 香港节点, <<: *use, filter: "港|HK|(?i)Hong"}
  - {name: 🇨🇳 台湾节点, <<: *use, filter: "台|湾|TW|(?i)Taiwan"}
  - {name: 🇺🇲 美国节点, <<: *use, filter: "美|(?i)States|American"}
  - {name: 🇯🇵 日本节点, <<: *use, filter: "日本|东京|JP|(?i)Japan"}
  - {name: 🇸🇬 狮城节点, <<: *use, filter: "新加|坡|SG|(?i)Singapore"}
  - {name: 🇰🇷 韩国节点, <<: *use, filter: "韩|韓|首尔|KR|(?i)Korea"}
  - {name: ✈️ 其他节点, <<: *use, filter: "^[^香台美本坡韩]*$"}
#  - {name: ✈️ 其他节点, <<: *use, filter: "^((?!香|台|美|日本|新加坡|韩).)*$"}    # 需要meta内核


# 分流规则订阅
# 默认每二十四小时更新一次分流规则
rule-providers:
  Ad: # 广告
    <<: *d
    url: https://cdn.jsdelivr.net/gh/privacy-protection-tools/anti-AD@master/anti-ad-clash.yaml
    path: ./ruleset/anti-ad-clash.yaml
    interval: 86400
  GlobalMedia: # 国际媒体
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/GlobalMedia/GlobalMedia_Classical.yaml
    path: ./ruleset/GlobalMedia.yaml
    interval: 86400
  Amazon: # 亚马逊
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Amazon/Amazon.yaml
    path: ./ruleset/Amazon.yaml
    interval: 86400
  Apple: # 苹果服务
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Apple/Apple_Classical.yaml
    path: ./ruleset/Apple.yaml
    interval: 86400
  Github: # Gtihub
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/GitHub/GitHub.yaml
    path: ./ruleset/Github.yaml
    interval: 86400
  Google: # 谷歌服务
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Google/Google.yaml
    path: ./ruleset/Google.yaml
    interval: 86400
  Telegram: # 电报消息
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Telegram/Telegram.yaml
    path: ./ruleset/Telegram.yaml
    interval: 86400
  Twitter: # 推特消息
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Twitter/Twitter.yaml
    path: ./ruleset/Twitter.yaml
    interval: 86400
  OpenAI: # AI平台
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/OpenAI/OpenAI.yaml
    path: ./ruleset/OpenAI.yaml
    interval: 86400
  NewBing: # AI平台
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Bing/Bing.yaml
    path: ./ruleset/Bing.yaml
    interval: 86400
  BiliBili: # 哔哩哔哩
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/BiliBili/BiliBili.yaml
    path: ./ruleset/BiliBili.yaml
    interval: 86400
  Game: # 游戏平台
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Game/Game.yaml
    path: ./ruleset/Game.yaml
    interval: 86400
  ProxyLite: # 代理网站
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/ProxyLite/ProxyLite.yaml
    path: ./ruleset/ProxyLite.yaml
    interval: 86400
  ChinaIP: # 中国大陆IP
    <<: *i
    url: https://cdn.jsdelivr.net/gh/soffchen/GeoIP2-CN@release/clash-rule-provider.yml
    path: ./ruleset/ChinaIP.yaml
    interval: 86400
  Direct: # 规则修正
    <<: *d
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Direct/Direct.yaml
    path: ./ruleset/Direct.yaml
    interval: 86400
  Lan: # 局域网
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Lan/Lan.yaml
    path: ./ruleset/Lan.yaml
    interval: 86400
  Download: # 下载服务
    <<: *c
    url: https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Download/Download.yaml
    path: ./ruleset/Download.yaml
    interval: 86400

rules:
  #- SCRIPT,quic,REJECT
  - RULE-SET,Ad,🆎 AdBlock
  - RULE-SET,GlobalMedia,🎬 国际媒体
  - RULE-SET,Amazon,🛍️ 亚马逊
  - RULE-SET,Apple,🍎 苹果服务
  - RULE-SET,Github,🌏 全球加速
  - RULE-SET,Google,🌌 谷歌服务
  - RULE-SET,Telegram,📟 电报消息
  - RULE-SET,Twitter,🐦 推特消息
  - RULE-SET,NewBing,🤖 New Bing
  - RULE-SET,BiliBili,📺 哔哩哔哩
  - RULE-SET,Game,🎮 游戏平台
  - RULE-SET,ProxyLite,🌏 全球加速
  - RULE-SET,ChinaIP,DIRECT
  - GEOIP,CN,DIRECT
  - RULE-SET,Direct,DIRECT
  - RULE-SET,Lan,DIRECT
  - RULE-SET,Download,DIRECT
  - MATCH,🐟 兜底分流

###meta内核不支持
#script:
#  shortcuts:
#    quic: network == 'udp' and dst_port == 443
```

