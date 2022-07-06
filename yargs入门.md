---
tags: 命令解析
grammar_cjkRuby: false
---
``` javascript
// yargs入门
const yargs = require("yargs/yargs");
const { hideBin } = require("yargs/helpers");

// 注入命令参数（方式一） 例如：注入版本号
// 获取package内参数
const pkg = require("../package.json");
const context = {
  miniwhiteVersion: pkg.version,
};
const argv = process.argv.splice(2);
//const cli = yargs();  //方式一的情况下不传入参

// 注入命令参数（方式二）
hideBin(process.argv);
const arg = hideBin(process.argv);
const cli = yargs(arg);

cli
  //usage 用法描述
  .usage("Usage: miniwhite001 [command] <options>")
  .demandCommand(1, "最少输入一个command")

  // alias: 别名
  .alias("h", "help")
  .alias("v", "version")

  // 命令输入错误时，会提示相近的命令
  .recommendCommands()

  // 命令输入错误时的自定义提示
  .fail((err, msg) => {
    console.log(err); // err 相似的命令
    console.log(msg); // msg 输入的命令
  })

  // 输入的命令不能识别时的提示
  .strict()
  // 控制台显示宽度  terminalWidth:当前宽度
  .wrap(cli.terminalWidth())
  .epilogue("页脚显示的内容")

  // 增加一个全局的选项  传入为对象呢
  .options({
    debug: {
      type: "boolean",
      describe: "描述描述描述",
      alias: "d", // 别名
    },
  })
  // 增加一个单独的全局选项
  .option("registry", {
    type: "boolean",
    describe: "描述啦描述",
    hidden: true, // 选项隐藏，作为内部开发时使用
    alias: "r",
  })

  // 将上面定义的选项分组
  .group(["debug"], "单独一个组 dev Options")

  // command 自定义指令(方式一)
  // 第一个参数command命令，[*]定义command后面的参数对应的是哪个option，command init 相当于 command init name
  // 第二个参数command描述
  // 第三个参数是builder（运行之前），定义option
  // 第四个参数是handler，具体执行
  .command(
    "init [name]", // 第一个参数command命令
    "command描述", // 第二个参数command描述

    // 第三个参数是builder
    (yargs) => {
      yargs.option("name", {
        type: "string",
        describe: "a",
      });
    },

    // 第四个参数是handler，具体执行
    (argv) => {
      console.log(argv);
    }
  )

  //  // command 自定义指令(方式二)
  .command({
    command: "list",
    aliases: ["ls", "ll", "la"],
    describe: "描述",
    builder: (yargs) => {
      console.log(yargs);
    },
    handler: (argv) => {
      console.log(argv);
    },
  })
  .parse(argv, context);

```