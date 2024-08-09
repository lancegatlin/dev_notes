decoding messages for troubleshooting
```
pbpaste | base64 -D | gunzip | protoc --decode_raw
```
