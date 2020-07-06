#### 1.redis缓存session

像以前我们都是使用redis存session信息，但是后来发现成本太高了，毕竟存了数据。(还是太年轻了)

随着之后JWT的出现，校验方式更加简单便捷化，无需通过redis缓存，而是直接根据token取出保存的用户信息，以及对token可用性校验，单点登录更为简单。

使用redis存session(session数据持久化)：缺点:1.比较浪费redis内存。2.如果redis持久层挂了，就全挂了。

#### 2.JWT

JSON Web Token（JWT）是目前最流行的跨域身份验证解决方案。

优点:通过客户端保存数据，而服务器根本不保存会话数据，每个请求都被发送回服务器。

(1.在认证服务器登录成功一次，生成用户登录凭证TGT(存在cookie中，全局的,前端可以将token存储到localStorage，不建议存到cookie，cookie容易被盗取)，通过这个就可以知道用户登录了2.用这个生成token返回给客户端，客户端将这个token去认证服务器检验，如果成功，就可以生成session
3.登录成功后，下次访问认证服务器，如果登录凭证还在，就可以直接生成token返回了。)

JWT可以省去服务端读取Session的步骤，这样更符合RESTful的规范。但是对于客户端（或App端）来说，为了保存用户
授权信息，仍然需要通过Cookie或类似的机制进行本地保存。因此JWT是用来取代服务端的Session而非客户端Cookie的方案.

**太强了，每次只看cookie，如果有就发token真好，比redis好多了，而且登录凭证就在用户这边，不用担心会挂**

**jwt使用原理:**