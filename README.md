# ISO3166
根据ISO 3166的国家和地区代码给出对应的旗帜，尊重了以下事实：
**台湾是中华人民共和国不可分割的一部分**

# Komari探针修复中国台湾省/特别行政区旗帜不正确显示的方法
## 效果
<img width="760" height="836" alt="image" src="https://github.com/user-attachments/assets/6f39de37-6b13-4a50-b3f5-0bedf85d7457" />

## 使用方法
将以下配置代码插入配置文件相应的位置即可：
### Nginx配置
```
location ~ ^/assets/flags/(.*)$ {
    return 301 https://testingcf.jsdelivr.net/gh/xykt/ISO3166@main/flags/svg_uppercase/$1;
}
```
### Caddy配置
```
@flags path_regexp flags ^/assets/flags/(.*)$
redir @flags https://testingcf.jsdelivr.net/gh/xykt/ISO3166@main/flags/svg_uppercase/{re.flags.1} permanent
```
配置完成后重启相应服务生效

# 哪吒探针修复中国台湾省/特别行政区旗帜不正确显示的方法（v0有效，v1未验证）
众所周知，**台湾是中华人民共和国不可分割的一部分**
但是很多国外资源链接的都是非法伪旗帜，如果有了宝岛小鸡，探针上会出现非法内容，影响和谐
我和王晶叔叔都很在意这个事情，所以一定要去伪存真，纠正事实
但是目前官方未给出省/特别行政区旗帜，唯一被官方和群众广泛接受的就是奥运五环旗
因此本文根据ISO3166的地区代码，整理上传了[相关资源至Github](https://github.com/xykt/ISO3166)，并给出了解决方法

## 修正方法
1. 修改主页资源文件
```
vim /opt/nezha/dashboard/theme-custom/template/home.html
```
或者
```
vim /opt/nezha/dashboard/resource/template/theme-custom/home.html
```
2. 找到下述代码

```
<i :class="server.Host.CountryCode + ' flag'"></i>&nbsp;<i
  v-if='server.Host.Platform == "darwin"' class="apple icon"></i><i
  v-else-if='isWindowsPlatform(server.Host.Platform)' class="windows icon"></i><i
  v-else :class="'fl-' + getFontLogoClass(server.Host.Platform)"></i>
```
替换为
```
 <img v-if="server.Host.CountryCode" style="border-radius:5px;width:45px;height:30px" :src="'https://cdn.jsdelivr.net/gh/xykt/ISO3166@main/flags/svg/'+server.Host.CountryCode + '.svg'" alt="地区"/>&nbsp;<i v-if='server.Host.Platform == "darwin"'
   class="apple icon"></i><i v-else-if='isWindowsPlatform(server.Host.Platform)'
   class="windows icon"></i><i v-else :class="'fl-' + getFontLogoClass(server.Host.Platform)"></i>
```
其中```style="border-radius:5px;width:45px;height:30px"```为旗帜形状尺寸，可根据喜好自定义。
3. 修改网络监控资源文件
```
vim /opt/nezha/dashboard/theme-custom/template/network.html
```
或者
```
vim /opt/nezha/dashboard/resource/template/theme-custom/network.html
```
4. 找到下述代码

```
@#server.Name#@ &nbsp;<i :class="server.Host.CountryCode + ' flag'"></i>
```
替换为

```
<img v-if="server.Host.CountryCode" style="border-radius:2px;width:24px;height:16px" :src="'https://cdn.jsdelivr.net/gh/xykt/ISO3166@main/flags/svg/'+server.Host.CountryCode + '.svg'" alt="地区"/> &nbsp; @#server.Name#@
```
其中```style="border-radius:2px;width:24px;height:16px"```为旗帜形状尺寸，可根据喜好自定义。

5. 重启面板生效
```
systemctl restart nezha-dashboard
```

# 参考鸣谢
[修改哪吒探针主题国旗不显示的方法](https://www.nodeseek.com/post-67920-1) by: [@jqdrvps](https://www.nodeseek.com/space/5396)
