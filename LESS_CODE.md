# Less code

Here's an example of how `subscribeWhileMounted` reduces the amount of boilerplate, allowing you to write less, more readable code to subscribe to the store.

Assume a component `SomeComponent` wants to update upon changes to `foo` in the store state. With vanilla Redux you need about 15-17 lines of code spread over three chunks. With `subscribeWhileMounted` you can do the same with just three lines of code, in one chunk. See the examples below.


### Vanilla Redux, simple approach (17 lines)

```js
class SomeComponent extends React.Component {

    state = {
        foo: store.getState().foo
    }

    componentWillMount() {
        this.unsubscribe = store.subscribe(() => {
            const {foo} = store.getState();

            if (foo !== this.state.foo) {
                this.setState({ foo });
            }
        });
    }

    componentWillUnmount() {
        this.unsubscribe();
    }

    ...
}
```

### Vanilla Redux, DRY approach (15 lines)

```js
class SomeComponent extends React.Component {

    updateState = () => {
        const {foo} = store.getState();

        if (!this.state || this.state.foo !== foo) {
            this.setState({ foo });
        }
    }

    componentWillMount() {
        this.unsubscribe = store.subscribe(this.updateState);
    }

    componentWillUnmount() {
        this.unsubscribe();
    }

    ...
}
```

### `subscribeWhileMounted` (3 lines)

```js
class SomeComponent extends React.Component {

    componentWillMount() {
        store.subscribeWhileMounted(this, 'foo');
    }

    ...
}
```
