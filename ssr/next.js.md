# Next.js

Setup to make SSR work with [Next.js](https://nextjs.org).

```
yarn add @emotion/server
```

{% tabs %}
{% tab title="With MUI" %}
`pages/_document.tsx`

```tsx
import BaseDocument from "next/document";
import { withEmotionCache } from "tss-react/nextJs";
import { createMuiCache } from "./index";

export default withEmotionCache({
    //If you have a custom document pass it instead
    "Document": BaseDocument,
    //Every emotion cache used in the app should be provided.
    //Caches for MUI should use "prepend": true.
    "getCaches": ()=> [ createMuiCache() ]
});
```

`page/index.tsx`

```tsx
import type { EmotionCache } from "@emotion/cache";
import { CacheProvider } from "@emotion/react";
import createCache from "@emotion/cache";

let muiCache: EmotionCache | undefined = undefined;

export const createMuiCache = () =>
    muiCache = createCache({
        "key": "mui",
        "prepend": true,
    });

export default function Index() {
    return (
        <CacheProvider value={muiCache ?? createMuiCache()}>
            {/* Your app  */}
        </CacheProvider>
    );
}
```

You can find a working example [here](https://github.com/garronej/tss-react/tree/main/src/test/apps/ssr).

{% hint style="info" %}
This setup is merely a suggestion. Feel free, for example, to move the `<CacheProvider/>` into `pages/_app.tsx`.&#x20;

What's important to remember however is that new instances of the caches should be created **for each render**`!`
{% endhint %}
{% endtab %}

{% tab title="Without MUI" %}
`pages/_document.tsx`

```tsx
import BaseDocument from "next/document";
import { withEmotionCache } from "tss-react/nextJs";
import { createMuiCache } from "./index";

export default withEmotionCache({
    /** If you have a custom document pass it instead */,
    "Document": BaseDocument
});
```

{% hint style="warning" %}
`If you use` \<TssCacheProvider/> `or` \<CacheProvider/> `anywhere in your app you must provide a getCache function to withEmotionCache.` &#x20;

What's important to remember however is that new instances of the caches should be created **for each render**`!`

`You can get inspiration on how to do it under the`` `_`With MUI`_` ``tab.`
{% endhint %}
{% endtab %}
{% endtabs %}

