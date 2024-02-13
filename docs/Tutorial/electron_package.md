## 前言

本教程的应用结构：[AZCodingAccount/iTime: 基于electron、vue3、Arco Design的桌面端效率软件 (github.com)](https://github.com/AZCodingAccount/iTime)

恕我直言，网上的electron打包教程10个里面有9坨。踩了5个小时的坑才整完，看了几十个文章，基本没一个可以把一个大项目完整打下来的，看到几篇都是官方示例往上一搬就是vue+electron打包教程了，自己看都不看就硬搬。离谱至极

不使用electron-forge，使用electron-builder    官网：[electron-builder](https://www.electron.build/)

## 详细步骤

1：安装electron-is-dev 用来判断是开发环境还是生产环境（其他方式也可） 

```js
pnpm i electron-is-dev  # 安装
async function checkIsDev() {
  const module = await import("electron-is-dev");
  isDev = module.default;
  work();
} # 使用
```

2：改装项目，如果有使用加载路由的，改成加载文件url，vite会自动导航，**修改vue-router模式为hash**

如果需要创建新窗口，修改代码如:

```bash
 if (isDev) {
    pomodoroWindow.loadURL("http://localhost:5173/fullscreen/pomodoro");
  } else {
    pomodoroWindow.loadURL(
      `file://${path.join(
        __dirname,
        "dist",
        "index.html"
      )}#/fullscreen/pomodoro`
    );
  } 
```

3：新建.npmrc文件(需要从github拉取，添加一个镜像更快)

```bash
node-linker=hoisted     # pnpm兼容配置，参考官方issue https://github.com/electron-userland/electron-builder/issues/6289#issuecomment-1042620422
ELECTRON_MIRROR=https://npmmirror.com/mirrors/electron/
ELECTRON_BUILDER_BINARIES_MIRROR=https://npmmirror.com/mirrors/electron-builder-binaries/
```

4：安装electron-builder

```bash
pnpm i electron-builder -D
```

5：打包vue项目：

```bash
pnpm build
```

6：修改package.json（开发和生产环境依赖已省略 ）

```bash
{
  "name": "iTime",
  "version": "1.0.0",
  "description": "基于electron+vue3+arco design开发的桌面效率工具",
  "author": "AlbertZhang<han892577@qq.com>",
  "private": true,
  "main": "main.js",
  "homepage": "./",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "start": "nodemon --exec electron . --watch ./ --ext .js,.html,.css,.vue",
    "electron:build": "electron-builder"    # 添加这个脚本
  },    # 添加下面这个配置项
  "build": {
    "appId": "com.albertzhang.itime",
    "productName": "iTime",
    "directories": {
      "output": "dist_electron" # 指定打包后的输出目录
    },
    "nsis": {
      "oneClick": false,
      "allowToChangeInstallationDirectory": true,
      "deleteAppDataOnUninstall": true  # 卸载时清除用户数据
    },  # 取消一键安装，允许用户自定义
    "extraFiles": [{
      "from": "src/assets",
      "to": "resources/assets"
    },
      {
        "from": "软件使用说明书.md",
        "to": "软件使用说明书.md"
      }],    # 原封不动拷贝
    "afterPack": "scripts/afterPack.js",
    "artifactBuildCompleted": "scripts/rename.js",
    "files": [
      "dist/**/*",
      "main.js",
      "preload/**/*",
      "package.json"
    ],  # 定义应该被打包的文件路径
    "win": {
      "target": [
        {
          "target": "nsis"
        },
        {
          "target": "portable"
        },
        {
          "target": "msi"
        }
      ],
      "icon": "dist/icons/icon.png"
    },
    "mac": {
      "target": "dmg"
    }
  }
}
```

7：配置vite.config.js文件（根属性）。一定要设置，不然会白屏，因为路径找不到

```bash
base: "./", // 设置为相对路径
```

如果有引用到本地文件的，静态文件可以放到**public**或者**assets**下均可正常引用(vite会处理)。如果需要设置**动态加载的静态文件**按照下面步骤：

1. 获取app路径： getAppPath: () => process.resourcesPath：preload.js

2. 渲染进程（组件）中引入并使用：window.键名.getAppPath()，手动跟之前原封不动拷贝的`extraFiles`文件路径做拼接，注意getAppPath获取的是`win-unpacked\resources`目录，自己拷贝的目录文件在哪，手动拼接即可

3. 如果想设置动态背景图url，绝对路径遵循下面的格式，其他格式不可生效(自己对空格，前缀\这些进行一下处理)

   ```
   标准格式：
   file:///C:/Users/Albert%20han/Desktop/easyToDo/dist_electron/win-unpacked/resources/assets/timeBGI.jpg
   拼接格式（appPath+extraFiles）
   C:\\Users\\Albert han\\Desktop\\easyToDo\\dist_electron\\win-unpacked\\resources/assets/timeBGI.jpg
   ```

ℹ️：对于播放音频这种，简单拼接即可，Windows可以兼容\和/同时存在的路径

ℹ️：我们使用__dirname获得的路径不强关联与任何本机相关的路径，引用静态资源或创建目录时使用app.getPath或process.resourcesPath获取路径

8：运行打包命令

```bash
pnpm electron:build -- --win    # 注意自己包管理工具的语法
```

## 优化大小

我的应用打包完成是320MB，这个体积是非常不友好的。打包完成以后发现有些文件是完全没有必要，如下图

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20240201212512248.png" alt="image-20240201212512248" style="zoom: 33%;" />

- 其中locales是多语言的配置，36MB，我的应用不需要多语言，因此把除中文的全部排除
- 资源目录下是我们的asar归档文件，我的是40多M，有优化的空间但是不多，如优化代码，优化依赖项等

可以看asar文件中包含的是什么

```bash
npm i -g asar   # 下载插件
asar extract app.asar ./app #解压asar文件
```

- 两个许可证文件LICENSES.chromium和LICENSE.electron，我这里选择直接不要，优化了9MB

一共优化了45M，这个自己开发的话还是可以的，至少比网上的一些人才让把node_modules文件夹排除了有用点

需要自己在afterPack钩子后执行一个脚本，这个脚本返回一个异步函数负责删除那些不用的文件

在package.json里面添加这个配置项

```json
"afterPack": "scripts/afterPack.js",
```

files也要包含这个路径

```json
  "files": [
      "dist/**/*",
      "main.js",
      "preload/**/*",
      "package.json",
      "scripts/**/*"
 ],
