# 编译

1. 安装 OpenJDK

```
wget https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.deb

apt install ./jdk-21_linux-x64_bin.deb

export JAVA_HOME=/usr/lib/jvm/jdk-21-oracle-x64

export PATH=$JAVA_HOME/bin:$PATH
```

2. 安装 Android SDK

```
wget -O commandlinetools-linux.zip 'https://dl.google.com/android/repository/commandlinetools-linux-10406996_latest.zip?hl=zh-cn'

unzip commandlinetools-linux.zip

export PATH=$HOME/cmdline-tools/bin:$PATH

mkdir AndroidSDK

sdkmanager --list --sdk_root=$HOME/AndroidSDK

sdkmanager "platforms;android-30" "platform-tools" "build-tools;30.0.3"  "ndk-bundle" --sdk_root=$HOME/AndroidSDK
```

3. 安装 Gradle

```
wget https://services.gradle.org/distributions/gradle-8.5-bin.zip

unzip gradle-8.5-bin.zip

export PATH=$HOME/gradle-8.5/bin:$PATH
```

4. 编译 Libbox

```
CGO_ENABLED=1 gomobile bind -v -androidapi 30 -javapkg=io.nekohasekai -libname=box -tags with_dhcp,with_lwip,with_gvisor,with_v2ray_api,with_quic,with_wireguard,with_utls,with_reality_server,with_clash_api,with_grpc,with_ech,with_provider -ldflags "-X github.com/sagernet/sing-box/constant.Version=v1.8.0-beta.9-2220feb -buildid=" ./experimental/libbox

# 将生成的 libbox.aar 置于 sing-box-for-android/app/libs 目录下
```

5. 准备 Android Keystore

```
keytool -genkeypair -alias <别名 ALIAS_NAME> -keyalg RSA -keysize 2048 -validity 3650 -storetype JKS -keystore release.keystore

# release.keystore 置于 sing-box-for-android/app 目录下
```

```
Enter keystore password: # 输入 JKS 密码 <KEYSTORE_PASS>
Re-enter new password: # 输入 JKS 密码 <KEYSTORE_PASS>
Enter the distinguished name. Provide a single dot (.) to leave a sub-component empty or press ENTER to use the default value in braces.
What is your first and last name?
  [Unknown]: # 基本信息，自用可以直接回车
What is the name of your organizational unit?
  [Unknown]: # 基本信息，自用可以直接回车
What is the name of your organization?
  [Unknown]: # 基本信息，自用可以直接回车
What is the name of your City or Locality?
  [Unknown]: # 基本信息，自用可以直接回车
What is the name of your State or Province?
  [Unknown]: # 基本信息，自用可以直接回车
What is the two-letter country code for this unit?
  [Unknown]: # 基本信息，自用可以直接回车
Is CN=Unknown, OU=Unknown, O=Unknown, L=Unknown, ST=Unknown, C=Unknown correct?
  [no]:  yes # 填 yes
Generating 2,048 bit RSA key pair and self-signed certificate (SHA384withRSA) with a validity of 3,650 days
        for: CN=Unknown, OU=Unknown, O=Unknown, L=Unknown, ST=Unknown, C=Unknown
Enter key password for <别名 ALIAS_NAME>
        (RETURN if same as keystore password): # 输入别名密码 <ALIAS_PASS>
Re-enter new password:  # 输入别名密码 <ALIAS_PASS>
```

6. 创建 local.properties 文件

```
# local.properties 置于 sing-box-for-android 目录下

VERSION_CODE=1
VERSION_NAME=1.8.0-beta.9-2220feb
KEYSTORE_PASS=sing-box-personal
ALIAS_NAME=sing-box-personal
ALIAS_PASS=sing-box-personal
```

7. 编译

```
./gradlew assembleRelease
```
