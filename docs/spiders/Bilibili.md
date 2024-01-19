# 逆向B站刷播放量

## 找到刷播放量的请求URL

-.js -.css -.png -.svg -.html -data -Ha 首先在过滤器里面把静态资源相关的先过滤了

找到请求URL为：https://api.bilibili.com/x/click-interface/click/now。但是这个是GET，一看这个应该是不是加播放量的

https://api.bilibili.com/x/click-interface/click/web/h5，最后找到这个
观察url没有需要过逆向的，接下来就是看这些参数，哪些是需要逆向的。（一般加上id、auth这种的）

## 明确需要逆向的参数

### 查询字符串参数

**w_aid**: 325318514        // 可能需要逆向

w_part: 1               // 一般不需逆向

w_ftime: 1703944675     // 时间戳，

ftime是first_time，第一次播放

w_stime: 1703944674     // 时间戳，

stime是start_time，这个应该是进来的时间，因为比上面那个小

w_type: 3               // 这个应该是类型，一般不需逆向

**web_location**: 1315873   // 这个location，看着应该是标识地域这种东西，几乎不用逆向

**w_rid**: 156c3c9ccf38bbe3e32c2a8481540e07 // 这个需要逆向	088a19726ed0debc607fe1669b2a4b88

wts: 1703944676     // 这个也是时间戳，不知道是啥

### 请求体

aid: 325318514      // 跟上面那个aid一样

**cid**: 1381936481     // 应该需要逆向

part: 1             // 跟上面part一样

lv: 0               // 一般不需逆向

ftime: 1703944675   // 跟上面查询参数一样

stime: 1703944674   // 跟上面查询参数一样

type: 3             // 跟上面类型一样

sub_type: 0         // 发布类型，应该是视频投稿时候的类型

refer_url: 

outer: 0

**spmid**: 333.788.0.0  // 这个不知道是啥，应该需要逆向

from_spmid: 
    

**session**: 43b386d7ec6e006b96315d0a242ae6de   // 用户会话标识，必须逆向

csrf: 

### Cookie

// 下面这些cookie只有两种情况，一种是前面请求返回回来的，一种是前端生成的。一般不会在这里搞一些特别复杂的加密。

buvid3=961F977A-3B6C-A0B4-0084-31ECF47E0EBD72322infoc; 
    

b_nut=1703944672; 

CURRENT_FNVAL=4048; 
    

b_lsid=DC319B4B_18CBB045E54; 
    

_uuid=107BBB10BC-A2DF-D12E-5B44-10D35E10A7D3A774908infoc;
    

sid=8batyilc; 
    

buvid4=F20CC40F-8052-82EB-7114-6644F359C14273809-023123013-r%2Ft%2FxrhceKtUMRV2iYDcYg%3D%3D;
    buvid_fp=f700b2fa0217e916d769bf691fb41f92



整理完成了，真是个麻烦人的操作。但是这一步是必须要做的，不然你根本不知道要逆向哪些东西。接下来明确目标就行了，一点一点逆向。

1. **w_aid**

1. **web_location**
2. **w_rid**
3. **cid**
4. **spmid**
5. **session**

cookie是一种特殊的，你也不知道要不要逆向，这里都得弄清楚每个cookie的含义，但是这里逆向一般不难

1. buvid3
2. b_nut
3. b_lsid
4. _uuid
5. sid
6. buvid4
7. buvid_fp

## 逆向过程

### 查询字符串参数

**w_aid**

​	这个是从第一次请求中嵌套在html里面返回的。思路：先在请求里面搜索，发现都是携带的，然后直接全局搜这个的值，第一个请求就是。https://www.bilibili.com/video/BV1aw41157V3/

<hr>

**web_location**	

全局搜这个的值，这个是在一个js文件中给参数赋值的，也是服务端生成的，应该直接带着就行，可能B站服务端会拿着这个地域做一些风控限制。https://s1.hdslb.com/bfs/static/player/main/core.d98a5476.js

<hr>

**w_rid**

重头戏：

这是逆向过程中的参数

**数组不变和b不变**

 [46, 47, 18, 2, 53, 8, 23, 32, 15, 50, 10, 31, 58, 3, 45, 35, 27, 43, 5, 49, 33, 9, 42, 19, 29, 28, 14, 39, 12, 38, 41, 13, 37, 48, 7, 16, 24, 55, 40, 61, 26, 17, 0, 1, 60, 51, 30, 4, 22, 25, 54, 21, 56, 59, 6, 63, 57, 62, 11, 36, 20, 34, 44, 52]

