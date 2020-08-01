# Useful image processing scripts

## Convert HEIC image format to JPG

```batch
@ECHO OFF
setlocal enabledelayedexpansion
for %%f in (.\*.heic) do (
  set /p val=<%%f
  magick convert "%%~nf.heic" "%%~nf.jpg"
)
```
