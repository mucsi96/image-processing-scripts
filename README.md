# Useful image processing scripts

## Convert HEIC image format to JPG

```javascript
const { execSync } = require("child_process");
const { readdirSync } = require("fs");
const { resolve, extname } = require("path");

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

## Rename image files based on image creation date

```javascript
const { execSync } = require("child_process");
const { readdirSync, renameSync } = require("fs");
const { resolve, extname, join, dirname } = require("path");

function getDateTime(file) {
  const command = `magick identify -format %[EXIF:DateTime] "${file}"`;
  console.log(command);
  return execSync(command, {
    encoding: "utf8",
  });
}

function getFiles(pattern) {
  return readdirSync(process.cwd())
    .map((file) => resolve(process.cwd(), file))
    .filter((file) => pattern.test(file));
}

function renameToDateTime(file) {
  const [year, month, day, hours, minutes, seconds] = getDateTime(file).split(
    /[: ]/
  );
  const newName = join(
    dirname(file),
    `${year}-${month}-${day}T${hours}${minutes}${seconds}${extname(file)}`
  );
  renameSync(file, newName);
}

getFiles(/\.jpg$/i).forEach(renameToDateTime);
```
