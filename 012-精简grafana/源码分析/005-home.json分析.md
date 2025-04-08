```json
{
  // 顶层链接配置,目前为空数组
  "links": [],
  
  // 面板配置数组,定义了主页上显示的所有面板
  "panels": [
    {
      // 欢迎面板配置
      "datasource": null,  // 无需数据源
      "gridPos": {        // 网格位置配置
        "h": 3,          // 高度为3个单位
        "w": 24,         // 宽度占满(总宽度24)
        "x": 0,          // 从左侧开始
        "y": 0           // 从顶部开始
      },
      "id": 1,           // 面板唯一标识
      "title": "",       // 面板标题为空
      "transparent": false, // 非透明背景
      "type": "welcome"    // 使用welcome类型面板
    },
    {
      // 仪表板列表面板配置
      "datasource": null,
      "folderId": 0,     // 根文件夹
      "gridPos": {
        "h": 15,         // 高度15个单位
        "w": 12,         // 宽度占一半
        "x": 0,          // 左侧
        "y": 4           // 从第4行开始
      },
      "id": 3,
      "links": [],
      "options": {
        "showStarred": true,          // 显示已加星标的仪表板
        "showRecentlyViewed": true,   // 显示最近查看的仪表板
        "showSearch": false,          // 不显示搜索框
        "showHeadings": true,         // 显示标题
        "folderId": 0,               // 根文件夹
        "maxItems": 30,              // 最多显示30个项目
        "tags": [],                  // 标签过滤
        "query": ""                  // 搜索查询
      },
      "title": "Dashboards",         // 面板标题
      "type": "dashlist"             // 使用dashlist类型面板
    },
    {
      // 博客新闻面板配置
      "datasource": null,
      "gridPos": {
        "h": 15,        // 高度15个单位
        "w": 12,        // 宽度占一半
        "x": 12,        // 右侧
        "y": 4          // 从第4行开始
      },
      "id": 4,
      "links": [],
      "options": {
        "feedUrl": "https://grafana.com/blog/news.xml"  // RSS订阅源
      },
      "title": "Latest from the blog",  // 面板标题
      "type": "news"                    // 使用news类型面板
    }
  ],

  // 仪表板通用配置
  "schemaVersion": 22,        // schema版本号
  "tags": [],                 // 仪表板标签
  "templating": {             // 模板变量配置
    "list": []
  },
  "time": {                   // 时间范围配置
    "from": "now-6h",
    "to": "now"
  },
  "editable": false,          // 不可编辑
  "timepicker": {             // 时间选择器配置
    "hidden": true,           // 隐藏时间选择器
    "refresh_intervals": ["5s", "10s", "30s", "1m", "5m", "15m", "30m", "1h", "2h", "1d"],
    "type": "timepicker"
  },
  "timezone": "browser",      // 使用浏览器时区
  "title": "Home"             // 仪表板标题
}
```