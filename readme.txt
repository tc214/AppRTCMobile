project path: out\myWebRTC\gradle\






编译WebRTC Android源码步骤:

//我的源码放在root根路径下
//根路径下，设置depot_tools环境变量
export PATH=`pwd`/depot_tools:"$PATH"

/*安装编译依赖软件和环境，这个过程会安装linux的基础环境和Android的基础环境*/
src/build/install-build-deps-android.sh 


/*设置各种环境变量*/
/*进入src目录，执行*/
. build/android/envsetup.sh

/*生成编译脚本*/
gn gen out/myWebRTC --args='target_os="android" target_cpu="arm"'

#You can specify a directory of your own choice instead of out/Debug, to enable managing multiple configurations in parallel.

#To build for ARM64: use target_cpu="arm64"
#To build for 32-bit x86: use target_cpu="x86"
#To build for 64-bit x64: use target_cpu="x64"




(一) 编译生成WebRTC Android Demo的Android studio工程
1、项目编译
ninja -C out/myWebRTC  AppRTCMobile
#耗时大概20-30分钟

2、gradle 构建文件生成
Android studio 项目是依赖gradle进行构建编译的，在上面步骤编译出来的项目并没有gradle依赖文件，因此还需要进行编译生成，在src目录下执行命令：
build/android/gradle/generate_gradle.py --output-directory $PWD/out/myWebRTC \
--target "//examples:AppRTCMobile" --use-gradle-process-resources \
--split-projects
执行结束后就会在当前 **/out/myWebRTC目录下出现 gradle文件夹，即Android studio工程目录。

3、open the project
启用Android studio，import 当前gradle生成目录，即 out/myWebRTC/gradle
该过程需要Java1.8.
 

(二) 全编译
全编译指的是所有源代码的编译，编译文件会稍多，并且编译时间会稍长
/*编译*/
ninja -C out/myWebRTC

#编译完成后，会生成out/myWebRTc目录