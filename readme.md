** This project is not under maintaining  ***
# Recomended to use react-query instead
https://github.com/tannerlinsley/react-query


### React-Concurrent A collection of hooks for fetching data easily.

[![NPM](https://nodei.co/npm/react-concurrent.png)](https://nodei.co/npm/react-concurrent/)

[![install size](https://packagephobia.now.sh/badge?p=react-concurrent)](https://packagephobia.now.sh/result?p=react-concurrent) [![dependencies](https://david-dm.org/poolkhord/react-concurrent.svg)](https://david-dm.org/poolkhord/react-concurrent.svg)

## react-concurrent

A collection of hooks for fetching data easily.

## TOC

- [install](#install)
- [useFetching](#useFetching)
- [useFetchingCallback](#useFetchingCallback)
- [useCreateResource](#useCreateResource)
- [createResource](#createResource)
- [React_Concurrent_Mode](#React_Concurrent_Mode)

### install

`$ yarn add react-concurrent`

### useFetching

Get your api data easily by useFetching

```js
import { useFetching } from "react-concurrent";

const app = () => {
  const { data, isLoading, error, refetch } = useFetching(() =>
    fetch("http://example.com").then((res) => res.json()),
  );
};

//////// Axios
// If you are using axios
const { data, isLoading, error, refetch } = useFetching(async () => {
  const response = await axios.get("http://example.com");
  return response.data;
});
```

[useFetching.md](useFetching.md).

### useFetchingCallback

useFetchingCallback doesn't fetching until call refetch

```js
import { useFetchingCallback } from "react-concurrent";

const app = () => {
  const { data, isLoading, error, refetch } = useFetchingCallback((body) =>
    fetch("http://example.com/setting", { method: "post", body }),
  );

  return (
    <>
      <Button onPress={() => refetch({ language: "en" })} title="English" />
      {isLoading ? "Is loading ..." : data}
    </>
  );
};
```

### useCreateResource

useFetch give us a resource, we need to pass that to useResource for get data

```js
import { useCreateResource, useResource } from "react-concurrent";

const fetchApi = (id) => fetch(`http://example.com/${id}`);

const app = () => {
  const [id, setId] = useState(1); // fetch is calling again if this state changed
  const { resource } = useCreateResource(() => fetchApi(id), [id]);

  return <OtherComponent {...{ resource }} />;
};

const OtherComponent = ({ resource }) => {
  const { data, isLoading, error } = useResource(resource);
};
```

### createResource

```js
import { createResource, useResource } from "react-concurrent";

const resource = createResource(() => fetch("http://example.com"));
// resource.preload(); // fetching before render component

const app = () => {
  const { data, isLoading, error } = useResource(resource);
};
```

### React_Concurrent_Mode

As mentioned on react document you could use this

```js
import { createResource } from "react-concurrent";

const resource = createResource(() => fetch("http://example.com"));

const App = () => {
  return (
    <Suspense fallback={"Is loading ...."}>
      <OtherComponent />
    </Suspense>
  );
};

const OtherComponent = () => {
  const data = resource.read();

  return "loaded";
};
```

### Stories

[How to fetch data with React-Concurrent](https://hosseinm-developer.medium.com/how-to-fetch-data-with-react-concurrent-54e1bac3797c?source=friends_link&sk=cb835f9c764e0d43acfdf57eed952b62)
