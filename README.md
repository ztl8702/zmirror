# zmirror
[![version](https://img.shields.io/badge/version-0.29.0-blue.svg)](https://github.com/aploium/zmirror)
[![Build Status](https://travis-ci.org/aploium/zmirror.svg?branch=master)](https://travis-ci.org/aploium/zmirror)
[![codecov](https://codecov.io/gh/aploium/zmirror/branch/master/graph/badge.svg)](https://codecov.io/gh/aploium/zmirror)
[![Dependency Status](https://www.versioneye.com/user/projects/57addd5358ae9200345e108c/badge.svg?style=flat-square)](https://www.versioneye.com/user/projects/57addd5358ae9200345e108c)
[![Gitter](https://badges.gitter.im/zmirror/zmirror.svg)](https://gitter.im/zmirror/zmirror?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)  
  
an http reverse proxy designed to automatically and completely mirror a website (such as google), support cache and CDN  
一个Python反向HTTP代理程序, 用于快速、简单地创建别的网站的镜像, 自带本地文件缓存、CDN支持  
比如国内可以访问的Google镜像/中文维基镜像 请看`more_configs`文件夹下的配置文件  

程序附了几个配置文件:  Google镜像(含学术/其他/中文维基) twitter镜像 Youtube镜像 instagram镜像  
  
## Demo
* **Google** 
    * *PC & Mobile*   https://g.zmirrordemo.com  
        google demo站静态资源使用了CDN, 请使用浏览器的开发者工具查看资源加载情况  
        当一项静态资源(js/css/图片等)被第二次访问时, 会从CDN中获取  
        其余demo未启用CDN  
    * *Scholar*   https://g.zmirrordemo.com/scholar  
    * *Image*   https://g.zmirrordemo.com/imghp  
    * *中文维基*  https://g.zmirrordemo.com/wiki  

* **Youtube**  
    * *PC Only*  https://ytb-pc.zmirrordemo.com  
    * *Mobile Only* https://ytb-mobile.zmirrordemo.com
        Youtube Mobile 不支持iOS
* **Twitter**
    * *PC Only*  https://t-pc.zmirrordemo.com  
    * *Mobile Only*  https://t-mobile.zmirrordemo.com
* **Instagram**
    * *PC & Mobile*  https://in.zmirrordemo.com  
* **Facebook**
    * *PC Only*  https://fb.zmirrordemo.com  
        功能不完整, 不支持手机.  最大的问题是当地址栏链接改变时不会自动刷新, 需要手动刷新  
        未来会写一个脚本, 当url改变后刷新页面  

## Screenshot
Screenshots are here: [wiki-screenshots](https://github.com/aploium/zmirror/wiki/Screenshots)  

## 一键部署脚本
https://github.com/aploium/zmirror-onekey  

## Out-of-box configs
Together with the program, provided several (almost) out-of-box configs  

* **Google镜像** (整合**中文维基镜像**)
    * 同时支持PC/手机
    * google搜索与中文维基百科无缝结合
    * 大部分功能完全正常: google网页搜索/学术/图片/新闻/图书/视频(搜索)/财经/APP搜索/翻译/网页快照/...
    * 以下服务部分可用: gg地图(地图可看, 左边栏显示不正常)/G+(不能登录)
    * 目前暂时无法做到完美的登陆, 登录才可使用的功能无效
    * 不会被Google Ban掉  
        * 传统的Nginx反代镜像方案, 时间长了会被Google Ban掉, 或者弹图片验证码,   
            这是由于Nginx反代镜像非常简陋, 用户的许多请求无法被正确发回到Google服务器,  
            Google就会把真实的访问者当成是机器人.  
            
        * 而zmirror比较完善, 用户的请求能全部发回到Google服务器, 不会被当成机器人  

* **twitter镜像**
    * 支持PC站/手机  (两者需要以不同的域名部署, 详见配置)  
    * 几乎所有功能完整可用, 大部分视频可以播放  
* **instagram镜像**  
    * 所有功能完整可用, 包括视频  
* **Youtube镜像**  
    * 支持PC站/手机  (两者需要以不同的域名部署, 详见配置)
    * 视频播放、高清支持
    * 登陆支持、字幕支持
    * 小视频上传支持

## Requirements Install and Usage

### Requirements
* python 3.4+
* flask
* request

Theoretically, any environment that can run python3.4+, can also run zmirror  
Nginx was not officially tested, but it should work pretty well.  

However, due to my limited time, zmirror was only fully tested in:  

    Ubuntu14.04-x86_64 Apache2.4 wsgi python3.4
    Ubuntu16.04-x86_64 Apache2.4 wsgi python3.5
    windows10-x64 Apache2.4 wsgi python3.5-x64
    
    Ubuntu14.04-x86_64 directly run (I mean, just execute python3 wsgi.py)
    windows10-x64 directly run 


### Installation and helloworld
> This tutorial is mainly for your *localhost* demo test  
 If you want to deploy it to server, please complete the *localhost* demo first  

1. first, install python3  
    **Debian/Ubuntu**  `apt-get install python3`  
    **Windows**   go to [python's homepage](https://www.python.org/downloads/) and download python3.5 (or newer)  
2. install or upgrade flask and requests `python3 -m pip install -U flask requests`  
3. (recommended) `git clone https://github.com/aploium/zmirror` or download and unzip this package(not recommend).  
4. **copy** the `config_default.py` to `config.py`  

    > **Warning: You should NEVER EVER modify the `config_default.py` itself**  
    > Please edit the `config.py` instead of `config_default.py`  
    > Unless your are developer.  
    > Settings in the `config.py` would override the default ones  

5. Execute it: `python3 wsgi.py`  
6. Open your browser and enter `http://127.0.0.1/`, you will see exactly the `www.kernel.org`, and you can click and browse around. everything of the `*.kernel.org` is withing the mirror.  
7. please see the following [Setup an actual mirror] section  

#### Deploy an actual mirror

请使用: [一键部署脚本](https://github.com/aploium/zmirror-onekey)  

若希望手工部署, 可以看以下教程:  

1. [部署支持HTTPS和HTTP/2的zmirror镜像](https://github.com/aploium/zmirror/wiki/%E9%83%A8%E7%BD%B2%E6%94%AF%E6%8C%81HTTPS%E5%92%8CHTTP2.0%E7%9A%84%E9%95%9C%E5%83%8F)  
2. [在一台VPS部署多个zmirror镜像](https://github.com/aploium/zmirror/wiki/%E5%9C%A8%E4%B8%80%E5%8F%B0VPS%E9%83%A8%E7%BD%B2%E5%A4%9A%E4%B8%AAzmirror%E9%95%9C%E5%83%8F)  


### Upgrade
 - (for users of git) `cd YOUR_ZMIRROR_FOLDER` and `git pull`
 - (for users of plain zip download) re-download, unzip, and override all files

    > **警告**  
    > 由于 v0.27 有很大的结构改动, 所以 v0.27 以内的 custom_func.py 如果有 `from zmirror import ` 语句  
    > 将无法在 v0.27 以后的版本工作  
    > 解决办法是将 custom_func.py 中的 `from zmirror import ` 修改为  
    > `from zmirror.zmirror import ` 其他不需要改变  
    > 若使用自带配置文件, 则只有Youtube和Twitter受影响  


## Feature
 1. Completely mirror, provide some (almost) out-of-box configs  
  创建非常完整的镜像, 既支持古老的网站(比如内网网站), 也支持巨型的现代化的网站  
  提供几个(几乎)开箱即用的网站镜像配置文件  

 2. Mirror ANY website, highly compatible  
    非常高的兼容性和通用性, 可以镜像 _任意_ 网站, 而不只是Google/Wiki/twitter/instagram, 而且功能都非常完整  
    并且能很好地适应对现代化的、逻辑复杂、功能庞大的网站  
    _现在还在开发阶段, 虽然所有网站的绝大部分功能都可以开箱即用, 但是某些网站的某些功能仍然不完整, 正在不断改进_  
  
 3. (MIME-based) Local statistic file cache support (especially useful if we have low bandwidth or high latency)  
  (基于MIME)本地静态文件缓存支持(当镜像服务器与被镜像服务器之间带宽很小或延迟很大时非常有用)  
  
 4. CDN Support, hot statistic resource can serve by CDN, dramatically increase speed  
  CDN支持. 让热门静态资源走CDN, 极大提高用户访问速度(特别是使用国内CDN, 而镜像服务器在国外时)  
  
 5. Easy to config and deploy, highly automatic  
  非常容易配置和部署, 镜像一个网站只需要添加它的域名即可  
  
 6. Access control(IP, user-agent), visitor verification(question answer, or custom verification function)  
  访问控制(IP, user-agent)与用户验证(回答问题, 也支持写自定义的验证函数)  
  
 7. Automatically rewrite JSON/javascript/html/css, even dynamically generated url can ALSO be handled correctly  
  自动重写JSON/javascript/html/css中链接, 甚至即使是动态生成的链接, 都能被正确处理  
  
 8. Stream content support (audio/video)  
  流媒体支持(视频/音频)  
  
 9. Production ready.  
    程序已经经受住了生产环境的考验  
    
        使用的服务器均为 256M OpenVZ VPS
        Google:  
            单台服务器
            日6kPV, 峰值每小时740PV  
            峰值时段CPU占用小于10%  
        Youtube:  
            1台主服务器+8台视频服务器  
            日1wPV, 峰值每小时754PV  
            日发送流量178GB  
            高峰时段1080P流畅  

## Issues Report

欢迎发issues, 发issues找我聊天都欢迎.  

(以下只是可选步骤)  
如果遇到Bug需要发issues, 请在`config.py`最下面加上这样两句话, 然后重现一遍问题  
```python
developer_dump_all_traffics = True  
verbose_level = 4
```
这样程序会把所有流量dump到程序所在目录的`traffic`文件夹下, 发issues时请将所有程序log和所有dump文件打包发上来, 帮助我debug  

对于Apache(教程部署的即为Apache), 程序的日志在 `/var/log/apache2/你自定义的日志文件名.log` 中, 请将它也一并发上来  
  
_以下部分需要重写, 写的很乱_
  
## Mirror A Website
Mirror a website is very simple.  

Just set the `target_domain` to it's domain, the `external_domains` to it's external domain and sub domains 
such as static resource domains (If it has)  
save and run, the program will do the other works!   

All detects and rewrites are completely AUTOMATICALLY  

`tips:` you can find a website's external domains by using the developer tools of your browser, it will log all network traffics for you  

## Performance Enhance
### Local Cache
  Local file cache (along with 304 support) is implanted and enabled by default  
  If cache hits, you will see an `x-zmirror-cache: FileHit` in the response header.  
  Local cache will be deleted and cleaned once program exit.  
  
### CDN Support
  please see [使用七牛作为zmirror镜像的CDN](https://github.com/aploium/zmirror/wiki/%E4%BD%BF%E7%94%A8%E4%B8%83%E7%89%9B%E4%BD%9C%E4%B8%BAzmirror%E9%95%9C%E5%83%8F%E7%9A%84CDN)  
  

## Deploy To Server
请使用[一键部署脚本](https://github.com/aploium/zmirror-onekey)  
手动部署可以看 [部署支持HTTPS和HTTP/2的zmirror镜像](https://github.com/aploium/zmirror/wiki/%E9%83%A8%E7%BD%B2%E6%94%AF%E6%8C%81HTTPS%E5%92%8CHTTP2.0%E7%9A%84%E9%95%9C%E5%83%8F)  
  
  
Or, if you are family with flask, you can see flask's official deploy tutorial:  
    [http://flask.pocoo.org/docs/0.11/deploying/](http://flask.pocoo.org/docs/0.11/deploying/)  

## Similar Projects

Well, zmirror is (currently) better than them.  

@zxq2233 [youtube-php-mirroring](https://github.com/zxq2233/youtube-php-mirroring)  
@greatfire [website-mirror-by-proxy](https://github.com/greatfire/website-mirror-by-proxy)  
@restran [web-proxy](https://github.com/restran/web-proxy)  
@isayme [isayme/google](https://github.com/isayme/google)  
@zjuyxy [google200](https://github.com/zjuyxy/google200)  
@cuber [ngx_http_google_filter_module](https://github.com/cuber/ngx_http_google_filter_module)  
