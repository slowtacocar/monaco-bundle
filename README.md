# monaco-bundle

Monaco is a fantastic code editor, but its maintainers for some reason failed to bundle the code for browsers. This just compiles Monaco using esbuild and publishes it to npm so it can be easily used with things like React and Next.js.

To use, just `npm i monaco-bundle` and import `monaco-bundle` instead of `monaco-editor`. You will also want to include the css and web workers, as shown below. Here is an example:

```
import * as monaco from 'monaco-bundle';
import 'monaco-bundle/editor/editor.main.css';

self.MonacoEnvironment = {
  getWorkerUrl(moduleId, label) {
    if (label === 'json') {
      return new URL('monaco-bundle/language/json/json.worker.js', import.meta.url).toString();
    }
    if (label === 'css' || label === 'scss' || label === 'less') {
      return new URL('monaco-bundle/language/css/css.worker.js', import.meta.url).toString();
    }
    if (label === 'html' || label === 'handlebars' || label === 'razor') {
      return new URL('monaco-bundle/language/html/html.worker.js', import.meta.url).toString();
    }
    if (label === 'typescript' || label === 'javascript') {
      return new URL('monaco-bundle/language/typescript/ts.worker.js', import.meta.url).toString();
    }

    return new URL('monaco-bundle/editor/editor.worker.js', import.meta.url).toString();
  }
};

monaco.editor.create(..., {
  ...
})
```

Note--for Next.js, you'll need to dynamically import components that import monaco. This is because monaco cannot be rendered server-side. Example:

```
import dynamic from 'next/dynamic'

const CodeEditor = dynamic(() => import('../components/editor'), {
  ssr: false,
})
```
