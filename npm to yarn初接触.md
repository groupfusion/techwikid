# npm yarn 初接触

# 安装
1. nodejs安装
 https://nodejs.org/dist/v12.16.3/node-v12.16.3-x64.msi
2. yarn 安装
 https://classic.yarnpkg.com/zh-Hans/docs/install#windows-stable 

# npm对应yarn命令
yarn和npm常用命令对比
|作用|npm命令|yarn命令|
|安装|npm install|yarn|
|安装某个包|npm install xxx -save|yarn add xxx|
|删除某个包|npm uninstall xxx -save|yarn remove xxx|
|开发模式下安装某个包|npm install xxx -save-dev|yarn add xxx -dev|
|更新|npm update -save|yarn upgrade|
|全局安装|npm install xxx -global|yarn global add xxx|
|清除缓存|npm cache clean|yarn cache clean|

|yarn	|npm	|命令功能|
|yarn install	|npm install	|根据pack.json安装项目所需的依赖包|
|yarn install --flat	|--	|注释1|
|yarn install --no-lockfile	|npm install --no-package-lock|	不读取或生成yarn.lock锁文件|
|yarn install --pure-lockfile	|--	|不要生成yarn.lock锁文件|
|yarn add [package]	|npm install [package]	|安装需要的依赖包|
|yarn add [package] --dev	|npm install [package] --save-dev	|注释2|
|yarn add [package] --D	|npm install [package] --save-dev	|同上|
|yarn add [package] --peer	|--	|注释3|
|yarn add [package] --P	|--	|同上|
|yarn add [package] --optional	|npm install [package] --save-optional	|注释4|
|yarn add [package] --O	|npm install [package] --save-optional	|同上|
|yarn add [package] --exact	|npm install [package] --save-exact	|注释5|
|yarn add [package] --E	|npm install [package] --save-exact	|同上|
|yarn global add [package]	|npm install [package] --global	|全局安装依赖包|
|yarn global upgrade	npm update --global	全局更新依赖包
|yarn add --force	npm rebuild	更改包内容后进行重建
|yarn remove [package]	npm uninstall [package]	卸载已经安装的依赖包
|yarn cache clean [package]	npm cache clean	注释6
|yarn upgrade	rm -rf node_modules && npm install	更新依赖包
|yarn version --major	npm version major	更新依赖包的版本
|yarn version --minor	npm version minor	更新依赖包的版本
|yarn version --patch	npm version patch	更新依赖包的版本

# OpenLayers Demo

1. yarn init

2. yarn add ol

3. yarn add parcel-bundler --dev

4. 创建 code


5. yarn run
```
yarn run v1.22.4
info Commands available from binary scripts: acorn, acorn.cmd, ansi-to-html, ansi-to-html.cmd, atob, atob.cmd, brfs, brfs.cmd, browserslist, browserslist.cmd, cssesc, cssesc.cmd, envinfo, envinfo.cmd, escodegen, escodegen.cmd, esgenerate, esgenerate.cmd, esparse, esparse.cmd, esvalidate, esvalidate.cmd, js-yaml, js-yaml.cmd, jsesc, jsesc.cmd, json5, json5.cmd, loose-envify, loose-envify.cmd, miller-rabin, miller-rabin.cmd, mime, mime.cmd, mkdirp, mkdirp.cmd, parcel, parcel.cmd, parser, parser.cmd, pbf, pbf.cmd, purgecss, purgecss.cmd, quote-stream, quote-stream.cmd, regjsparser, regjsparser.cmd, rimraf, rimraf.cmd, semver, semver.cmd, sha.js, sha.js.cmd, sshpk-conv, sshpk-conv.cmd, sshpk-sign, sshpk-sign.cmd, sshpk-verify, sshpk-verify.cmd, svgo, svgo.cmd, terser, terser.cmd, uncss, uncss.cmd, uuid, uuid.cmd, which, which.cmd
info Project commands
   - build
      parcel build --public-url . index.html
   - start
      parcel index.html
   - test
      echo "Error: no test specified" && exit 1
question Which command would you like to run?: start
$ parcel index.html
Server running at http://localhost:1234
```
示例：
https://blog.csdn.net/hequhecong10857/article/details/103866111
https://blog.csdn.net/wan320/article/details/50789072
https://blog.csdn.net/u012413551/article/details/95742140
https://blog.csdn.net/Supreme_Sir/article/details/105296872
https://blog.csdn.net/Supreme_Sir/article/details/105296872

https://openlayers.org/en/latest/examples/