[46, 47, 18, 2, 53, 8, 23, 32, 15, 50, 10, 31, 58, 3, 45, 35, 27, 43, 5, 49, 33, 9, 42, 19, 29, 28, 14, 39, 12, 38, 41, 13, 37, 48, 7, 16, 24, 55, 40, 61, 26, 17, 0, 1, 60, 51, 30, 4, 22, 25, 54, 21, 56, 59, 6, 63, 57, 62, 11, 36, 20, 34, 44, 52]

**n，其实是两个url处理后的字符，但是如果不登录，一直是固定的，没必要逆向了，但是我当时也是逆向了**

ea1db124af3c7062474693fa704f4ff8

ea1db124af3c7062474693fa704f4ff8

https://api.bilibili.com/x/web-interface/nav：请求这个url得到img_url和sub_url，经过算法得到n

```js
 function d(e) {
                if (e || window.__NanoStaticHttpKey) {
                    return "https://i0.hdslb.com/bfs/wbi/".concat("5a6f002d0bb14fc9848fc64157648ad4", ".png")
                }
            }
            
            
          function f(e) {
                    if (e || window.__NanoStaticHttpKey) {
                        return "https://i0.hdslb.com/bfs/wbi/".concat("0503a77b29d7409d9548fb44fe9daa1a", ".png")
                    }
                }
                
                
      
 "https://i0.hdslb.com/bfs/wbi/4932caff0ff746eab6f01bf08b70ac45.png"

// 就是先
// 跟ip深度绑定的

  "img_url": "https://i0.hdslb.com/bfs/wbi/7cd084941338484aae1ad9425b84077c.png",
            "sub_url": "https://i0.hdslb.com/bfs/wbi/4932caff0ff746eab6f01bf08b70ac45.png"
```



**加密字符串**

y:aid=325318514&cid=1381936481&web_location=1315873&wts=1703998861

n:ea1db124af3c7062474693fa704f4ff8

**n，其实是两个url经过一个小算法处理后的字符串，但是如果不登录，一直是固定的，没必要逆向了，但是我当时也是逆向了**

就是根据这个wts来生成不同的w_rid  wts生成的逻辑：Date.now() / 1e3

 ()(t && t.asBytes )? r : (t && t.asString) )  ? a.bytesToString(r) : n.bytesToHex(r)		&&运算符优先级大于?:，

因此这个运算结果是n.bytesToHex(r).

下面这个函数根据传进来的查询字符串和固定的值

思路是这样的，加密过程是拆分一下是这个，n.bytesToHex(n.wordsToBytes(s(123456)))

s(123456)生成的这样一个数组[1791092075, -946928880, 1505655784, 1414671436]。是md5加密，16字节分成4个32位的整数，**猜测就是先进行md5加密，在转换成字节，在把字节转换成16进制。直接复现，发现结果一样，逆向成功。**

下面是这个函数，看不懂吧，没事我也看不懂，这些n里面的方法看起来应该是一个现成的库的，假设B站没有往里面掺自己的东西，直接复现就好了

