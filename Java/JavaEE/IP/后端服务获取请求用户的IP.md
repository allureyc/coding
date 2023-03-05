# [服务器如何获取请求 IP](https://www.jianshu.com/p/23ce44445e75?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

**转载于：**
 [叉叉哥](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fxiao__gui)
 [Jetty/Tomcat + Nginx反向代理获取客户端真实IP、域名、协议、端口](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fxiao__gui%2Farticle%2Fdetails%2F73733797)

[利用X-Forwarded-For伪造客户端IP漏洞成因及防范](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fxiao__gui%2Farticle%2Fdetails%2F83054462)

**1. IP地址问题**

> 客户端的请求经过服务器Nginx反向代理后，RemoteAddr地址为Nginx的地址。

Nginx反向代理后，Servlet应用通过**`request.getRemoteAddr()`**取到的IP是Nginx的IP地址，并非客户端真实的IP，通过**`request.getRequestURL()`**获取的域名、协议、端口都是Nginx访问Web应用时域名、协议、端口，而非客户端浏览器地址栏上的真实域名、协议、端口。

例如在某一台IP为10.4.64.22的服务器上，Jetty或者tomcat端口号为8080，Nginx端口号80，Nginx反向代理8080端口。



```bash
server {
    listen 80;
    location / {
        proxy_pass http://127.0.0.1:8080; # 反向代理应用服务器HTTP地址
    }
}
```

