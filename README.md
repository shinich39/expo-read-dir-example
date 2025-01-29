# expo-read-dir-example

- Legacy

```ts
import * as FileSystem from 'expo-file-system';

async function readDir(uri: string, prefix?: string) {
  const files = await FileSystem.readDirectoryAsync(uri);
  const result: Record<string, string> = {};
  for (const file of files) {
    const fileUri = `${uri}/${file}`;
    const fileInfo = await FileSystem.getInfoAsync(fileUri);
    if (fileInfo.isDirectory) {
      Object.assign(result, await readDir(fileUri, prefix ? `${prefix}/${file}` : file))
    } else {
      result[prefix ? `${prefix}/${file}` : file] = fileUri;
    }
  }
  return result;
}

readDir(uri);
```

- New FileSystem

```ts
import { File, Directory, Paths } from 'expo-file-system/next';

async function readDir(directory: Directory, prefix?: string) {
  const files = directory.list();
  const result: Record<string, File> = {};
  for (const file of files) {
    if (file instanceof Directory) {
      Object.assign(result, readDir(file, prefix ? `${prefix}/${file.name}` : file.name));
    } else {
      result[prefix ? `${prefix}/${file.name}` : file.name] = file;
    }
  }
  return result;
}

readDir(new Directory(uri));
```