```js
     e.exports = function(e, t) {
                    if (null == e)
                        throw new Error("Illegal argument " + e);
                    var r = n.wordsToBytes(s(e, t));
                    return t && t.asBytes ? r : t && t.asString ? a.bytesToString(r) : n.bytesToHex(r)
      }




s = function e(t, r) {
                    t.constructor == String ? t = r && "binary" === r.encoding ? a.stringToBytes(t) : i.stringToBytes(t) : o(t) ? t = Array.prototype.slice.call(t, 0) : Array.isArray(t) || (t = t.toString());
                    for (var s = n.bytesToWords(t), u = 8 * t.length, c = 1732584193, l = -271733879, d = -1732584194, f = 271733878, p = 0; p < s.length; p++)
                        s[p] = 16711935 & (s[p] << 8 | s[p] >>> 24) | 4278255360 & (s[p] << 24 | s[p] >>> 8);
                    s[u >>> 5] |= 128 << u % 32,
                    s[14 + (u + 64 >>> 9 << 4)] = u;
                    var h = e._ff
                      , y = e._gg
                      , m = e._hh
                      , g = e._ii;
                    for (p = 0; p < s.length; p += 16) {
                        var v = c
                          , _ = l
                          , E = d
                          , b = f;
                        c = h(c, l, d, f, s[p + 0], 7, -680876936),
                        f = h(f, c, l, d, s[p + 1], 12, -389564586),
                        d = h(d, f, c, l, s[p + 2], 17, 606105819),
                        l = h(l, d, f, c, s[p + 3], 22, -1044525330),
                        c = h(c, l, d, f, s[p + 4], 7, -176418897),
                        f = h(f, c, l, d, s[p + 5], 12, 1200080426),
                        d = h(d, f, c, l, s[p + 6], 17, -1473231341),
                        l = h(l, d, f, c, s[p + 7], 22, -45705983),
                        c = h(c, l, d, f, s[p + 8], 7, 1770035416),
                        f = h(f, c, l, d, s[p + 9], 12, -1958414417),
                        d = h(d, f, c, l, s[p + 10], 17, -42063),
                        l = h(l, d, f, c, s[p + 11], 22, -1990404162),
                        c = h(c, l, d, f, s[p + 12], 7, 1804603682),
                        f = h(f, c, l, d, s[p + 13], 12, -40341101),
                        d = h(d, f, c, l, s[p + 14], 17, -1502002290),
                        c = y(c, l = h(l, d, f, c, s[p + 15], 22, 1236535329), d, f, s[p + 1], 5, -165796510),
                        f = y(f, c, l, d, s[p + 6], 9, -1069501632),
                        d = y(d, f, c, l, s[p + 11], 14, 643717713),
                        l = y(l, d, f, c, s[p + 0], 20, -373897302),
                        c = y(c, l, d, f, s[p + 5], 5, -701558691),
                        f = y(f, c, l, d, s[p + 10], 9, 38016083),
                        d = y(d, f, c, l, s[p + 15], 14, -660478335),
                        l = y(l, d, f, c, s[p + 4], 20, -405537848),
                        c = y(c, l, d, f, s[p + 9], 5, 568446438),
                        f = y(f, c, l, d, s[p + 14], 9, -1019803690),
                        d = y(d, f, c, l, s[p + 3], 14, -187363961),
                        l = y(l, d, f, c, s[p + 8], 20, 1163531501),
                        c = y(c, l, d, f, s[p + 13], 5, -1444681467),
                        f = y(f, c, l, d, s[p + 2], 9, -51403784),
                        d = y(d, f, c, l, s[p + 7], 14, 1735328473),
                        c = m(c, l = y(l, d, f, c, s[p + 12], 20, -1926607734), d, f, s[p + 5], 4, -378558),
                        f = m(f, c, l, d, s[p + 8], 11, -2022574463),
                        d = m(d, f, c, l, s[p + 11], 16, 1839030562),
                        l = m(l, d, f, c, s[p + 14], 23, -35309556),
                        c = m(c, l, d, f, s[p + 1], 4, -1530992060),
                        f = m(f, c, l, d, s[p + 4], 11, 1272893353),
                        d = m(d, f, c, l, s[p + 7], 16, -155497632),
                        l = m(l, d, f, c, s[p + 10], 23, -1094730640),
                        c = m(c, l, d, f, s[p + 13], 4, 681279174),
                        f = m(f, c, l, d, s[p + 0], 11, -358537222),
                        d = m(d, f, c, l, s[p + 3], 16, -722521979),
                        l = m(l, d, f, c, s[p + 6], 23, 76029189),
                        c = m(c, l, d, f, s[p + 9], 4, -640364487),
                        f = m(f, c, l, d, s[p + 12], 11, -421815835),
                        d = m(d, f, c, l, s[p + 15], 16, 530742520),
                        c = g(c, l = m(l, d, f, c, s[p + 2], 23, -995338651), d, f, s[p + 0], 6, -198630844),
                        f = g(f, c, l, d, s[p + 7], 10, 1126891415),
                        d = g(d, f, c, l, s[p + 14], 15, -1416354905),
                        l = g(l, d, f, c, s[p + 5], 21, -57434055),
                        c = g(c, l, d, f, s[p + 12], 6, 1700485571),
                        f = g(f, c, l, d, s[p + 3], 10, -1894986606),
                        d = g(d, f, c, l, s[p + 10], 15, -1051523),
                        l = g(l, d, f, c, s[p + 1], 21, -2054922799),
                        c = g(c, l, d, f, s[p + 8], 6, 1873313359),
                        f = g(f, c, l, d, s[p + 15], 10, -30611744),
                        d = g(d, f, c, l, s[p + 6], 15, -1560198380),
                        l = g(l, d, f, c, s[p + 13], 21, 1309151649),
                        c = g(c, l, d, f, s[p + 4], 6, -145523070),
                        f = g(f, c, l, d, s[p + 11], 10, -1120210379),
                        d = g(d, f, c, l, s[p + 2], 15, 718787259),
                        l = g(l, d, f, c, s[p + 9], 21, -343485551),
                        c = c + v >>> 0,
                        l = l + _ >>> 0,
                        d = d + E >>> 0,
                        f = f + b >>> 0
                    }
                    return n.endian([c, l, d, f])
                }
```



