## webpack 配置proxy 解决跨域

在前端开发期间 ,前端在本地起了服务，和后端的服务是独立开来的

要访问后端服务会涉及到跨域问题。

例如前端环境是`http://localhost:8090`,后台环境是：`http://localhost:80/simi`，直接访问会报跨域问题。


### 配置webpack devServer 的 proxy选项

    proxy: {
             '/index.php': {
    			target: 'http://localhost:80/simi',
                 changeOrigin: true,
                 secure: false
             }
             }

这样配置,在使用axios请求服务的时候,只要是前面带`/index.php`的路径,会自动被转到`http://localhost:80/simi`服务上

相当于在前面追加上了 'http://localhost:80/simi'

并且可以跨域

---
试了几次,终于可以正常工作了...

**注意路径不能写重复**,它是把target **追加** 到了前面!

