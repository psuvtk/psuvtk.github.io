# HUGO 本博客部署


&lt;!--more--&gt;


在hugo生成的根目录部署发现会出问题，但是部署public目录就是正常的


那么每次生成的时候将public目录内容进行git commit, 然后推送到远端

原始内容怎么办呢？
在public目录下创建 repo目录, 然后将外层目录拷贝到repo目录推送到远端；
更换电脑后，克隆仓库，然后将public/repo目录内容拷贝到外层;

---

> Author:   
> URL: https://psuvtk.github.io/posts/7398491/  

