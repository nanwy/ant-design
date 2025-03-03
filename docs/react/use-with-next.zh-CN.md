---
group:
  title: 如何使用
order: 2
title: 在 Next.js 中使用
---

[Next.js](https://nextjs.org/) 是目前世界上最流行的 React 服务端同构框架，本文会尝试在 Next.js 创建的工程中使用 `antd` 组件。

## 安装和初始化

在开始之前，你可能需要安装 [yarn](https://github.com/yarnpkg/yarn/) 或者 [pnpm](https://pnpm.io/zh/)。

<InstallDependencies npm='$ npx create-next-app antd-demo' yarn='$ yarn create next-app antd-demo' pnpm='$ pnpm create next-app antd-demo'></InstallDependencies>

工具会自动初始化一个脚手架并安装项目的各种必要依赖，在安装过程中，有一些配置项需要自行选择，如果在过程中出现网络问题，请尝试配置代理，或使用其他 npm registry。

初始化完成后，我们进入项目并启动。

```bash
$ cd antd-demo
$ npm run dev
```

此时使用浏览器访问 http://localhost:3000/ ，看到 NEXT 的 logo 就算成功了。

## 引入 antd

现在从 yarn 或 npm 或 pnpm 安装并引入 antd。

<InstallDependencies npm='$ npm install antd --save' yarn='$ yarn add antd' pnpm='$ pnpm install antd --save'></InstallDependencies>

修改 `src/app/page.tsx`，引入 antd 的按钮组件。

```tsx
'use client'; // 如果是在 Pages Router 中使用，则不需要添加 "use client"

import React from 'react';
import { Button } from 'antd';

const Home = () => (
  <div className="App">
    <Button type="primary">Button</Button>
  </div>
);

export default Home;
```

好了，现在你应该能看到页面上已经有了 `antd` 的蓝色按钮组件，接下来就可以继续选用其他组件开发应用了。其他开发流程你可以参考 Next.js 的[官方文档](https://nextjs.org/)。

我们现在已经把 antd 组件成功运行起来了，开始开发你的应用吧！

## 使用 App Router

如果你在 Next.js 当中使用了 App Router, 并使用 antd 作为页面组件库，为了让 antd 组件库在你的 Next.js 应用中能够更好的工作，提供更好的用户体验，你可以尝试使用下面的方式将 antd 首屏样式按需抽离并植入到 HTML 中，以避免页面闪动的情况。

1. 安装 `@ant-design/cssinjs`

> 开发者注意事项：
>
> 请注意，安装 `@ant-design/cssinjs` 时必须确保版本号跟 `antd` 本地的 `node_modules` 中的 `@ant-design/cssinjs` 版本保持一致，否则会出现多个 React 实例，导致无法正确的读取 ctx。（Tips: 你可以通过 `npm ls @ant-design/cssinjs` 命令查看本地版本）

<InstallDependencies npm='$ npm install @ant-design/cssinjs --save' yarn='$ yarn add @ant-design/cssinjs' pnpm='$ pnpm install @ant-design/cssinjs --save'></InstallDependencies>

2. 创建 `lib/AntdRegistry.tsx`

```tsx
'use client';

import React from 'react';
import { createCache, extractStyle, StyleProvider } from '@ant-design/cssinjs';
import type Entity from '@ant-design/cssinjs/es/Cache';
import { useServerInsertedHTML } from 'next/navigation';

const StyledComponentsRegistry = ({ children }: React.PropsWithChildren) => {
  const cache = React.useMemo<Entity>(() => createCache(), []);
  const isServerInserted = React.useRef<boolean>(false);
  useServerInsertedHTML(() => {
    // 避免 css 重复插入
    if (isServerInserted.current) {
      return;
    }
    isServerInserted.current = true;
    return <style id="antd" dangerouslySetInnerHTML={{ __html: extractStyle(cache, true) }} />;
  });
  return <StyleProvider cache={cache}>{children}</StyleProvider>;
};

export default StyledComponentsRegistry;
```

3. 在 `app/layout.tsx` 中使用

```tsx
import React from 'react';
import { Inter } from 'next/font/google';

import StyledComponentsRegistry from '../lib/AntdRegistry';

import '@/globals.css';

const inter = Inter({ subsets: ['latin'] });

export const metadata = {
  title: 'Create Next App',
  description: 'Generated by create next app',
};

const RootLayout = ({ children }: React.PropsWithChildren) => (
  <html lang="en">
    <body className={inter.className}>
      <StyledComponentsRegistry>{children}</StyledComponentsRegistry>
    </body>
  </html>
);

export default RootLayout;
```

4. 在 `theme/*.ts` 中自定义主题配置

```ts
// theme/themeConfig.ts
import type { ThemeConfig } from 'antd';

const theme: ThemeConfig = {
  token: {
    fontSize: 16,
    colorPrimary: '#52c41a',
  },
};

export default theme;
```

5. 在页面中使用

```tsx
import React from 'react';
import { Button, ConfigProvider } from 'antd';

import theme from './theme/themeConfig';

const HomePage = () => (
  <ConfigProvider theme={theme}>
    <div className="App">
      <Button type="primary">Button</Button>
    </div>
  </ConfigProvider>
);

export default HomePage;
```

> 注意: 上述方式没有在页面中使用类似 `<Select.Option />`、`<Typography.Text />` 等子组件，因此可以正常使用。但如果你的页面中有使用类似这样的子组件，目前在 Next.js 中会看到如下警告：`Error: Cannot access .Option on the server. You cannot dot into a client module from a server component. You can only pass the imported name through.`，目前需等待 Next.js 官方解决。在此之前，如果你的页面中使用了上述子组件，可在页面组件第一行加上 `"use client"` 来避免警告。更多细节可以参考示例：[with-sub-components](https://github.com/ant-design/ant-design-examples/blob/main/examples/with-nextjs-app-router-inline-style/src/app/with-sub-components/page.tsx)。

更多详细的细节可以参考 [with-nextjs-app-router-inline-style](https://github.com/ant-design/ant-design-examples/tree/main/examples/with-nextjs-app-router-inline-style)。

## 使用 Pages Router

如果你在 Next.js 当中使用了 Pages Router, 并使用 antd 作为页面组件库，为了让 antd 组件库在你的 Next.js 应用中能够更好的工作，提供更好的用户体验，你可以尝试使用下面的方式将 antd 首屏样式按需抽离并植入到 HTML 中，以避免页面闪动的情况。

1. 安装 `@ant-design/cssinjs`

> 开发者注意事项：
>
> 请注意，安装 `@ant-design/cssinjs` 时必须确保版本号跟 `antd` 本地的 `node_modules` 中的 `@ant-design/cssinjs` 版本保持一致，否则会出现多个 React 实例，导致无法正确的读取 ctx。（Tips: 你可以通过 `npm ls @ant-design/cssinjs` 命令查看本地版本）

<InstallDependencies npm='$ npm install @ant-design/cssinjs --save' yarn='$ yarn add @ant-design/cssinjs' pnpm='$ pnpm install @ant-design/cssinjs --save'></InstallDependencies>

2. 改写 `pages/_document.tsx`

```tsx
import React from 'react';
import { createCache, extractStyle, StyleProvider } from '@ant-design/cssinjs';
import Document, { Head, Html, Main, NextScript } from 'next/document';
import type { DocumentContext } from 'next/document';

const MyDocument = () => (
  <Html lang="en">
    <Head />
    <body>
      <Main />
      <NextScript />
    </body>
  </Html>
);

MyDocument.getInitialProps = async (ctx: DocumentContext) => {
  const cache = createCache();
  const originalRenderPage = ctx.renderPage;
  ctx.renderPage = () =>
    originalRenderPage({
      enhanceApp: (App) => (props) => (
        <StyleProvider cache={cache}>
          <App {...props} />
        </StyleProvider>
      ),
    });

  const initialProps = await Document.getInitialProps(ctx);
  const style = extractStyle(cache, true);
  return {
    ...initialProps,
    styles: (
      <>
        {initialProps.styles}
        <style dangerouslySetInnerHTML={{ __html: style }} />
      </>
    ),
  };
};

export default MyDocument;
```

3. 支持自定义主题

```ts
// theme/themeConfig.ts
import type { ThemeConfig } from 'antd';

const theme: ThemeConfig = {
  token: {
    fontSize: 16,
    colorPrimary: '#52c41a',
  },
};

export default theme;
```

4. 改写 `pages/_app.tsx`

```tsx
import React from 'react';
import { ConfigProvider } from 'antd';
import type { AppProps } from 'next/app';

import theme from './theme/themeConfig';

const App = ({ Component, pageProps }: AppProps) => (
  <ConfigProvider theme={theme}>
    <Component {...pageProps} />
  </ConfigProvider>
);

export default App;
```

5. 在页面中使用 antd

```tsx
import React from 'react';
import { Button } from 'antd';

const Home = () => (
  <div className="App">
    <Button type="primary">Button</Button>
  </div>
);

export default Home;
```

更多详细的细节可以参考 [with-nextjs-inline-style](https://github.com/ant-design/ant-design-examples/tree/main/examples/with-nextjs-inline-style)。
