# 项目说明

---

### **1. 核心配置目录 (`config/`)**
| 文件/目录             | 作用说明                                                                 |
|----------------------|------------------------------------------------------------------------|
| [alias.txt](file:///Users/jinchen/Documents/github/jc-iptv-api/config/alias.txt)          | 频道别名映射表（例：`CCTV-1` → `中央一套`）                                |
| [blacklist.txt](file:///Users/jinchen/Documents/github/jc-iptv-api/config/blacklist.txt)      | 黑名单过滤规则（屏蔽特定接口/域名）                                         |
| [whitelist.txt](file:///Users/jinchen/Documents/github/jc-iptv-api/config/whitelist.txt)      | 白名单过滤规则（仅保留指定接口/域名）                                       |
| [config.ini](file:///Users/jinchen/Documents/github/jc-iptv-api/config/config.ini)         | **主配置文件**（全局参数，如 [open_rtmp](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/config.py#L354-L355), [speed_test_timeout](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/speed.py#L20-L20) 等）          |
| [user_config.ini](file:///Users/jinchen/Documents/github/jc-iptv-api/config/user_config.ini)    | 用户自定义配置（覆盖 [config.ini](file:///Users/jinchen/Documents/github/jc-iptv-api/config/config.ini)，避免更新时被覆盖）                         |
| `rtp/`               | 地域化直播源模板（按省份+运营商分类，如 `上海_电信.txt`）                      |
| [epg.txt](file:///Users/jinchen/Documents/github/jc-iptv-api/config/epg.txt)            | EPG数据源配置（节目预告接口地址）                                           |
| [subscribe.txt](file:///Users/jinchen/Documents/github/jc-iptv-api/config/subscribe.txt)      | 订阅源列表（集成开源项目如 `iptv-org/iptv` 的订阅地址）                      |

---

### **2. 输出结果目录 (`output/`)**
| 文件/目录             | 作用说明                                                                 |
|----------------------|------------------------------------------------------------------------|
| [result.txt](file:///Users/jinchen/Documents/github/jc-iptv-api/output/result.txt)         | **默认文本结果**（按模板生成的直播源列表）                                  |
| [result.m3u](file:///Users/jinchen/Documents/github/jc-iptv-api/output/result.m3u)         | 带图标M3U播放列表（支持播放器直接加载）                                     |
| [ipv4/result.txt](file:///Users/jinchen/Documents/github/jc-iptv-api/output/ipv4/result.txt)    | IPv4专用结果文件                                                       |
| [ipv6/result.txt](file:///Users/jinchen/Documents/github/jc-iptv-api/output/ipv6/result.txt)    | IPv6专用结果文件                                                       |
| [epg/epg.xml](file:///Users/jinchen/Documents/github/jc-iptv-api/output/epg/epg.xml)        | EPG节目预告数据（需开启 [open_epg](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/config.py#L362-L363)）                                      |
| [user_result.txt](file:///Users/jinchen/Documents/github/jc-iptv-api/output/user_result.txt)    | 用户自定义模板生成结果（优先级高于默认结果）                                 |

---

### **3. 服务与入口 (`service/`, [main.py](file:///Users/jinchen/Documents/github/jc-iptv-api/main.py))**
| 文件/目录             | 作用说明                                                                 |
|----------------------|------------------------------------------------------------------------|
| [main.py](file:///Users/jinchen/Documents/github/jc-iptv-api/main.py)            | **主程序入口**（执行更新流程）                                            |
| [service/app.py](file:///Users/jinchen/Documents/github/jc-iptv-api/service/app.py)     | Web服务核心（提供 [/](file:///Users/jinchen/Documents/github/jc-iptv-api/config/config.ini), `/m3u`, `/txt` 等接口）                            |
| [Dockerfile](file:///Users/jinchen/Documents/github/jc-iptv-api/Dockerfile)         | Docker镜像构建配置                                                      |
| [entrypoint.sh](file:///Users/jinchen/Documents/github/jc-iptv-api/entrypoint.sh)      | Docker容器启动脚本                                                      |
| [nginx.conf](file:///Users/jinchen/Documents/github/jc-iptv-api/nginx.conf)         | RTMP推流服务配置（需配合 [open_rtmp](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/config.py#L354-L355) 使用）                               |

---

### **4. 功能模块 (`updates/`, `utils/`)**
#### **4.1 数据源更新模块 (`updates/`)**
| 子模块                | 作用说明                                                                 |
|----------------------|------------------------------------------------------------------------|
| `hotel/`             | 酒店源抓取（通过 `Foodie/FOFA` 爬取酒店公共IPTV）                          |
| `multicast/`         | 组播源抓取（解析组播网络中的直播流）                                       |
| `subscribe/`         | 订阅源管理（拉取 `iptv-org/iptv` 等开源项目数据）                          |
| `online_search/`     | 关键字搜索源（主动搜索网络直播接口）                                        |
| `epg/`               | EPG数据获取（解析节目预告信息）                                           |
| `proxy/`             | 代理请求工具（绕过网络限制）                                              |

#### **4.2 工具库 (`utils/`)**
| 文件                 | 作用说明                                                                 |
|----------------------|------------------------------------------------------------------------|
| [config.py](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/config.py)          | 配置解析与管理（读取 [config.ini](file:///Users/jinchen/Documents/github/jc-iptv-api/config/config.ini) 和 [user_config.ini](file:///Users/jinchen/Documents/github/jc-iptv-api/config/user_config.ini)）                 |
| [speed.py](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/speed.py)           | 测速引擎核心（验证接口延迟、速率、分辨率）                                  |
| [channel.py](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/channel.py)         | 频道处理逻辑（别名映射、黑白名单过滤）                                     |
| [ip_checker.py](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/ip_checker/ip_checker.py)      | IPv4/IPv6网络检测                                                       |
| [retry.py](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/retry.py)           | 网络请求重试机制                                                        |
| [driver/tools.py](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/driver/tools.py)    | 浏览器自动化工具（需开启 [open_driver](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/config.py#L292-L295) 时使用）                            |
| `nginx-rtmp-win32/`  | Windows版Nginx-RTMP服务（支持本地推流）                                   |

---

### **5. GUI界面 (`tkinter_ui/`)**
| 文件                 | 作用说明                                                                 |
|----------------------|------------------------------------------------------------------------|
| [tkinter_ui.py](file:///Users/jinchen/Documents/github/jc-iptv-api/tkinter_ui/tkinter_ui.py)      | **GUI主程序**（启动图形化界面）                                           |
| [prefer.py](file:///Users/jinchen/Documents/github/jc-iptv-api/tkinter_ui/prefer.py)          | 偏好设置面板（配置 [open_rtmp](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/config.py#L354-L355), [min_speed](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/config.py#L159-L160) 等参数）                      |
| [epg.py](file:///Users/jinchen/Documents/github/jc-iptv-api/tkinter_ui/epg.py)             | EPG管理面板                                                            |
| [speed.py](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/speed.py)           | 测速参数配置面板                                                        |
| [select_combobox.py](file:///Users/jinchen/Documents/github/jc-iptv-api/tkinter_ui/select_combobox.py) | 下拉选择框组件（如地区、运营商选择）                                      |

---

### **6. 辅助文件与资源**
| 文件                 | 作用说明                                                                 |
|----------------------|------------------------------------------------------------------------|
| [source.json](file:///Users/jinchen/Documents/github/jc-iptv-api/source.json)        | 点播源结果文件（支持JSON格式播放列表）                                     |
| [CHANGELOG.md](file:///Users/jinchen/Documents/github/jc-iptv-api/CHANGELOG.md)       | 版本更新日志                                                            |
| [README.md](file:///Users/jinchen/Documents/github/jc-iptv-api/README.md)          | 项目说明文档（含快速上手指南）                                            |
| [version.json](file:///Users/jinchen/Documents/github/jc-iptv-api/version.json)       | 版本号与构建信息                                                        |
| `static/`            | 静态资源（Logo、赞赏二维码等）                                           |

---

### **关键设计特点**
1. **模块化架构**
    - 数据源（`updates/`）、工具库（`utils/`）、服务层（`service/`）严格分离，便于扩展新数据源。
    - 通过 `open_*` 配置项动态开关功能模块（如 [open_hotel](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/config.py#L207-L208), [open_rtmp](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/config.py#L354-L355)）。

2. **配置优先级**
    - 用户配置（[user_config.ini](file:///Users/jinchen/Documents/github/jc-iptv-api/config/user_config.ini)） > 默认配置（[config.ini](file:///Users/jinchen/Documents/github/jc-iptv-api/config/config.ini)） > 代码硬编码默认值。

3. **结果生成逻辑**
    - 模板驱动：通过 [source_file](file:///Users/jinchen/Documents/github/jc-iptv-api/utils/config.py#L191-L192)（默认 [config/demo.txt](file:///Users/jinchen/Documents/github/jc-iptv-api/config/demo.txt)）定义输出格式。
    - 多协议支持：同时生成 `txt`/`m3u` 格式，区分 `ipv4`/`ipv6` 结果。

4. **运行模式**
    - 命令行：`pipenv run dev`（更新） + `pipenv run service`（启动服务）
    - GUI：`pipenv run ui`（图形化操作）
    - Docker：容器化部署（支持 `amd64/arm64`）

> 提示：如需自定义功能，建议优先修改 `config/` 下的配置文件，而非直接修改代码逻辑。  
> 详细参数说明可参考 [README.md#配置](file:///Users/jinchen/Documents/github/jc-iptv-api/README.md#配置)。