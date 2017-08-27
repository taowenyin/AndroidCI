# AndroidCITest
Android持续集成测试

```Bash
language: android
android:
  components:
  # 使用最新的Android SDK Tools
  - tools
  - platform-tools

  # 设置构建Android项目的构建工具版本
  - build-tools-26.0.1

  # 设置Android项目构建的目标版本
  - android-25

  # 添加必要的组件
  - extra-google-google_play_services
  - extra-google-m2repository
  - extra-android-m2repository
  - addon-google_apis-google-24
before_install:
  # 添加签名文件的解密指令
  - openssl aes-256-cbc -K $encrypted_8dbd61925ec5_key -iv $encrypted_8dbd61925ec5_iv
    -in keys/taowenyin.jks.enc -out keys/taowenyin.jks -d
  # 添加保存证书的文件夹
  - mkdir "$ANDROID_HOME/licenses" || true
  # 把证书信息添加到证书文件中
  - echo -e "\n8933bad161af4178b1185d1a37fbf41ea5269c55" > "$ANDROID_HOME/licenses/android-sdk-license"
before_script:
  # 为gradlew添加可执行权限，实现跨平台性
  - chmod +x gradlew
script:
  # 执行APP构建指令
  - "./gradlew assembleRelease"
deploy:
  # 执行部署到GitHub
  provider: releases
  # 部署的Access Token
  api_key:
    secure: jHI1x1vYMFdUE8i9VwMQIPBIQXCywhX4p9a7nl2CYKzShcyUeDDw0NMr+DPgBoyGhNsyJvDlfofcEDPdZI0Yd8QRNWeLKPE5dtWu6DI7qxo/kwLd+HxmVkoF2Dk2+TT25sgYwTo1X/LOvtrWOto99tVHjNZMzkNUm4/9PPRpmaQ+BhgeklT340ma2DI14BJZ2jaeR6ThCMB9Izh6DTNh+TozqhalvYGxNrihqDBcBWp81WLTfzs9mmaujyek7Qw5iAn0yJZG5NSlJZAcR4rINaL4jUtL75DrZbJtUh6V+0hjAXOje4fuURuNVOcWTJg188WZSFy1qze/MgpupGVhfEHsrnqEKtXZVi5s3MUVez1H36Eu6G9dmR8biuPaVt95NCKoZdP64sZAKvpiszJHsdmWoQsCFtgEXg/ULV5fkiMftIM/P4pq/o9rsLkdi/fUmnUqCCAO0IHtDQJzYSJVuiuoxpLu3ggGeRMTDf1a9o/hbSczxklnE3Tgze5MsqTfvgetpYOB1+6U2Q41Bf0TCukdJ+YBtBavVhiAzB5xpJ5spWgWQQdH1+zwtL+PKX8Y76jDXZZ2U8PGoVhEk0ukKoIOUmB9ZJpnQDFdFLGf58UazQm2Hgj8DvEOqxC0RWbAMO20Bc0x8al5ldIaWu3y5jZVfIKeQQEn2JOaXM7u1Aw=
  # 要部署文件的位置
  file: 'app/build/outputs/apk/app-release.apk'
  # 禁止清理生成文件
  skip_cleanup: true
  on:
    #指定部署的目标
    repo: taowenyin/AndroidCI
```