# Useful image processing scripts

## Convert HEIC image format to JPG

```javascript
function getFiles(pattern) {
  return readdirSync(process.cwd())
    .map((file) => resolve(process.cwd(), file))
    .filter((file) => pattern.test(file));
}

function convertHeicFiles() {
  getFiles(/\.heic$/i).map((file) => {
    const command = `magick convert "${file}" "${file.replace(
      extname(file),
      ".jpg"
    )}"`;
    console.log(command);
    execSync(command);
  });
}

convertHeicFiles();
```
