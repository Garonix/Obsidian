
目前，主流的V2Ray客户端已移除PAC模式支持，但可通过"高级路由设置"功能实现相同的自动代理效果。

在代理(Proxy)、直连(Direct)、阻止(Block)三类规则输入框中，按以下规则填写域名或IP。

## 一、域名路由规则的写法

### 预定义域名列表 geosite:

以 `geosite:`开头，后接预定义域名列表名称：

```
geosite:category-ads      # 常见的广告域名
geosite:category-ads-all  # 常见的广告域名及广告提供商域名
geosite:cn                # 常见的大陆站点域名和中国顶级域名合集
geosite:apple             # Apple旗下绝大部分域名
geosite:google            # Google旗下绝大部分域名
geosite:microsoft         # Microsoft旗下绝大部分域名
geosite:facebook          # Facebook旗下绝大部分域名
geosite:twitter           # Twitter旗下绝大部分域名
geosite:telegram          # Telegram旗下绝大部分域名
geosite:geolocation-cn    # 常见的大陆站点域名
geosite:geolocation-!cn   # 常见的非大陆站点域名
geosite:tld-cn            # CNNIC管理的中国大陆顶级域名(.cn、.中国等)
geosite:tld-!cn           # 非中国大陆使用的顶级域名(.hk、.tw、.jp等)
```

更多域名类别见[data目录](https://github.com/v2fly/domain-list-community/tree/master/data)

### 域名 domain:

由 `domain:`开始，后面是域名。例如 `domain:baiyunju.cc`匹配主域名及其所有子域名。
该前缀可省略，直接输入域名即可。

### 完整匹配 full:

由 `full:`开始，后面是域名。例如 `full:baiyunju.cc`只匹配完整域名，不匹配子域名。

### 纯字符串

直接输入域名，如 `sina.com`。可分行，也可用英文逗号分隔。匹配包含该字符串的域名。

### 正则表达式 regexp:

由 `regexp:`开始，后面是正则表达式。例如 `regexp:\.goo.*\.com$`。

### 外部文件导入 ext:

格式为 `ext:file:tag`，从资源目录中的外部文件导入规则。

## 二、IP路由规则的写法

### geoip:

以 `geoip:`开头，后接双字符国家代码，如：

```
geoip:private     # 所有私有地址
geoip:cn          # 中国大陆境内IP
geoip:cloudflare  # Cloudflare的IP
geoip:cloudfront  # Cloudfront的IP
geoip:facebook    # Facebook的IP
geoip:fastly      # Fastly的IP
geoip:google      # Google的IP
geoip:netflix     # Netflix的IP
geoip:telegram    # Telegram的IP
geoip:twitter     # Twitter的IP
```

**重要提示**：以上geoip列表需配合加强版资源文件geoip.dat、geosite.dat使用，详见[v2ray-rules-dat](https://github.com/Loyalsoldier/v2ray-rules-dat)

### 特殊值

`geoip:private`包含所有私有地址，如127.0.0.1（V2Ray 3.5+支持）。

### IP

直接输入IP地址，如 `127.0.0.1`或 `20.194.25.232`。

### CIDR

输入CIDR格式地址，如 `10.0.0.0/8`。

### 外部文件导入

格式为 `ext:file:tag`，从资源目录中的外部文件导入规则。
