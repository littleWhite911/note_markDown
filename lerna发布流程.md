---
tags: '发布流程
grammar_cjkRuby: false
---
>注： 当前npm仓库名为`miniwhite-cli-dev`
# 脚手架初始化

1. 初始化npm `npm init -y`
2. 全库安装lerna `npm i -g lerna`
3. lerna初始化项目 `lerna init`

# 创建package
1. 创建脚手架1 `lerna create core`
    - `package name: @miniwhite-cli-dev/core`
    - 则会在package文件中创建出来core文件
2. 创建脚手架2：`lerna create utils`

# 添加/删除依赖

### 添加
1. 例如添加vux `lerna add vuex`
2. 添加指定目录安装:`lerna add @miniwhite-cli-dev/utils package/core/`

### 链接
1. 重新安装package中的依赖 `lerna bootstrap`
2. 手动添加依赖`lerna link`
    - 当前项目miniwhite-cli-dev中存在2个包`core`和`utils`
    - 修改*utils文件*中package.json中的main，指向自身目录lib/index.js中
    - 在*core文件*中package.json添加依
      ```json
      dependencies:{
       "@miniwhite-cli-dev/utils":"^1.0.0"
      }
      ```
     - 在执行一次 lerna link


### 删除
1. 清空依赖 `lerna clean`, 执行之后手动删除package.josn中的依赖关系
2. 执行`lerba exec -- rm -rf node_nodules/`，会删除每个package中的目录，根目录下的node_modules不会删除


# 脚手架发布上线
1. 先在git创建仓库
2. 在项目中`git remote add origin <git SSH地址>`手动建立链接
3. 提交代码到git：`git push origin master --set-upstream`
4. `lerna version` 选择版本号，一般选择path
5. `lerna publish` 发布

### 发布报错信息
```
lerna WARN ENOLICENSE Package @miniwhite-cli-dev/core is missing a license.
lerna WARN ENOLICENSE One way to fix this is to add a LICENSE.md file to the root of this repository.
lerna WARN ENOLICENSE See https://choosealicense.com for additional guidance.
lerna http fetch PUT 402 https://registry.npmjs.org/@miniwhite-cli-dev%2fcore 433ms
lerna ERR! E402 You must sign up for private packages
```
1. 可以在当前文件目录下添加`LICENSE.md`文件
2. 修改所有的package.json中的version文件与lerna中的version相同
3. 每个package中的package.json中添加
 ```
"publishConfig":{
    "access":"public"
}
 ```