```

脚本文件

```js
const path = require("path");
const fs = require("fs-extra"); // fs 的一个扩展

module.exports = async (context) => {
  const unpackedDir = path.join(context.appOutDir, "locales");

  // 删除除 zh-CN.pak 之外的所有文件
  const files = await fs.readdir(unpackedDir);
  for (const file of files) {
    if (!file.endsWith("zh-CN.pak")) {
      await fs.remove(path.join(unpackedDir, file));
    }
  }

  // 删除特定的文件
  const filesToDelete = ["LICENSE.electron.txt", "LICENSES.chromium.html"];

  for (const fileName of filesToDelete) {
    const filePath = path.join(context.appOutDir, fileName);
    if (await fs.pathExists(filePath)) {
      await fs.remove(filePath);
    } 
  }
};
```

经过一系列操作，优化到了280MB。

## 最后

​		electron-builder有三个钩子：`artifactBuildCompleted`、`afterPack`、`afterSign`，分别是构建完成、打包完成、签名完成、可以自动化构建流程。我这里还用到了构建完成，自动化命名构建文件（当然，手动命名也是可以的）。

```js
const fs = require("fs");
const path = require("path");

module.exports = async function (params) {
  // 读取版本号
  const packageJson = require("../package.json");
  const version = packageJson.version;
  let artifact = params.file;
  let originFile = artifact;

  const ext = path.extname(artifact);
  // 处理版本号前缀，注意空格
  artifact = artifact.replace(` ${version}`, `-${version}`);
  let newName;

  if (ext === ".exe" && !artifact.includes("Setup")) {
    // 不用安装的程序
    newName = artifact.replace(/\.exe$/, "-windows-no-installer.exe");
  } else if (ext === ".exe") {
    // 常规安装包
    newName = artifact.replace(
      ` Setup-${version}.exe`,
      `-${version}-windows-installer.exe`
    );
  } else {
    newName = artifact;
  }

  // 重命名
  if (newName) {
    fs.renameSync(originFile, newName);
  }
};
```

​		其他还有一些进阶语法，比如增量更新，有这种需求官网和stackoverflow、issues区都可以，其他的见仁见智吧，国内用的人还是不太多，有的回答API都废弃了，注意版本。