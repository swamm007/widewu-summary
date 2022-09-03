# yargs中文翻译

## API相关

* 解析命令行参数为一个对象

```js
require('yargs/yargs')(process.argv.slice(2)).argv
```

* 使用parse做同样的事情。parse不传参数的时候和调用argv 一样，这两个api都应放在最后
```js
// context 参数可以合并到解析过后的argv对象当中
require('yargs/yargs')(process.argv.slice(2)).parse(argv, context)
```
* hideBin 
  * 工具方法等同于 process.argv.slice(2)
* .alias 为一些命令设置别名
* .argv 
  * 将命令行参数解析为普通的对象
  * 没有--标记的命令会被收纳在 argv._数组中
  * 脚本名称会被收纳在argv.$0中
* .array(key) 例如.array('foo') 告诉解析器以数组形式去解析该key值
* .boolean(key)
  * 解析当前key值为boolean类型。如果该key值后面跟随的不是true或者false。将不会把后面跟随的指解析成该key的值
  * 可以使用 ** --no- **来显示设置为false
  * 可以使用数组形式
* check(fn)
  * 检查命令行参数是否符合当前函数的定义
  * 举个例子
   ```js
   const argv = require('yargs/yargs')(process.argv.slice(2))
    .check((argv, options) => {
        const filePaths = argv._
        if (filePaths.length > 1) {
        throw new Error("Only 0 or 1 files may be passed.")
        } else {
        return true // tell Yargs that the arguments passed the check
        }
    })
    .argv
   ```
* .choices(key, choices)
  * 将 key 的有效值限制为一组预定义的选项，可以使用数组或者单个形式
  * 例子
    ```js
    var argv = require('yargs/yargs')(process.argv.slice(2))
    .alias('i', 'ingredient')
    .describe('i', 'choose your sandwich ingredients')
    .choices('i', ['peanut-butter', 'jelly', 'banana', 'pickles'])
    .help('help')
    .argv
    ```
  * choices 同样可以被用在单独的options当中
   ```js
    var argv = require('yargs/yargs')(process.argv.slice(2))
    .option('size', {
        alias: 's',
        describe: 'choose a size',
        choices: ['xs', 's', 'm', 'l', 'xl']
    })
    .argv
   ```
* coerce(key, fn)
  * 提供一个函数来矫正或者转换该key对应的value
  * 举个例子
  ```js
  var argv = require('yargs/yargs')(process.argv.slice(2))
  .coerce('file', function (arg) {
    return await require('fs').promises.readFile(arg, 'utf8')
  })
  .argv


  ------------------------------


  var argv = require('yargs/yargs')(process.argv.slice(2))
  .coerce({
    date: Date.parse,
    json: JSON.parse
  })
  .argv


  -----------------------------


  var path = require('path')
  var argv = require('yargs/yargs')(process.argv.slice(2))
  .coerce(['src', 'dest'], path.resolve)
  .argv

  ----------------------------


  var argv = require('yargs/yargs')(process.argv.slice(2))
  .option('user')
  .coerce('user', opt => {
    opt.name = opt.name.toLowerCase()
    opt.password = '[SECRET]'
    return opt
  })
  .argv


  ```
* .commandDir(directory, [opts])
  * TODO 暂时不知道怎么用
* .command(cmd, desc, [builder], [handler])
  * 核心用法，定义相关命令
  * 例子
  ```js
  yargs
  .command('get', 'make a get HTTP request', {
    url: {
      alias: 'u',
      default: 'http://yargs.js.org/'
    }
  })
  .help()
  .argv

  ------------------------
  // builder也可以是一个函数，用来注册相关描述
  yargs
  .command('get', 'make a get HTTP request', function (yargs, helpOrVersionSet) {
    return yargs.option('url', {
      alias: 'u',
      default: 'http://yargs.js.org/'
    })
  })
  .help()
  .argv

  -----------------------
  // 我们自己的逻辑处理一般都在handler中
  yargs
  .command(
    'get',
    'make a get HTTP request',
    function (yargs) {
      return yargs.option('u', {
        alias: 'url',
        describe: 'the URL to make an HTTP request to'
      })
    },
    function (argv) {
      console.log(argv.url)
    }
  )
  .help()
  .argv
  ```
