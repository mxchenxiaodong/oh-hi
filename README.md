# Welcome to [Slidev](https://github.com/slidevjs/slidev)!

To start the slide show:

- `npm install`
- `npm run dev`
- visit http://localhost:3030

Learn more about Slidev on [documentations](https://sli.dev/).

## 操作提示

```bash
npx slidev --help

命令：
  slidev [entry]             Start a local server for Slidev            [默认值]
  slidev build [entry]       Build hostable SPA
  slidev export [entry]      Export slides to PDF
```

注意打包是的一些参数：

```bash
npx slidev build 'markdowns/miniweb.md' --base="/slidev/docs/miniweb" --out='docs/miniweb'

npx slidev build 'markdowns/learn_ruby.md' --base="/slidev/docs/learn_ruby" --out='docs/learn_ruby'
```

具体命令行参数，可以查看 slidev 源码的 `packages/slidev/node/cli.ts`

## 例子

比如，现在如果需要新增一个样式文稿，只需要在 `markdowns` 文件新增一个 `markdown` 文件，假如叫做（`some_example.md`） 。

然后就可以启动预览：`npx slidev 'markdowns/some_example.md'` 。

## 其他配置

如有需要，可以在 `vite.config.ts` 设置。
