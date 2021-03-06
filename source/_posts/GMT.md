---
title: GMT
date: 2015-05-14 10:12:01
author: SeisMan
categories: 地震学软件
tags: [GMT]
toc: true
---
## 前言

本文源于[SeisMan](http://seisman.info/)开源项目[GMT_docs](https://github.com/gmt-china/GMT_docs)

## Linux 下安装GMT
<h3 id="安装预编译版">安装预编译版</h3>
<p>大多数 Linux 发行版都可以通过系统自带的软件包管理器直接安装 GMT。但通常系统软件 源里自带的 GMT 版本都比较老，因而如果可能，还是建议 Linux 用户手动编译安装。</p>
<p>CentOS 7 用户:</p>
<pre><code>sudo yum install epel-release
sudo yum install GMT GMT-devel GMT-doc
sudo yum install dcw-gmt gshhg-gmt-nc4 gshhg-gmt-nc4-full gshhg-gmt-nc4-high</code></pre>
<p>Ubuntu 用户:</p>
<pre><code>sudo apt-get install gmt gmt-dcw gmt-gshhg</code></pre>
<p>其他发行版用户可以到 <a href="https://pkgs.org/" class="uri">https://pkgs.org/</a> 查询自己的 Linux 发行版软件源中是否包含 GMT 以及 GMT 的具体版本。</p>
<h3 id="使用社区提供的快速安装脚本">使用社区提供的快速安装脚本</h3>
<p>GMT 中文社区为常见的 Linux 发行版提供了 GMT 最新版本的安装脚本，见 <a href="https://github.com/gmt-china/gmt-easy-installer" class="uri">https://github.com/gmt-china/gmt-easy-installer</a>。</p>
<p>用户需要下载两个安装脚本：</p>
<ol style="list-style-type: decimal">
<li>自己的发行版对应的安装脚本，比如 <code>ubuntu-installer.sh</code>， 用于安装 GMT 所需的依赖</li>
<li>GMT安装脚本 <code>GMT-installer.sh</code> 用于编译安装GMT源码</li>
</ol>
<p>并依次执行 <code>ubuntu-installer.sh</code> 和 <code>GMT-installer.sh</code> 即可。</p>
<h3 id="从源码编译">从源码编译</h3>
<p>为了使用最新版本的 GMT，建议用户从源码编译 GMT。</p>
<h4 id="解决依赖关系">解决依赖关系</h4>
<p>GMT 主要依赖于 cmake（&gt;=2.8.5）、fftw（&gt;=3.3）、glib2（&gt;=2.32）、 netCDF（&gt;4.0且支持netCDF-4/HDF5）、ghostscript等。</p>
<blockquote>
<p><strong>warning</strong></p>
<p>由于 Linux 发行版众多，以下仅所列仅供参考，请自行确认自己的发行版上软件包的 具体名字。</p>
</blockquote>
<p>对于Ubuntu/Debian:</p>
<pre><code># 更新
$ sudo apt-get update

# 必须安装的包
$ sudo apt-get install gcc g++ cmake make libc6
$ sudo apt-get install ghostscript
$ sudo apt-get install libnetcdf-dev

# 可选包(即便安装不成功也不影响 GMT 的使用)
$ sudo apt-get install libgdal-dev python-gdal
$ sudo apt-get install liblapack3
$ sudo apt-get install libglib2.0-dev
$ sudo apt-get install libpcre3-dev
$ sudo apt-get install libfftw3-dev</code></pre>
<p>对于CentOS/RHEL/Fedora:</p>
<pre><code>$ sudo yum install epel-release  # CentOS用户必须先安装epel-release
# 必须安装的包
$ sudo yum install gcc gcc-c++ cmake make glibc
$ sudo yum install ghostscript
$ sudo yum install netcdf-devel

# 可选包
$ sudo yum install gdal-devel gdal-python
$ sudo yum install lapack64-devel lapack-devel
$ sudo yum install glib2-devel
$ sudo yum install pcre-devel
$ sudo yum install fftw-devel</code></pre>
<p>确认 netCDF 支持 netCDF-4/HDF5 格式:</p>
<pre><code>$ nc-config --has-nc4
yes</code></pre>
<p>若输出为 <code>yes</code> 则可正常安装 GMT，否则无法正常安装。</p>
<h4 id="下载">下载</h4>
<p>Linux安装GMT需要下载三个文件（这里提供的国内下载源）：</p>
<ol>
<li>GMT源码： <a href="http://mirrors.ustc.edu.cn/gmt/gmt-5.4.2-src.tar.gz" class="uri">http://mirrors.ustc.edu.cn/gmt/gmt-5.4.2-src.tar.gz</a></li>
<li>全球海岸线数据GSHHG： <a href="http://mirrors.ustc.edu.cn/gmt/gshhg-gmt-2.3.7.tar.gz" class="uri">http://mirrors.ustc.edu.cn/gmt/gshhg-gmt-2.3.7.tar.gz</a></li>
<li>全球数字图表DCW： <a href="http://mirrors.ustc.edu.cn/gmt/dcw-gmt-1.1.2.tar.gz" class="uri">http://mirrors.ustc.edu.cn/gmt/dcw-gmt-1.1.2.tar.gz</a></li>
</ol>
<h4 id="安装gmt">安装GMT</h4>
<p>将下载的三个压缩文件放在同一个目录里，按照如下步骤进行安装：</p>
<div class="sourceCode"><pre class="sourceCode bash"><code class="sourceCode bash"><span class="co"># 解压三个压缩文件</span>
$ <span class="kw">tar</span> -xvf gmt-5.4.2-src.tar.gz
$ <span class="kw">tar</span> -xvf gshhg-gmt-2.3.7.tar.gz
$ <span class="kw">tar</span> -xvf dcw-gmt-1.1.2.tar.gz

<span class="co"># 将gshhg和dcw数据复制到gmt的share目录下</span>
$ <span class="kw">mv</span> gshhg-gmt-2.3.7 gmt-5.4.2/share/gshhg
$ <span class="kw">mv</span> dcw-gmt-1.1.2 gmt-5.4.2/share/dcw-gmt

<span class="co"># 切换到gmt源码目录下</span>
$ <span class="kw">cd</span> gmt-5.4.2

<span class="co"># 新建用户配置文件</span>
$ <span class="kw">gedit</span> cmake/ConfigUser.cmake</code></pre></div>
<p>向 <code>cmake/ConfigUser.cmake</code> 文件中加入如下语句:</p>
<pre><code>set (CMAKE_INSTALL_PREFIX &quot;/opt/GMT-5.4.2&quot;)
set (GMT_INSTALL_MODULE_LINKS FALSE)
set (COPY_GSHHG TRUE)
set (COPY_DCW TRUE)
set (GMT_USE_THREADS TRUE)</code></pre>
<ul>
<li><code>CMAKE_INSTALL_PREFIX</code> 设置GMT的安装路径，可以修改为其他路径。对于没有 root 权限的用户，可以将安装路径设置为 <code>/home/xxx/software/GMT-5.4.2</code> 等有可读写 权限的路径；</li>
<li><code>GMT_INSTALL_MODULE_LINKS</code> 为FALSE，表明不在GMT的bin目录下建立命令的软链接， 也可设置为TRUE</li>
<li><code>COPY_GSHHG</code> 为TRUE会将GSHHG数据复制到 <code>GMT/share/coast</code> 下</li>
<li><code>COPY_DCW</code> 为TRUE会将DCW数据复制到 <code>GMT/share/dcw</code> 下</li>
<li><code>GMT_USE_THREADS</code> 表示是否开启某些模块的并行功能</li>
</ul>
<blockquote>
<p><strong>tip</strong></p>
<p>此处为了便于一般用户理解，只向 <code>cmake/ConfigUser.cmake</code> 中写入了必要的5行语句。</p>
<p>对于高级用户而言，可以直接在 GMT 提供的配置模板基础上进行更多配置。将 <code>cmake/ConfigUserTemplate.cmake</code> 复制为 <code>cmake/ConfigUser.cmake</code> ， 然后根据配置文件中的大量注释说明信息自行修改配置文件。</p>
</blockquote>
<p>继续执行如下命令以检查GMT的依赖关系:</p>
<pre><code># 注意，此处新建的 build 文件夹位于 gmt-5.4.2 目录下，不是 gmt-5.4.1/cmake 目录下
$ mkdir build
$ cd build/
$ cmake ..</code></pre>
<p><code>cmake ..</code> 会检查GMT对软件的依赖关系，我的检查结果如下:</p>
<pre><code>*  Options:
*  Found GSHHG database       : /home/user/GMT/gmt-5.4.2/share/gshhg (2.3.7)
*  Found DCW-GMT database     : /home/user/GMT/gmt-5.4.2/share/dcw-gmt
*  NetCDF library             : /usr/lib64/libnetcdf.so
*  NetCDF include dir         : /usr/include
*  GDAL library               : /usr/lib64/libgdal.so
*  GDAL include dir           : /usr/include/gdal
*  FFTW library               : /usr/lib64/libfftw3f.so
*  FFTW include dir           : /usr/include
*  Accelerate Framework       :
*  Regex support              : PCRE (/usr/lib64/libpcre.so)
*  ZLIB library               : /usr/lib64/libz.so
*  ZLIB include dir           : /usr/include
*  LAPACK library             : yes
*  License restriction        : no
*  Triangulation method       : Shewchuk
*  OpenMP support             : enabled
*  GLIB GTHREAD support       : enabled
*  PTHREAD support            : enabled
*  Build mode                 : shared
*  Build GMT core             : always [libgmt.so]
*  Build PSL library          : always [libpostscriptlight.so]
*  Build GMT supplements      : yes [supplements.so]
*  Build GMT Developer        : yes
*  Build proto supplements    : none
*
*  Locations:
*  Installing GMT in          : /opt/GMT-5.4.2
*  GMT_DATADIR                : /opt/GMT-5.4.2/share
*  GMT_DOCDIR                 : /opt/GMT-5.4.2/share/doc
*  GMT_MANDIR                 : /opt/GMT-5.4.2/share/man
-- Configuring done
-- Generating done</code></pre>
<p>正常情况下的检查结果应该与上面给出的类似。若出现问题，则需要检查之前的步骤是否 有误，检查完毕后重新执行 <code>cmake ..</code> ，直到出现类似的检查结果。检查完毕后， 开始编译和安装:</p>
<pre><code>$ make
$ sudo make install</code></pre>
<blockquote>
<p><strong>note</strong></p>
<p>对于多核计算机，可以使用如下命令实现并行编译以减少编译时间:</p>
<pre><code>$ make -j
$ sudo make -j install</code></pre>
<p>但并行编译可能在个别发行版上无法使用。</p>
</blockquote>
<h4 id="修改环境变量">修改环境变量</h4>
<p>修改环境变量并使其生效：</p>
<div class="sourceCode"><pre class="sourceCode bash"><code class="sourceCode bash">$ <span class="kw">echo</span> <span class="st">&#39;export GMT5HOME=/opt/GMT-5.4.2&#39;</span> <span class="kw">&gt;&gt;</span> ~/.bashrc
$ <span class="kw">echo</span> <span class="st">&#39;export PATH=${GMT5HOME}/bin:$PATH&#39;</span> <span class="kw">&gt;&gt;</span> ~/.bashrc
$ <span class="kw">echo</span> <span class="st">&#39;export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:${GMT5HOME}/lib64&#39;</span> <span class="kw">&gt;&gt;</span> ~/.bashrc
$ <span class="kw">exec</span> <span class="ot">$SHELL</span> -l</code></pre></div>
<h4 id="测试是否安装成功">测试是否安装成功</h4>
<p>在终端键入 <code>gmt</code> ，若出现如下输出，则安装成功:</p>
<pre><code>$ gmt --version
5.4.2</code></pre>


<h2 id="macos-下安装-gmt">macOS 下安装 GMT</h2>
<p>macOS 下 GMT 的安装方法有很多，可以直接使用安装包，也可以使用各种软件管理工具。</p>
<p>推荐使用 homebrew 方式安装。</p>
<h3 id="使用-homebrew-安装">使用 homebrew 安装</h3>
<p><a href="http://brew.sh/">Homebrew</a> 是 macOS 下的第三方软件包管理工具。</p>
<ol style="list-style-type: decimal">
<li><p>安装 Homebrew:</p>
<pre><code>$ /usr/bin/ruby -e &quot;$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)&quot;</code></pre></li>
<li><p>安装 GMT:</p>
<pre><code>$ brew update &amp;&amp; brew upgrade
$ brew tap homebrew/science
$ brew install gmt</code></pre></li>
<li><p>测试安装是否成功:</p>
<pre><code>$ gmt --version
5.4.2</code></pre></li>
<li><p>如果你想要同时安装 GMT4 和 GMT5:</p>
<pre><code>$ brew unlink gmt &amp;&amp; brew install gmt4</code></pre></li>
<li><p>从 GMT5 切换至 GMT4:</p>
<pre><code>$ brew unlink gmt &amp;&amp; brew link gmt4</code></pre>
<p>从 GMT4 切换至 GMT5:</p>
<pre><code>$ brew unlink gmt4 &amp;&amp; brew link gmt5</code></pre></li>
</ol>
<blockquote>
<p><strong>warning</strong></p>
<p>以下几种安装方法翻译自官方文档，我们未作验证。</p>
</blockquote>
<h3 id="使用-gmt-安装包">使用 GMT 安装包</h3>
<p>GMT 为 macOS 用户提供了 dmg 安装包。</p>
<ol style="list-style-type: decimal">
<li>到社区主页的 <a href="http://gmt-china.org/download/">下载页面</a> 下载最新版本的 dmg 安装包。</li>
<li>双击 dmg 包以解压，将解压得到的 <code>GMT-5.4.2.app</code> 拖动到 Applications 目录即可。</li>
<li><p>GMT 默认会安装到 <code>/Applications/GMT-5.4.2.app/</code> 目录下，将如下语句:</p>
<pre><code>export PATH=${PATH}:/Applications/GMT-5.4.2.app/Contents/Resources/bin</code></pre>
<p>加入到 <code>~/.bashrc</code> 中即可。</p></li>
<li><p>测试安装是否成功:</p>
<pre><code>$ gmt --version
5.4.2</code></pre></li>
</ol>
<h3 id="使用-macports-安装">使用 macports 安装</h3>
<pre><code>sudo port install gdal +curl +geos +hdf5 +netcdf +fftw3 +openmp
sudo port install gmt5</code></pre>
<h3 id="使用fink安装">使用fink安装</h3>
<pre><code>sudo fink install gmt5</code></pre>