在另一台机器上浏览器打开[http://10.4.64.22/test](https://links.jianshu.com/go?to=http%3A%2F%2F10.4.64.22%2Ftest)访问某个Servlet应用，获取客户端IP和URL：



```css
System.out.println("RemoteAddr: " + request.getRemoteAddr());
System.out.println("URL: " + request.getRequestURL().toString());
```

结果为：



```bash
RemoteAddr: 127.0.0.1
URL: http://127.0.0.1:8080/test
```

可以发现，Servlet程序获取到的客户端IP是Nginx的IP而非浏览器所在机器IP，获取到的URL是Nginx proxy_pass配置的URL组成的地址，而非浏览器地址上的真实地址。如果将Nginx用作https服务器反向代理后端的http服务，那么**`request.getRequestURL()`**获取的URL是http前缀，而不是https前缀。无法获取到浏览器地址栏的真实地址。
 如果此时将**`request.getRequestURL()`**获取到的URL用作拼接Redirect地址，就会出现跳转到错误的地址，这也是Nginx反向代理时经常出现的一个问题。

**2. 问题出现的原因**

Nginx的反向代理实际上是客户端和真实应用服务器之间的一个桥梁，客户端（一般是浏览器）访问Nginx服务器，Nginx再去访问Web服务器。对于Web应用来说，这次HTTP请求的客户端是Nginx而非真实的客户端浏览器，如果不加特殊处理的话，Web应用会把Nginx当做请求的客户端，获取到的客户端信息就是Nginx的信息。

**3. 解决方案**

可以通过以下两个方面解决：

1. 由于Nginx是代理服务器，所有的客户端请求都从Nginx转发到Jetty/Tomcat，需要Nginx配置一些HTTPHeader来将这些信息告诉代理的Jetty/Tomcat。
2. Jetty/Tomcat这一端（服务端）要从Nginx传递的HTTP Header中获取客户端信息。

添加如下配置：

```bash
proxy_set_header Host $http_host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
```

以上配置就是在Nginx反向代理的时候，添加一些请求Header。

1. Host：包含客户端真实的域名和端口号。
2. X-Forwarded-Proto：表示客户端真实的协议（http还是https）。
3. X-Real-IP：表示客户端真实的IP
4. X-Forwarded-For：这个Header和X-Real-IP类似，但是它在多层代理时会包含真实客户端及中间每个代理服务器的IP。

在试一下request.getRemoteAddr()和request.getRequestURL()的输出结果。



```bash
RemoteAddr: 127.0.0.1
URL: http://10.4.64.22/test
```

1. RemoteAddr获取到的地址还是Nginx地址而非真实客户端IP。
2. 浏览器地址栏是https前缀，但是request.getRequestURL()获取到的URL还是http前缀。也就是仅仅配置Nginx还不能彻底解决问题。

> **1. 获取X-Forwarded-For或者X-Real-IP**

```kotlin
 public String getIp(HttpServletRequest request) throws Exception {
        String ip = request.getHeader("X-Forwarded-For");
        if (ip != null){
            if (!ip.isEmpty() && !"unKnown".equalsIgnoreCase(ip)) {
                int index = ip.indexOf(",");
                if (index != -1) {
                    return ip.substring(0, index);
                } else {
                    return ip;
                }
            }
        }
        ip = request.getHeader("X-Real-IP");
        if (ip != null) {
            if (!ip.isEmpty() && !"unKnown".equalsIgnoreCase(ip)) {
                return ip;
            }
        }
        ip = request.getHeader("Proxy-Client-IP");
        if (ip != null) {
            if (!ip.isEmpty() && !"unKnown".equalsIgnoreCase(ip)) {
                return ip;
            }
        }
        ip = request.getHeader("WL-Proxy-Client-IP");
        if (ip != null) {
            if (!ip.isEmpty() && !"unKnown".equalsIgnoreCase(ip)) {
                return ip;
            }
        }
        ip =  request.getRemoteAddr();
        return ip.equals("0:0:0:0:0:0:0:1") ? "127.0.0.1" : ip;
    }
```

**请求头的意思：**

1. X-Forwarded-For：
    只有通过HTTP代理或者负载均衡（比如Nginx）才会添加该项，格式为**`X-Forwarded-For:client1,proxy1,proxy2`**，一般情况下，第一个ip为客户端真实的IP，后面的为经过的代理服务器的IP。
2. Proxy-Client-IP/WL- Proxy-Client-IP
    这个一般是经过apache http服务器的请求才有，用apache http做代理时一般加上Proxy-Client-Ip请求头，而WL-Proxy-Client-IP是他的weblogic插件加上的头。
3. X-Real-IP
    nginx代理一般会加上此请求头。

**`注：针对前后端分离的项目， request.getRemoteAddr();获取的是客户的真实IP，而非前端IP。因为用户请求前端服务器，前端将HTML和JS代码渲染到本地，本地再去调用服务端。`**

> **Jetty**

在Jetty服务器的jetty.xml文件中，找到httpConfig，加入配置：



```jsx
<New id="httpConfig" class="org.eclipse.jetty.server.HttpConfiguration">

    ...

  <Call name="addCustomizer">
    <Arg><New class="org.eclipse.jetty.server.ForwardedRequestCustomizer"/></Arg>
  </Call>
</New>
```

重新启动Jetty，再用浏览器打开[http://10.4.64.22/test](https://links.jianshu.com/go?to=http%3A%2F%2F10.4.64.22%2Ftest)测试，结果：



```bash
RemoteAddr: 10.1.3.7
URL: http://10.4.64.22/test
```

此时可发现**`request.getRemoteAddr()`**获取到的IP不再是**`127.0.0.1`**，而是客户端真实IP。**`request.getRequestURL()`**获取的URL也是浏览器上真实URL，如果Nginx作为https代理，**`request.getRequestURL()`**的前缀也会是https。

> **Tomcat**

和Jetty类似，如果使用Tomcat作为应用服务器。也可以通过配置Tomcat的server.xml文件，在Host元素内最后加入：



```xml
<Valve className="org.apache.catalina.valves.RemoteIpValve" />
```

**4. 伪造X-Forwarded-For**

一般客户端（例如浏览器）发送HTTP请求是没有**`X-Forwarded-For`**头，当请求到达第一个代理服务器时，代理服务器会加上**`X-Forwarded-For`**请求头，并将值设为客户端的IP地址（也就是最左边的第一个值），后面如果还有多个代理，会依次将IP追加到**`X-Forwarded-For`**头最右边，最终请求到达Web服务器，应用通过获取**`X-Forwarded-For`**头取左边第一个IP即为客户端真实IP。

正如上面所说，X-Forwarded-For只是追加地址，就会给伪造IP可乘之机。

**但是如果客户端在发起请求时，请求头上带上一个伪造的X-Forwarded-For，由于后续每层代理只会追加不会覆盖，那么最终到达服务器时，获取到的左边第一个IP地址将会是客户端伪造的IP。也就是上面Java代码中getClientIp()方法获取到的IP地址很可能是伪造的IP地址。**

伪造X-Forwarded-For头的方法很简单，例如Postman就可以轻松做到：

![img](https:////upload-images.jianshu.io/upload_images/16013479-2e469c8b1ffb221d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

Postman伪造X-Forwarded-For

**5. 如何防范**

方法一：在直接对外的Nginx反向代理服务器上配置

```bash
proxy_set_header X-Forwarded-For $remote_addr;
```

若不是追加，我们拿到客户端真实IP覆盖呢？

这里使用$remote_addr替代上面的$proxy_add_x_forwarded_for。$proxy_add_x_forwarded_for会在原有X-Forwarded-For上追加IP，这就相当于给了伪造X-Forwarded-For的机会。而$remote_addr是获取的是直接TCP连接的客户端IP（类似于Java中的request.getRemoteAddr()），这个是无法伪造的，即使客户端伪造也会被覆盖掉，而不是追加。

需要注意的是，如果有多层代理，那么只要在直接对外访问的Nginx上配置X-Forwarded-For为$remote_addr，内部层的Nginx还是要配置为$proxy_add_x_forwarded_for，不然内部层的Nginx又会覆盖掉客户端的真实IP。

方法二：遍历X-Forwarded-For

实现思路：遍历X-Forwarded-For头中的IP地址，和上面方法不同的是，不是直接取左边第一个IP，而是从右向左遍历。遍历时可以根据正则表达式剔除掉内网IP和已知的代理服务器本身的IP（例如192.168开头的），那么拿到的第一个非剔除IP就会是一个可信任的客户端IP。这种方法的巧妙之处在于，即时伪造X-Forwarded-For，那么请求到达应用服务器时，伪造的IP也会在X-Forwarded-For值的左边，从右向左遍历就可以避免取到这些伪造的IP地址。