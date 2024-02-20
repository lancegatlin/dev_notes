Note: no OSX aarch available for 3.11.4 release; x84_64 appears to run in emulation ok

1. Download `protoc-3.11.4-osx-x86_64.zip` from https://github.com/protocolbuffers/protobuf/releases/tag/v3.11.4
2. ```
```
cd ~/Downloads
unzip protoc-3.11.4-osx-x86_64.zip
cd protoc-3.11.4-osx-x86_64
sudo cp bin/protoc /usr/local/bin
sudo cp -R include/google /usr/local/include
protoc --version
```
Note: first `protoc` call will fail because of OSX security. Allow in Security & Privacy tab.
3. Modify build.sbt:
```
PB.protocExecutable := file( "/usr/local/bin/protoc" ),
```