<h2 id="windows-下安装-gmt">Windows 下安装 GMT</h2>
<p>GMT 最初是在类 Unix 系统上开发的，尽管已经移植到了 Windows 上，但还是建议尽可能在类 Unix 系统上使用。</p>
<p>在 Windows 下使用 GMT 有如下几种途径：</p>
<ol>
<li>使用 GMT 提供的 Windows 安装包</li>
<li>在 <a href="https://www.cygwin.com/">cygwin</a> 中安装 GMT</li>
<li>在 <a href="http://msys2.github.io/">MSYS2</a> 中安装 GMT</li>
<li>在 <a href="https://mingw-w64.org/doku.php">MinGW-w64</a> 中安装GMT</li>
<li>使用 Windows 10 提供的 Bash on Ubuntu on Windows</li>
</ol>
<h3 id="windows下的gmt安装包">Windows下的GMT安装包</h3>
<p>GMT 为 Windows 用户提供了安装包，可以直接安装使用。Windows 下需要安装 GMT、ghostscript 和 gsview。</p>
<blockquote>
<p><strong>warning</strong></p>
<p>从 GMT 5.2.1 开始，GMT 提供的 Windows 下的安装包已经不再支持 Windows XP。</p>
</blockquote>
<ol style="list-style-type: decimal">
<li><p>下载</p>
<p>到社区主页的 <a href="http://gmt-china.org/download/">下载页面</a> 下载所需要的安装包。</p></li>
<li><p>安装GMT</p>
<p>直接双击安装包即可安装，直接点击下一步，使用默认的选项即可，无须做任何修改。在“选择组件”页面， 建议将四个选项都勾选上，然后点击下一步安装完成。</p>
<blockquote>
<p><strong>note</strong></p>
<p>安装过程中可能会出现“Warning! PATH too long installer unable to modify PATH!”的警告。 出现此警告的原因是系统的环境变量 <code>PATH</code> 太长，GMT安装包无法直接修改。</p>
<p>解决办法是，先忽略这一警告，待安装完成后按照如下步骤自行修改系统环境变量 <code>PATH</code> 。</p>
<ol style="list-style-type: decimal">
<li>点击“计算机”-&gt;“属性”-&gt;“高级系统设置”-&gt;“环境变量”打开“环境变量”编辑工具</li>
<li>在“系统变量”部分中，选中“Path”并点击“编辑”</li>
<li>在“变量值”的最后加上GMT的安装路径。需要注意“path”变量值中多个路径之间用英文分号分隔</li>
</ol>
</blockquote>
<p>安装完成后，“开始”-&gt;“所有程序”-&gt;“附件”-&gt;“命令提示符”以启动cmd。在cmd窗口中执行:</p>
<pre><code>C:\Users\xxxx&gt; gmt --version
5.4.2</code></pre>
<p>即表示安装成功。</p></li>
<li><p>安装ghostscript</p>
<p>安装的过程没什么可说的，在最后一步，记得勾选“Generate cidfmap for Windows CJK TrueType fonts”。</p></li>
<li><p>安装gsview</p>
<p>双击直接安装即可。</p></li>
<li><p>安装 UnixTools</p>
<p>解压压缩包，并将解压得到的 exe 文件移动到 GMT 的 bin 目录即可。</p></li>
</ol>
<h3 id="在-cygwin-中安装">在 cygwin 中安装</h3>
<p>欢迎补充，请参考：</p>
<ol>
<li><a href="http://gmt.soest.hawaii.edu/doc/5.4.2/GMT_Docs.html#cygwin-and-gmt" class="uri">http://gmt.soest.hawaii.edu/doc/5.4.2/GMT_Docs.html#cygwin-and-gmt</a></li>
<li><a href="http://gmt.soest.hawaii.edu/projects/gmt/wiki/BuildingGMT#Cygwin" class="uri">http://gmt.soest.hawaii.edu/projects/gmt/wiki/BuildingGMT#Cygwin</a></li>
</ol>
<h3 id="在-msys2-中安装">在 MSYS2 中安装</h3>
<ol style="list-style-type: decimal">
<li>下载并安装 <a href="http://msys2.github.io/">MSYS2</a></li>
</ol>
<p>欢迎补充</p>
<h3 id="在-mingw-w64-中安装">在 MinGW-w64 中安装</h3>
<p>欢迎补充</p>
<h3 id="bash-on-ubuntu-on-windows">Bash on Ubuntu on Windows</h3>
<p>欢迎补充</p>