* .command的高级用法
  * exports.command: 字符串（或字符串数​​组），当在命令行上给出时执行此命令，第一个字符串可能包含位置参数
  * exports.aliases: 表示别名的字符串数组（或单个字符串） ，别名中exports.command定义的位置参数被忽略
  * exports.describe: 用于帮助文本中命令描述的字符串，false用于隐藏命令
  * exports.builder: 声明命令接受的选项的对象，或接受并返回 yargs 实例的函数
  * exports.handler: 一个函数，将传递解析的 argv。
  * exports.deprecated：一个布尔值（或字符串）来显示弃用通知。
  * 举个例子
  ```js
  // my-module.js
  exports.command = 'get <source> [proxy]'

  exports.describe = 'make a get HTTP request'

  exports.builder = {
    banana: {
      default: 'cool'
    },
    batman: {
      default: 'sad'
    }
  }

  exports.handler = function (argv) {
    // do something with argv.
  }
  // 像这样注册模块
  yargs.command(require('my-module'))
  .help()
  .argv
  ```
* .completion暂时未看懂是干嘛的 貌似也用不到的样子
* .config 不知道有什么用途
* extends  想不到使用场景
* .conflicts 想不到使用场景
* .count 想不到使用场景
* 。default(key, value, description)
  * 对于一些命令可能没有设置 option,则默认使用返回的value值
  * 举个例子
  ```js
  var argv = require('yargs/yargs')(process.argv.slice(2))
  .default('random', function randomValue() {
    return Math.random() * 256;
  }).argv;

  .default('timeout', 60000, '(one-minute)')
  ```

* .demandOption(key, msg)
  * 一个命令必须包含的option
  * 举个例子
  ```js
  // demand an array of keys to be provided
    require('yargs/yargs')(process.argv.slice(2))
    .option('run', {
        alias: 'r',
        describe: 'run your program'
    })
    .option('path', {
        alias: 'p',
        describe: 'provide a path to file'
    })
    .option('spec', {
        alias: 's',
        describe: 'program specifications'
    })
    .demandOption(['run', 'path'], 'Please provide both run and path arguments to work with this tool')
    .help()
    .argv
    -------------------------------------------
    // demand individual options within the option constructor
    require('yargs/yargs')(process.argv.slice(2))
    .options({
        'run': {
        alias: 'r',
        describe: 'run your program',
        demandOption: true
        },
        'path': {
        alias: 'p',
        describe: 'provide a path to file',
        demandOption: true
        },
        'spec': {
        alias: 's',
        describe: 'program specifications'
        }
    })
    .help()
    .argv

  ```
* .demandCommand([min=1], [max], [minMsg], [maxMsg])
  * 提示用户必须使用一个命令来执行
  * 举个例子
  ```js
  require('yargs/yargs')(process.argv.slice(2))
  .command({
    command: 'configure <key> [value]',
    aliases: ['config', 'cfg'],
    desc: 'Set a config variable',
    builder: (yargs) => yargs.default('value', 'true'),
    handler: (argv) => {
      console.log(`setting ${argv.key} to ${argv.value}`)
    }
  })
  // provide a minimum demand and a minimum demand message
  .demandCommand(1, 'You need at least one command before moving on')
  .help()
  .argv
  ```
* .describe(key, desc)
  * 对命令的描述
* .hide(key)
  * 隐藏一些内部使用的命令
  * 使用 --show-hidden来显示隐藏命令

* .epilogue(结束语)
  * 脚手架工具的一些结束语

* .example([[cmd1, desc1], [cmd2, desc2], ...])
  * 给使用者的一些提示例子
* .exitProcess(false) 关闭yargs 的发生错误默认退出的逻辑
* .option(key, [opt])  .options(key, [opt])
  *  此方法用于定义脚手架当中存在的一些option。您还可以传递一个 opt 对象，该对象可以进行进一步的自定义
* .parse 解析args
* .positional(key, opt)
  * 允许在 .command中的builder中定义options,不能在顶层api中使用
* .recommendCommands()
  * 默认检测一些命令并推荐给使用折
* .scriptName($0)
  * 改变脚本定义的默认$0
* .strict()
  * 任何不需要的或没有相应描述的命令行参数都将被报告为错误。 无法识别的命令也将被报告为错误
* .usage
  * 告诉使用者脚手架的用途
* .wrap(columns)
  * 格式化输出命令
  * 默认情况下，wrap 将设置为 Math.min(80, windowWidth)。使用 .wrap(null) 指定无列限制（无右对齐）。使用 .wrap(yargs.terminalWidth()) 最大化 yargs 使用说明的宽度。