```python
import struct

def s(input_string):
    """
    计算字符串的 MD5 散列，并返回四个整数值组成的数组。
    """
    md5_hash = hashlib.md5(input_string.encode())
    # 将 16 字节的 MD5 哈希分成四个 32 位的整数
    return list(struct.unpack('>4I', md5_hash.digest()))

def words_to_bytes(words):
    """
    将整数数组转换为字节序列。
    """
    # 每个整数转换为 4 字节
    return [byte for word in words for byte in struct.pack('>I', word)]

def convert_md5(input_string, as_bytes=False, as_string=False):
    """
    根据选项返回 MD5 哈希的不同表示。
    """
    # 计算 MD5 整数数组
    md5_words = s(input_string)

    if as_bytes:
        # 返回字节序列
        return words_to_bytes(md5_words)
    elif as_string:
        # 返回字符串表示（假设是 UTF-8 编码的字符串）
        return ''.join(chr(byte) for byte in words_to_bytes(md5_words))
    else:
        # 返回十六进制表示
        return ''.join(f'{word:08x}' for word in md5_words)

# 测试字符串
test_string = 'aid=325318514&cid=1381936481&web_location=1315873&wts=1704001621ea1db124af3c7062474693fa704f4ff8'

# 测试函数
md5_as_bytes = convert_md5(test_string, as_bytes=True)
md5_as_string = convert_md5(test_string, as_string=True)
md5_as_hex = convert_md5(test_string)

md5_as_bytes, md5_as_string, md5_as_hex
```

<hr>

### 请求体

**cid**

跟w_aid一样，都是在第一个请求里面返回回来的。另外这个是跟视频绑定的，aid，cid

<hr>

**spmid**

跟上面那两个一样，都是跟视频深度绑定的，我猜想可能是存储到哪服务器的标识？

<hr>

**session**

这个是服务端随机生成的，看起来应该是md5那种的，

main?oid=325318514&type=1&mode=3&pagination_str=%7B%22offset%22:%22%22%7D&plat=1&seek_rpid=&web_lo

session是在这里返回的，使用正则拿到就行

### Cookie

前面也说了，cookie一般都是前面请求设置进去的，很少有生成的。这里直接找set_cookie的地方（看前面的请求）

**buvid3、b_nut**

这个请求里面返回的https://www.bilibili.com/video/BV1aw41157V3

<hr>
**b_lsid、_uuid、sid**

b_lsid和_uuid是客户端生成的，sid是https://api.bilibili.com/x/player/wbi/v2这个返回的

b_lsid

下面的r就是我们的cookie，看到也是随机生成的，直接去python伪造就可以了，注意毫秒值伪造一下

(0, l.G$)是函数o。(0, l.Q4)是函数a

```js
t = (0, l.G$)(e.millisecond) ,
 r = "".concat((0, l.Q4)(8), "_").concat(t) 
, a = function(e) {
                return Math.ceil(e).toString(16).toUpperCase()
}

 , o = function(e) {
                for (var t = "", r = 0; r < e; r++)
                    t += a(16 * Math.random());
                return i(t, e)
}
s.Z.setCookie("b_lsid", r, 0, "current-domain")
```



_uuid:这个一看就是随机生成的，随机生成直接伪造就可以了，就是一个uuid+时间戳+后缀

```js
   var n = function() {
                var e = o(8)
                  , t = o(4)
                  , r = o(4)
                  , n = o(4)
                  , a = o(12)
                  , s = (new Date).getTime();
                return e + "-" + t + "-" + r + "-" + n + "-" + a + i((s % 1e5).toString(), 5) + "infoc"
            }
              , o = function(e) {
                for (var t = "", r = 0; r < e; r++)
                    t += a(16 * Math.random());
                return i(t, e)
            }
   , i = function(e, t) {
                var r = "";
                if (e.length < t)
                    for (var n = 0; n < t - e.length; n++)
                        r += "0";
                return r + e
            }
```



**buvid4、buvid_fp**

这个请求里面返回的https://api.bilibili.com/x/frontend/finger/spi。buvid4

n是UA，o是cookie值

o = u().x64hash128(n, 31);

## Python复现

上面都找到了，接下来就是写py代码复现了。需要注意的是，对有些参数的逆向有的可以扣代码、补环境。这里我统一都用py代码复现了。这个代码太长，我在这里就不提供了，移步github：https://github.com/AZCodingAccount/python-spider

❗这个仅仅是学习使用，投入应用是不可行的，因为我们这里没有登录，最多刷200-300个播放。另外提醒大家，尽量也不要刷自己视频，用心做视频才会有更多同学看！







