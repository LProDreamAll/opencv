
> @author Hh.li1993 https://github.com/LProDreamAll/opencv/blob/master/README.md
>文件下载地址 https://download.csdn.net/download/arcprodrelhh/10560350

## Install OpenCV3 on Ubuntu or mac 
### Install OpenCV3 on Ubuntu (linux Ubuntu Ubuntu Server 16.04 LTS java1.8 ant python3 环境 )
#### 只是测试了java 环境 (python网上一大堆)

```
2.2- 配置java环境
	从 https://github.com/opencv/opencv.git 下载zip文件 上传到linux 
	sudo apt-get update
	sudo add-apt-repository ppa:webupd8team/java
	sudo apt-get update
	sudo apt-get install oracle-java8-installer
	echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections
	sudo update-java-alternatives -s java-8-oracle
	测试jdk 是是否安装成功:
	javac -version

	配置JAVA_HOME(在这里不配置JAVA_HOME会导致opencv编译和java没有关系)
	/usr/lib/jvm/java-8-oracle
	vim /etc/profile

	export JAVA_HOME=/usr/lib/jvm/java-8-oracle
	export JRE_HOME=${JAVA_HOME}/jre   
	export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
	export PATH=${JAVA_HOME}/bin:$PATH
	source /etc/profile
```
```
2.3配置安装 Apache Ant
	sudo apt-get install ant
	sudo yum install ant
	#Verify Installation of Apache Ant
	ant -version
```
```
2.4配置 opencv linux环境
sudo apt-get update
 sudo apt-get upgrade
Remove any previous installations of x264
sudo apt-get remove x264 libx264-dev
sudo apt-get install build-essential checkinstall cmake pkg-config yasm
sudo apt-get install git gfortran
sudo apt-get install libjpeg8-dev libjasper-dev libpng12-dev
# 我的 Ubuntu 16.04
sudo apt-get install libtiff5-dev
#sudo apt-get install libtiff4-dev 
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libdc1394-22-dev
sudo apt-get install libxine2-dev libv4l-dev
sudo apt-get install libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev
sudo apt-get install qt5-default libgtk2.0-dev libtbb-dev
sudo apt-get install libatlas-base-dev
sudo apt-get install libfaac-dev libmp3lame-dev libtheora-dev
sudo apt-get install libvorbis-dev libxvidcore-dev
sudo apt-get install libopencore-amrnb-dev libopencore-amrwb-dev
sudo apt-get install x264 v4l-utils
sudo apt-get install libprotobuf-dev protobuf-compiler
sudo apt-get install libgoogle-glog-dev libgflags-dev
sudo apt-get install libgphoto2-dev libeigen3-dev libhdf5-dev doxygen
sudo apt-get install python-dev python-pip python3-dev python3-pip
sudo -H pip2 install -U pip numpy
sudo -H pip3 install -U pip numpy
#安装python包可能比较慢建议用国内的源(分钟级别变成毫秒级) 命令如下 pip install XXXX  -i http://pypi.douban.com/simple 
sudo pip3 install virtualenv virtualenvwrapper
echo "# Virtual Environment Wrapper"  >> ~/.bashrc
echo "source /usr/local/bin/virtualenvwrapper.sh" >> ~/.bashrc
source ~/.bashrc
#安装python的虚拟环境和相关的包(没有必要无需下载)
mkvirtualenv facecourse-py3 -p python3
workon facecourse-py3
pip3 install numpy scipy matplotlib scikit-image scikit-learn ipython -i http://pypi.douban.com/simple 
deactivate
#进入到之前解压的opencv的文件夹
nkdir build 
cd build 
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..
   如果在最后的地方显示
		--  Java:
		--     ant:                         /usr/bin/ant (ver 1.9.6)
		--     JNI:                         /usr/lib/jvm/java-8-oracle/include /usr/lib/jvm/java-8-oracle/include/linux /usr/lib/jvm/java-8-oracle/include
		--     Java wrappers:               YES
		--     Java tests:                  NO
说明目前没有出错!
   接下来 在build文件里执行
  make
  完成后会在build/bin 文件里生成opencv-341.jar 和会在build/lib 中生成libopencv_java341.so   

```		
直接在maven项目中使用	
	<!-- pom -->
		<dependency>
            <groupId>nu.pattern</groupId>
            <artifactId>opencv</artifactId>
            <version>3.4.1</version>
        </dependency>

        //代码
        public class SampleCVUtils {
    public static void main(String[] args) {
        String path;
        if (args.length == 0) {
            path = System.getProperty("user.dir") + "/src/main/resources/libopencv_java341.dylib";
        } else {
            ///home/ubuntu/libopencv_java341.so
            path = args[0];
        }
        System.out.println("path = " + path);
        System.load(path);
        System.out.println(Core.VERSION);
        Mat eye = Mat.eye(3, 3, CvType.CV_8UC1);
        System.out.println("eye = " + eye.dump());
    }

}


```

> 在maven中使用assembly:assembly 命令把当前的依赖打成jar包 放到linux下 
> 运行 java -classpath opencvtest-1.0-SNAPSHOT-jar-with-dependencies.jar SampleCVUtils /home/ubuntu/libopencv_java341.so (/home/ubuntu/
>libopencv_java341.so表示的是在linux下的libopencv_java341.so路径编译成功后原始是在build/lib下)
>
> 成功 

```
3.4.1
eye = [  1,   0,   0;
   0,   1,   0;
   0,   0,   1]
```

### mac上安装opencv

```
    brew install ant
    brew install opencv
    brew edit opencv

```
修改 -DBUILD_opencv_java=OFF in -DBUILD_opencv_java=ON 
获取libopencv_java341.dylib opencv-341.jar 文件(从官网下载源码经过编译后在build文件中生成的文件)
在idea中使用 mvn install:install-file -DgroupId=nu.pattern -DartifactId=opencv -Dversion=1.0 -Dpackaging=jar -Dfile=opencv3.4.1.jar
 
```	
```	
		直接在maven项目中使用	
		<!-- pom -->
		<dependency>
            <groupId>nu.pattern</groupId>
            <artifactId>opencv</artifactId>
            <version>3.4.1</version>
        </dependency>

        //代码
        public class SampleCVUtils {
    public static void main(String[] args) {
        String path;
        if (args.length == 0) {
            path = System.getProperty("user.dir") + "/src/main/resources/libopencv_java341.dylib";
        } else {
            ///home/ubuntu/libopencv_java341.so
            path = args[0];
        }
        System.out.println("path = " + path);
        System.load(path);
        System.out.println(Core.VERSION);
        Mat eye = Mat.eye(3, 3, CvType.CV_8UC1);
        System.out.println("eye = " + eye.dump());
    }

}


```

> 控制台打印结果
>
> 成功 

```
3.4.1
eye = [  1,   0,   0;
   0,   1,   0;
   0,   0,   1]
```
