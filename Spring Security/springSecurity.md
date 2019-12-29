1.springSecurity(基于表达式配置)

应用的安全性包括用户认证（Authentication）和用户授权（Authorization）两个部分.

认证：验证是否是该系统用户，一般通过用户名和密码，对用户进行认证。

授权：不同用户授予不同角色，不同角色具有不同权限。

使用：(讲下大概思路，仔细想想他帮我们做了什么事？无非就是完成用户名和密码的验证，然后记录下这个用户的角色集合罢了)

1.编写mySecurityConfig类继承WebSecurityConfigurerAdapter类，重写config方法，在里面配置拦截的路径和权限，csrf，成功失败处理器等，成功跳转路径和失败跳转路径等。

2.配置完拦截路径，那怎么完成用户名和密码校验呢？通过实现UserDetailsService接口，重写loadUserByUserName方法(可接受一个username参数，用于我们去数据查找用户名，无形之中就确定用户名就是唯一的了)，注入自己userService通过findUserByName查找出用户。最后封装成一个UserDetails(含用户名，密码，权限等这就是一个用户认证授权所有必须信息了)返回。

3.密码的比较必须使用他的加密器，使用专门的密码对比方法。(但是登录时，springSecurity会自动将返回的UserDetails密码和输入的进行对比)

4.大概认证就是这么多了，还有其他的一些配置，比如successHandler和failHandler等，可以自动配置。

5.授权：就是分配角色，前面的UserDetailsService中就查出了用户，所以之后遍历用户Roles属性，将所有含有的角色添加进List<GrantedAuthority>  list.add(new SimpleGrantedAuthority("ROLE_"+role))，之后用户名和密码一起添加进UserDetails中。