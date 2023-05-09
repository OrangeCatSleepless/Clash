# 来源
- https://github.com/Fndroid/clash_for_windows_pkg/issues/2729#issue-1151910122

```yaml
parsers:`` # array
  - reg: ^.*$  
  # - reg: ^.*$ 匹配所有订阅，或  - url: https://example.com/profile.yaml 指定订阅
  # 下面是删除服务商自带的策略组和规则
    code: |
      module.exports.parse = (raw, { yaml }) => {
        const rawObj = yaml.parse(raw)
        const groups = []
        const rules = []
        return yaml.stringify({ ...rawObj, 'proxy-groups': groups, rules })
      } 
  # 建立自己的配置
    yaml:
      prepend-proxy-groups: # 建立策略组
      - name: PROXY
        type: fallback
        url: http://www.google.com/generate_204
        interval: 300
        proxies:
         - 香港
         - 台湾
         - 日本
         - 新加坡
         - 韩国
         - 美国
         - 其他

      - name: Netflix
        type: select
      
      - name: 香港 
        type: url-test
        url: http://www.google.com/generate_204
        interval: 300
      
      - name: 台湾 
        type: url-test
        url: http://www.google.com/generate_204
        interval: 300
       
      - name: 日本
        type: url-test
        url: http://www.google.com/generate_204
        interval: 300
        
      - name: 新加坡 
        type: url-test
        url: http://www.google.com/generate_204
        interval: 300
       
      - name: 韩国 
        type: url-test
        url: http://www.google.com/generate_204
        interval: 300
       
      - name: 美国 
        type: url-test
        url: http://www.google.com/generate_204
        interval: 300
       
      - name: 其他 
        type: url-test
        url: http://www.google.com/generate_204
        interval: 300
              
  # 策略组示例
      # - name: 分组名
        # type: select       # 手动选点   
        # url-test     # 自动选择延迟最低的节点
        # fallback     # 节点故障时自动切换下一个
        # laod-balance # 均衡使用分组内的节点
        # url: http://www.gstatic.com/generate_204 # 测试地址 非select类型分组必要
        # interval: 300 # 自动测试间隔时间，单位秒 非select类型分组必要
        # tolerance: 50 # 允许的偏差，节点之间延迟差小于该值不切换 非必要
        # proxies:  
          # - 节点名称或其他分组套娃
          
      commands:
        - proxy-groups.Netflix.proxies=[]proxyNames|奈飞|nf|解锁
        - proxy-groups.香港.proxies=[]proxyNames|香港 # 向指定策略组添加订阅中的节点名，可使用正则过滤
        - proxy-groups.台湾.proxies=[]proxyNames|台
        - proxy-groups.日本.proxies=[]proxyNames|日本
        - proxy-groups.韩国.proxies=[]proxyNames|韩国
        - proxy-groups.新加坡.proxies=[]proxyNames|新加坡
        - proxy-groups.美国.proxies=[]proxyNames|美国
        - proxy-groups.其他.proxies=[]proxyNames|^[^香日台新韩美]*$ # 这个可以自定义，这个正则的意思是排除节点名字有“香日台新韩美”的节点。

       # 为各个策略组添加一个DIRECT，避免机场节点无法匹配上面的正则筛选而导致配置出错。应该有其他办法避免，但是我不会。
        - proxy-groups.Netflix.proxies.0+DIRECT
        - proxy-groups.香港.proxies.0+DIRECT
        - proxy-groups.台湾.proxies.0+DIRECT
        - proxy-groups.日本.proxies.0+DIRECT
        - proxy-groups.韩国.proxies.0+DIRECT
        - proxy-groups.新加坡.proxies.0+DIRECT
        - proxy-groups.美国.proxies.0+DIRECT
        - proxy-groups.其他.proxies.0+DIRECT

        # - proxy-groups.节点名字.proxies.0+DIRECT # 向指定分组第一个位置添加一个 DIRECT 节点名
        # 一些可能用到的正则过滤节点示例，使分组更细致
        # []proxyNames|a                         # 包含a
        # []proxyNames|^(.*)(a|b)+(.*)$          # 包含a或b
        # []proxyNames|^(?=.*a)(?=.*b).*$        # 包含a和b
        # []proxyNames|^((?!b).)*a((?!b).)*$     # 包含a且不包含b
        # []proxyNames|^((?!b|c).)*a((?!b|c).)*$ # 包含a且不包含b或c
        # 更多正则教程，请看这里：https://deerchao.cn/tutorials/regex/regex.htm#top
        
  # 添加规则
      prepend-rules: # 规则由上往下遍历，如上面规则已经命中，则不再往下处理
        - RULE-SET,netflix,Netflix
        - RULE-SET,applications,DIRECT
        - DOMAIN,clash.razord.top,DIRECT
        - DOMAIN,yacd.haishan.me,DIRECT
        - RULE-SET,private,DIRECT
        - RULE-SET,reject,REJECT
        - RULE-SET,icloud,DIRECT #
        - RULE-SET,apple,DIRECT # 这三个为国内可直连地址，如果希望走代理改为PROXY
        - RULE-SET,google,DIRECT # 
        - RULE-SET,tld-not-cn,PROXY
        - RULE-SET,gfw,PROXY
        - RULE-SET,greatfire,PROXY
        - RULE-SET,telegramcidr,PROXY
        - RULE-SET,lancidr,DIRECT
        - RULE-SET,cncidr,DIRECT
        - GEOIP,,DIRECT
        - GEOIP,CN,DIRECT
        - RULE-SET,direct,DIRECT
        - RULE-SET,proxy,PROXY
        - MATCH,PROXY # 规则之外的，在这里来修改是白名单模式还是黑名单模式，把“PROXY”改成“DIRECT"就好了。
  # 添加规则集
      mix-rule-providers: 
        reject: # 广告域名列表
          type: http
          behavior: domain
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/reject.txt"
          path: ./ruleset/reject.yaml
          interval: 86400
          
        icloud: # iCloud 域名列表
          type: http
          behavior: domain
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/icloud.txt"
          path: ./ruleset/icloud.yaml
          interval: 86400
          
        apple: # Apple 在中国大陆可直连的域名列表
          type: http
          behavior: domain
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/apple.txt"
          path: ./ruleset/apple.yaml
          interval: 86400
          
        google: # Google 在中国大陆可直连的域名列表
          type: http
          behavior: domain
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/google.txt"
          path: ./ruleset/google.yaml
          interval: 86400
          
        proxy: # 代理域名列表
          type: http
          behavior: domain
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/proxy.txt"
          path: ./ruleset/proxy.yaml
          interval: 86400
          
        direct: # 直连域名列表
          type: http
          behavior: domain
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/direct.txt"
          path: ./ruleset/direct.yaml
          interval: 86400
          
        private: # 私有网络专用域名列表
          type: http
          behavior: domain
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/private.txt"
          path: ./ruleset/private.yaml
          interval: 86400
          
        gfw: # GFWList 域名列表
          type: http
          behavior: domain
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/gfw.txt"
          path: ./ruleset/gfw.yaml
          interval: 86400
          
        greatfire: # GreatFire 域名列表
          type: http
          behavior: domain
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/greatfire.txt"
          path: ./ruleset/greatfire.yaml
          interval: 86400
          
        tld-not-cn: # 非中国大陆使用的顶级域名列表
          type: http
          behavior: domain
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/tld-not-cn.txt"
          path: ./ruleset/tld-not-cn.yaml
          interval: 86400
          
        telegramcidr: # Telegram 使用的 IP 地址列表
          type: http
          behavior: ipcidr
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/telegramcidr.txt"
          path: ./ruleset/telegramcidr.yaml
          interval: 86400
          
        cncidr: # 中国大陆 IP 地址列表
          type: http
          behavior: ipcidr
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/cncidr.txt"
          path: ./ruleset/cncidr.yaml
          interval: 86400
          
        lancidr: # 局域网 IP 及保留 IP 地址列表
          type: http
          behavior: ipcidr
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/lancidr.txt"
          path: ./ruleset/lancidr.yaml
          interval: 86400
          
        applications: # 需要直连的常见软件列表
          type: http
          behavior: classical
          url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/applications.txt"
          path: ./ruleset/applications.yaml
          interval: 86400

        netflix: # 奈飞的分流规则
          type: http
          behavior: classical
          url: "https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/release/rule/Clash/Netflix/Netflix.yaml"
          path: ./ruleset/netflix.yaml
          interval: 86400
```
