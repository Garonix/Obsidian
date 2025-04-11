[[干货]泰拉瑞亚TShock配置文件全文翻译复制即用（1.4.0.5，1.4.3.6） - 哔哩哔哩](https://www.bilibili.com/read/cv15882660?spm_id_from=333.999.0.0) 

{

  "ServerPassword": "", //服务器密码

  "ServerPort": 7777, //服务器端口

  "MaxSlots": 235,  //最大人数

  "ReservedSlots": 20, //预留给管理员的人数

  "ServerName": "三连",  //服务器名称 （请检查是否是 utf编码）（如果不是，记事本打开另存为选择utf-8）

  "UseServerName": false, //是否使用服务器名称

  "LogPath": "tshock", //日志路径，像默认填的 "tshock"就是指 服务器根目录\\tshock

  "DebugLogs": false,  //报错日志（建议关闭要不烦人）

  "DisableLoginBeforeJoin": false, //踢出登录失败玩家

  "IgnoreChestStacksOnLoad": false, //忽略箱子堆叠上限

  "AutoSave": true, //自动保存

  "AnnounceSave": true, //自动保存提示（开启后自动保存会在聊天框里显示 saving world...）

  "ShowBackupAutosaveMessages": true,  //自动保存提示（显示自动保存消息）

  "BackupInterval": 0, //地图备份间隔时间

  "BackupKeepFor": 60, //备份保留时间

  "SaveWorldOnCrash": true, //崩溃时自动保存

  "SaveWorldOnLastPlayerExit": true, //在最后一个玩家退出后保存世界

  "InvasionMultiplier": 1, //入侵乘数（人话：打事件进度条需要多少怪）

  "DefaultMaximumSpawns": 5,  //刷怪最大数量

  "DefaultSpawnRate": 600,  //刷怪间隔

  "InfiniteInvasion": false,  //无限入侵（开了之后打事件没完）

  "PvPMode": "normal",  //双引号中 always开启强制pvp disabled强制不让pvp normal正常模式

  "SpawnProtection": false, //出生点保护(岩浆和水生成黑曜石可以恶意堵上)

  "SpawnProtectionRadius": 30, //出生点保护范围（出生点为中心的正方形）

  "RangeChecks": true,  //范围检查（检查超出数组范围）（物品堆叠和其他的）

  "HardcoreOnly": false,  //仅允许硬核人物进入服务器

  "MediumcoreOnly": false, //仅允许中核进入

  "DisableBuild": false,  //是否禁止建筑 为true禁止放方块挖方块

  "DisableHardmode": false, //如果为true禁止进入肉后

  "DisableDungeonGuardian": false, //禁止打吴克（骷髅王）

  "DisableClownBombs": true, //禁止小丑（日食怪）扔炸弹爆炸

  "DisableSnowBalls": false, //禁止雪人军团爆炸

  "DisableTombstones": true,  //禁止巨石

  "ForceTime": "normal", //day永远白天 night永夜 normal正常

  "DisableInvisPvP": false,  //打架的时候隐身药水是否失效

  "MaxRangeForDisabled": 10,  //被禁的时候最大移动距离

  "RegionProtectChests": false,  //领地内箱子保护

  "RegionProtectGemLocks": true,  //领地内宝石锁保护

  "IgnoreProjUpdate": false,  //忽略射弹更新检查

  "IgnoreProjKill": false,  //忽略射弹击毁检查

  "AllowCutTilesAndBreakables": false, //允许玩家破坏易碎方块（草或者花）

  "AllowIce": false, //允许玩家在无法建筑的地方放冰块

  "AllowCrimsonCreep": true,  //猩红蔓延

  "AllowCorruptionCreep": true, //腐化蔓延

  "AllowHallowCreep": true, //神圣蔓延

  "StatueSpawn200": 3, //雕像在200格内生成最大数量

  "StatueSpawn600": 6, //同上，但是是600格

  "StatueSpawnWorld": 10, //雕像可以生成多少npc

  "PreventBannedItemSpawn": false, //防止被禁的物品生成或指令拿到

  "PreventDeadModification": true, //防止死后作妖

  "PreventInvalidPlaceStyle": true, //防止放置无效方块

  "ForceXmas": false, //强制圣诞节

  "ForceHalloween": false, //强制万圣节

  "AllowAllowedGroupsToSpawnBannedItems": true,  //允许管理员give被禁的物品

  "RespawnSeconds": 5,  //重生时间

  "RespawnBossSeconds": 10,  //老怪（划掉）boss战复活时间

  "AnonymousBossInvasions": true,  //公告事件（boss苏醒 事件入侵）

  "MaxHP": 500,  //最大生命值（水晶+生命果）（不算装备增益）

  "MaxMP": 200,  //最大蓝条（同上）

  "BombExplosionRadius": 5,  //炸弹影响范围

  "DefaultRegistrationGroupName": "default",  //新注册玩家的权限组

  "DefaultGuestGroupName": "guest",  //未注册权限组

  "RememberLeavePos": true, //储存最后下线位置（重启失效）

  "MaximumLoginAttempts": 3, //登录失败最大次数

  "KickOnMediumcoreDeath": false,  //中核死亡后踢出服务器

  "MediumcoreKickReason": "Death results in a kick",  //踢出中核的原因

  "BanOnMediumcoreDeath": false,  //是否三件套死亡的中核玩家（三件套：石化冰冻网住）

  "MediumcoreBanReason": "Death results in a ban",  //中核被ban的原因

  "EnableWhitelist": false,  //根据whitelist里的ip开白名单（和配置文件一个目录）

  "WhitelistKickReason": "You are not on the whitelist.",  //不在白名单里被踢的原因

  "ServerFullReason": "Server is full",  //人满被踢的原因

  "ServerFullNoReservedReason": "Server is full. No reserved slots open.",  //保留人数到上限被踢的原因

  "KickOnHardcoreDeath": false,  //是否踢出死亡的硬核玩家

  "HardcoreKickReason": "Death results in a kick",  //踢出死亡硬核玩家的原因

  "BanOnHardcoreDeath": false,  //是否ban死亡的硬核玩家

  "HardcoreBanReason": "Death results in a ban",  //三件套死亡硬核玩家原因

  "EnableIPBans": true,  //ban玩家的时候顺道ban IP

  "EnableUUIDBans": true,  //ban玩家的时候ban客户端的UUID

  "EnableBanOnUsernames": false,  //ban玩家的时候ban角色名

  "KickProxyUsers": false,  //如果GeoIP可用，ban连代理服务器的玩家（人话：连某种和谐的东西就踢了）

  "RequireLogin": false,  //强制注册登录

  "AllowLoginAnyUsername": true,  //允许玩家登录的账号与角色名不符

  "AllowRegisterAnyUsername": false,  //允许玩家注册与角色名称不同的账号

  "MinimumPasswordLength": 4, //新账号密码的最短长度（最大2147483647）

  "HashAlgorithm": "sha512",  //密码加密算法（sha512 sha256 md5）

  "BCryptWorkFactor": 7,  //决定使用的 BCrypt 工作因子

  "DisableUUIDLogin": false,  //防止用客户端UUID登录

  "KickEmptyUUID": false,   //踢出不把UUID上传服务器的玩家

  "TilePaintThreshold": 15,  //如果一秒刷漆超过这个上限就石化冰冻网住三件套

  "KickOnTilePaintThresholdBroken": false,  //玩家超过刷漆上限是否ban

  "MaxDamage": 1175,    //玩家可以造成的最大伤害

  "MaxProjDamage": 1175,  //射弹最大伤害

  "KickOnDamageThresholdBroken": false,  //超过最大伤害是否踢出服务器

  "TileKillThreshold": 60,  //如果一秒内破坏方块超过这个数，三件套加方块复原

  "KickOnTileKillThresholdBroken": false,  //超过破坏方块上限是否ban

  "TilePlaceThreshold": 32,  //一秒内放置方块上限，超过三件套加复原

  "KickOnTilePlaceThresholdBroken": false,  //超过放置上限是否ban

  "TileLiquidThreshold": 50,  //一秒内放置液体上限，超过还是老样子

  "KickOnTileLiquidThresholdBroken": false,  //超过液体放置上限是否ban

  "ProjIgnoreShrapnel": true,  //射弹数量计算是否加上水晶子弹碎片

  "ProjectileThreshold": 50,  //一秒内发射超过这些射弹，直接三件套

  "KickOnProjectileThresholdBroken": false,  //超过发射上限是否ban

  "HealOtherThreshold": 50,  //一秒内发送超过治疗（绿字）数据包上限，超过同上

  "KickOnHealOtherThresholdBroken": false, //超过治疗（绿字）数据包上限是否踢出

  "CommandSpecifier": "/",  //指令前缀（一个字符不能为中文）（如果设为\* 那么打指令是这样的 \* give）（默认就行）

  "CommandSilentSpecifier": ".",  //指令静音处理前缀（没有任何动静处理命令，恶搞必备）

  "DisableSpewLogs": true,    //阻止日志记录发送给有日志权限的玩家

  "DisableSecondUpdateLogs": false,  //防止二次更新（是这么叫吗）检查写入日志

  "SuperAdminChatRGB": \[  //管理员权限组在聊天框显示颜色（rgb）

    255,

    255,

    255

  \],                                        

  "SuperAdminChatPrefix": "(Super Admin) ", //管理员权限组在聊天框显示的前置字（低调点就不填或者空格，双引号里的括号能删）

  "SuperAdminChatSuffix": "",   //管理员权限组在聊天框后置显示的字

  "EnableGeoIP": false,   //是否根据玩家ip在加入服务器的时候广播国家/地区

  "DisplayIPToAdmins": false,   //向具有日志权限的玩家显示加入玩家的ip

  "ChatFormat": "{1}{2}{3}: {4}",  //聊天格式{0}=权限组名称 {1}=权限组前置字串 {2}=玩家名称 {3}=权限组后置字串 {4}=聊天信息（注意！双引号里那个冒号也能改！）

  "ChatAboveHeadsFormat": "{2}",    //用聊天泡泡时改变玩家名称

  "EnableChatAboveHeads": false,  //是否在玩家头上显示聊天信息（这玩意就是聊天泡泡）

  "BroadcastRGB": \[     //广播信息的rgb颜色

    127,

    255,

    212

  \],

  //以下小白直接跳过就行

  "StorageType": "sqlite",    //数据库类型 可选 mysql  sqlite                    

  "SqliteDBPath": "tshock.sqlite",  //储存数据库路径（像tshock.sqlite的意思就是 服务器根目录\\tshock）

  "MySqlHost": "localhost:3306",  //mysql连接的ip和端口（localhost是本地  冒号加3306表示3306端口）

  "MySqlDbName": "",      //mysql作为存储类型时用的数据库名称

  "MySqlUsername": "",    //mysql作为存储类型时用的账号

  "MySqlPassword": "",    //mysql作为存储类型时用的密码

  "UseSqlLogs": false,    //是否将日志存到数据库而不是txt

  "RevertToTextLogsOnSqlFailures": 10,   //SQL日志必须在回复到文本日志之前无法插入日志的次数

  "RestApiEnabled": false,    //是否开启rest api 

  "RestApiPort": 7878,    //rest api使用的端口

  "LogRest": false,   //是否记录rest api连线

  "EnableTokenEndpointAuthentication": false,   //是否通过权限认证才能使用

  "RESTMaximumRequestsPerInterval": 5,    //拒绝请求前连线池中最大的rest请求数（最小为5）

  "RESTRequestBucketDecreaseIntervalMinutes": 1,    //rest请求连线池以分钟为单位所减少1单位数量的频率（最小为1（分钟））

  "ApplicationRestTokens": {}   //外部应用可对服务器查询的rest权限列表

}

![](../_resources/4adb9255ada5b97061e610b682b86367_3c3905fffe454ea28.png)

{

  "Enabled": false,  //强制开荒是否开启

  //如果未开启以下都不用看

  "ServerSideCharacterSave": 5,  //保存服务器人物资料的时间（单位分钟）（强制开荒人物资料存在服务器）

  "LogonDiscardThreshold": 250,  //登录后不允许丢东西的时间

  "StartingHealth": 100,    //强制开荒初始生命值

  "StartingMana": 20,     //同上，但是初始魔力值

  "StartingInventory": \[    //强制开荒初始物品  （吐槽：为什么不是泰拉原版的物品id）

    {                     //netID：物品id  在这里看 https://tshock.readme.io/docs/item-list-10  这个文档可能会抽风

      "netID": -15,       //prefix：物品前缀  这里看 https://tshock.readme.io/docs/prefix-list

      "prefix": 0,        //stack：物品数量

      "stack": 1

    },

    {

      "netID": -13,

      "prefix": 0,

      "stack": 1

    },

    {

      "netID": -16,

      "prefix": 0,

      "stack": 1

    }

  \]   //这里为什么不是大括号我没看明白（不报错）

}

![](../_resources/02db465212d3c374a43c60fa2625cc1c_c52934f2664645a2a.png)

手动上色，肝吃不消

![](../_resources/2ed5fe92eef43859f0ec9d183a390025_ee3ebb804e2e4c4f9.webp)

点个赞吧！

{

  "Settings": {

    "ServerPassword": "",   //服务器密码

    "ServerPort": 7777,     //服务器端口

    "MaxSlots": 8,          //最大人数

    "ReservedSlots": 20,    //预留给管理员的人数

    "ServerName": "三连",       //服务器名称 （请检查是否是 utf编码）（如果不是记事本打开另存为选择utf-8）

    "UseServerName": false,   //是否使用服务器名称

    "LogPath": "tshock/logs",   //日志路径，像默认填的 "tshock/logs"就是指 服务器根目录\\tshock\\logs

    "DebugLogs": false,     //报错日志（建议关闭要不烦人）

    "DisableLoginBeforeJoin": false,      //踢出登录失败玩家

    "IgnoreChestStacksOnLoad": false,     //忽略箱子堆叠上限

    "AutoSave": true,       //自动保存

    "AnnounceSave": true,   //自动保存提示（开启后自动保存会在聊天框里显示 saving world...）

    "ShowBackupAutosaveMessages": true,     //自动保存提示（显示自动保存消息）

    "BackupInterval": 10,       //地图备份间隔时间

    "BackupKeepFor": 240,       //备份保留时间

    "SaveWorldOnCrash": true,       //崩溃时自动保存

    "SaveWorldOnLastPlayerExit": true,      //在最后一个玩家退出后保存世界

    "InvasionMultiplier": 1,        //入侵乘数（人话：打事件进度条需要多少怪）

    "DefaultMaximumSpawns": 5,      //刷怪最大数量

    "DefaultSpawnRate": 600,        //刷怪间隔

    "InfiniteInvasion": false,      //无限入侵（开了之后打事件没完）

    "PvPMode": "normal",      //双引号中 always开启强制pvp disabled强制不让pvp normal正常模式

    "SpawnProtection": false,      //出生点保护(岩浆和水生成黑曜石可以恶意堵上)

    "SpawnProtectionRadius": 10,    //出生点保护范围（出生点为中心的正方形）

    "RangeChecks": true,      //范围检查（检查超出数组范围）（物品堆叠和其他的）

    "HardcoreOnly": false,      //仅允许硬核人物进入服务器

    "MediumcoreOnly": false,    //仅允许中核进入

    "SoftcoreOnly": false,      //仅允许软核

    "DisableBuild": false,    //是否禁止建筑 为true禁止放方块挖方块

    "DisableHardmode": false,   //如果为true禁止进入肉后

    "DisableDungeonGuardian": false,  //禁止打骷髅王

    "DisableClownBombs": false,     //禁止小丑（日食怪）扔炸弹爆炸

    "DisableSnowBalls": false,      //禁止雪人军团爆炸

    "DisableTombstones": true,      //禁止巨石

    "DisablePrimeBombs": false,     //禁用炸弹

    "ForceTime": "normal",      //day永远白天 night永夜 normal正常

    "DisableInvisPvP": false,     //打架的时候隐身药水是否失效

    "MaxRangeForDisabled": 10,      //被禁的时候最大移动距离

    "RegionProtectChests": false,   //领地内箱子保护

    "RegionProtectGemLocks": true,      //领地内宝石锁保护

    "IgnoreProjUpdate": false,        //忽略射弹更新检查

    "IgnoreProjKill": false,      //忽略射弹击毁检查

    "AllowCutTilesAndBreakables": false,      //允许玩家破坏易碎方块（草或者花）

    "AllowIce": false,      //允许玩家在无法建筑的地方放冰块

    "AllowCrimsonCreep": true,    //猩红蔓延

    "AllowCorruptionCreep": true,     //腐化蔓延

    "AllowHallowCreep": true,     //神圣蔓延

    "StatueSpawn200": 3,    //雕像在200格内生成最大数量

    "StatueSpawn600": 6,    //同上，但是是600格

    "StatueSpawnWorld": 10,   //雕像可以生成多少npc

    "PreventBannedItemSpawn": false,    //防止被禁的物品生成或指令拿到

    "PreventDeadModification": true,    //防止死后作妖

    "PreventInvalidPlaceStyle": true,    //防止放置无效方块

    "ForceXmas": false,     //强制圣诞节

    "ForceHalloween": false,    //强制万圣节

    "AllowAllowedGroupsToSpawnBannedItems": true,    //允许管理员give被禁的物品

    "RespawnSeconds": 0,    //重生时间

    "RespawnBossSeconds": 0,      //boss战复活时间

    "AnonymousBossInvasions": true,   //公告事件（boss苏醒 事件入侵）

    "MaxHP": 500,   //最大生命值（水晶+生命果）（不算装备增益）

    "MaxMP": 200,  //最大蓝条（同上）

    "BombExplosionRadius": 5,  //炸弹影响范围

    "DefaultRegistrationGroupName": "default",    //新注册玩家的权限组

    "DefaultGuestGroupName": "guest",   //未注册权限组

    "RememberLeavePos": false,    //储存最后下线位置（重启失效）

    "MaximumLoginAttempts": 3,    //登录失败最大次数

    "KickOnMediumcoreDeath": false,   //中核死亡后踢出服务器

    "MediumcoreKickReason": "Death results in a kick",    //踢出中核的原因

    "BanOnMediumcoreDeath": false,    //是否三件套死亡的中核玩家（三件套：石化冰冻网住）

    "MediumcoreBanReason": "Death results in a ban",    //中核被ban的原因

    "DisableDefaultIPBan": false,  //禁用默认ip

    "EnableWhitelist": false,       //根据whitelist里的ip开白名单（和配置文件一个目录）

    "WhitelistKickReason": "You are not on the whitelist.",   //不在白名单里被踢的原因

    "ServerFullReason": "Server is full", //人满被踢的原因

    "ServerFullNoReservedReason": "Server is full. No reserved slots open.",    //保留人数到上限被踢的原因

    "KickOnHardcoreDeath": false,   //是否踢出死亡的硬核玩家

    "HardcoreKickReason": "Death results in a kick",  //踢出死亡硬核玩家的原因

    "BanOnHardcoreDeath": false,    //是否三件套死亡的硬核玩家

    "HardcoreBanReason": "Death results in a ban", //三件套死亡硬核玩家原因

    "KickProxyUsers": true,   //如果GeoIP可用，ban连代理服务器的玩家（人话：连某个被和谐的东西就踢了）

    "RequireLogin": false,    //强制注册登录

    "AllowLoginAnyUsername": true,    //允许玩家登录的账号与角色名不符

    "AllowRegisterAnyUsername": false,    //允许玩家注册与角色名称不同的账号

    "MinimumPasswordLength": 4,   //新账号密码的最短长度（最大2147483647）

    "BCryptWorkFactor": 7,    //决定使用的 BCrypt 工作因子

    "DisableUUIDLogin": false,    //防止用客户端UUID登录

    "KickEmptyUUID": false,   //踢出不把UUID上传服务器的玩家

    "TilePaintThreshold": 15,   //如果一秒刷漆超过这个上限就石化冰冻网住三件套

    "KickOnTilePaintThresholdBroken": false,    //玩家超过刷漆上限是否ban

    "MaxDamage": 1175,    //玩家可以造成的最大伤害

    "MaxProjDamage": 1175,    //射弹最大伤害

    "KickOnDamageThresholdBroken": false,   //超过最大伤害是否踢出服务器

    "TileKillThreshold": 60,    //如果一秒内破坏方块超过这个数，三件套加方块复原

    "KickOnTileKillThresholdBroken": false,   //超过破坏方块上限是否ban

    "TilePlaceThreshold": 32,  //一秒内放置方块上限，超过三件套加复原

    "KickOnTilePlaceThresholdBroken": false,    //超过放置上限是否ban

    "TileLiquidThreshold": 50,    //一秒内放置液体上限，超过还是老样子

    "KickOnTileLiquidThresholdBroken": false,   //超过液体放置上限是否ban

    "ProjIgnoreShrapnel": true,   //射弹数量计算是否加上水晶子弹碎片

    "ProjectileThreshold": 50,    //一秒内发射超过这些射弹，直接三件套

    "KickOnProjectileThresholdBroken": false,   //超过发射上限是否ban

    "HealOtherThreshold": 50, //一秒内发送超过治疗（绿字）数据包上限，超过同上

    "KickOnHealOtherThresholdBroken": false,  //超过治疗（绿字）数据包上限是否踢出

    "TileRectangleSizeThreshold": 50,   //物块大小阈值

    "KickOnTileRectangleSizeThresholdBroken": false, //超过物块大小阈值三件套

    "SuppressPermissionFailureNotices": false,  //禁止显示权限失败通知（人话：权限不够打完指令也不告诉你）

    "DisableModifiedZenith": false,   //禁用修改后的天顶剑

    "DisableCustomDeathMessages": true,   //禁用自定义死亡消息

    "CommandSpecifier": "/",  //指令前缀（一个字符不能为中文）（如果设为\* 那么打指令是这样的 \* give）（默认就行）

    "CommandSilentSpecifier": ".",    //指令静音处理前缀（没有任何动静处理命令，恶搞必备）

    "DisableSpewLogs": true,    //阻止日志记录发送给有日志权限的玩家

    "DisableSecondUpdateLogs": false,   //防止二次更新（是这么叫吗）检查写入日志

    "SuperAdminChatRGB": \[  //管理员权限组在聊天框显示颜色（rgb）

      255,

      255,

      255

    \],                                         

    "SuperAdminChatPrefix": "(Super Admin) ",   //管理员权限组在聊天框显示的前置字（低调点就不填或者空格，双引号里的括号能删）

    "SuperAdminChatSuffix": "",                  //管理员权限组在聊天框后置显示的字

    "EnableGeoIP": false,                    //是否根据玩家ip在加入服务器的时候广播国家/地区

    "DisplayIPToAdmins": false,         //向具有日志权限的玩家显示加入玩家的ip

    "ChatFormat": "{1}{2}{3}: {4}",     //聊天格式{0}=权限组名称 {1}=权限组前置字串 {2}=玩家名称 {3}=权限组后置字串 {4}=聊天信息（注意！双引号里那个冒号也能改！）

    "ChatAboveHeadsFormat": "{2}",      //用聊天泡泡时改变玩家名称

    "EnableChatAboveHeads": false,      //是否在玩家头上显示聊天信息（这玩意就是聊天泡泡）

    "BroadcastRGB": \[        //广播信息的rgb颜色

      127,

      255,

      212

    \],

    //以下小白直接跳过就行

    "StorageType": "sqlite",      //数据库类型 可选 mysql  sqlite

    "SqliteDBPath": "tshock.sqlite",       //储存数据库路径（像tshock.sqlite的意思就是 服务器根目录\\tshock）

    "MySqlHost": "localhost:3306",    //mysql连接的ip和端口（localhost是本地  冒号加3306表示3306端口）

    "MySqlDbName": "",      //mysql作为存储类型时用的数据库名称

    "MySqlUsername": "",    //mysql作为存储类型时用的账号

    "MySqlPassword": "",    //mysql作为存储类型时用的密码

    "UseSqlLogs": false,     //是否将日志存到数据库而不是txt

    "RevertToTextLogsOnSqlFailures": 10,    //SQL日志必须在回复到文本日志之前无法插入日志的次数

    "RestApiEnabled": false,    //是否开启rest api

    "RestApiPort": 7878,        //rest api使用的端口

    "LogRest": false,           //是否记录rest api连线

    "EnableTokenEndpointAuthentication": false,     //是否通过权限认证才能使用

    "RESTMaximumRequestsPerInterval": 5,        //拒绝请求前连线池中最大的rest请求数（最小为5）

    "RESTRequestBucketDecreaseIntervalMinutes": 1,       //rest请求连线池以分钟为单位所减少1单位数量的频率（最小为1（分钟））

    "ApplicationRestTokens": {}    //外部应用可对服务器查询的rest权限列表

  }

}

{

  "Settings": {

"Enabled": false,   //强制开荒是否开启

    //如果未开启以下都不用看

    "ServerSideCharacterSave": 5, //保存服务器人物资料的时间（单位分钟）（强制开荒人物资料存在服务器）

    "LogonDiscardThreshold": 250, //登录后不允许丢东西的时间

    "StartingHealth": 100,    //强制开荒初始生命值

    "StartingMana": 20,     //同上，但是初始魔力值

    "StartingInventory": \[    //强制开荒初始物品  （吐槽：为什么不是泰拉原版的物品id）

      {                         //netID：物品id  在这里看 https://tshock.readme.io/docs/item-list-10  这个文档可能会抽风

        "netID": -15,           //prefix：物品前缀  这里看 https://tshock.readme.io/docs/prefix-list

        "prefix": 0,             //stack：物品数量

        "stack": 1

      },

      {

        "netID": -13,

        "prefix": 0,

        "stack": 1

      },

      {

        "netID": -16,

        "prefix": 0,

        "stack": 1

      }

    \],  //这里为什么不是大括号我没看明白

    "WarnPlayersAboutBypassPermission": true    //警告玩家有关绕过权限的信息

  }

}

在翻译的时候查到了这个wiki https://wiki.snkms.com/wiki/TShock\_設定檔配置及權限介紹

如果我有翻译错的地方欢迎指出，看不懂可以去上面的wiki看  

纯手翻加上色耗时四天（才没有摸鱼呢！）点个赞吧！