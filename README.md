# Awesome Installation

## 本地构建
项目主页用静态网站引擎 [Hugo](https://gohugo.io/) 构建。
1.  安装 Hugo
    Hugo 具有跨平台、易安装的特点。

    对于Linux & Windows 用户，直接到 [Release](https://github.com/spf13/hugo/releases) 页面下载平台对应的安装包，解压得到一个二进制文件，即可使用。

2.  下载源码
  ```
  $ git clone https://github.com/nicklinyi/awesome-installation.git
  $ cd awesome-installation
  ```

3.  更新源码
  ```
  $ git pull
  ```

4.  本地查看网页效果

    终端执行 `hugo server` 然后浏览器打开 `localhost:1313` 即可。

## 贡献

Feel free to update this content, just click the Edit this page link displayed on top right of each page, and pull request it.

### push to master

    git add . && git commit -m "update " && git push origin master

### push to gh-pages

    git subtree push --prefix public origin gh-pages
