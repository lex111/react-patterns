# Function as child component

A Function as Child Component is a pattern that lets you pass a render function to a component as the children prop, so you can change what you can pass as children to a component.

When you use a Function as child component, instead of passing JSX markup, you assign children as a function.

```
<Foo>
  {(name) => <div>`hello from ${name}`</div>}
</Foo>
```

And the `Foo` component would look something like this.

```
const Foo = ({ children }) => {
  return children('foo');
};
```

Now let take a look at a advanced example of a Function as child component.

```
class PageWidth extends React.Component {
  state = { width: 0 };

  componentDidMount() {
    this.setState({ width: window.innerWidth });

    window.addEventListener(
      'resize',
      ({ target }) => {
        this.setState({ width: target.innerWidth });
      }
    );
  }

  render() {
    const { width } = this.state;

    return this.props.children(width);
  }
}
```

And then you could use it as.

```
<PageWidth>
  {width => <div>Page width is {width}</div>}
</PageWidth>

As you can see above, children is "overloaded" and passed to PageWidth as a function instead of being a ReactNodeList as nature intended. The PageWidth component’s render method calls this.props.children (passing it width), which returns rendered JSX.

The real power of render callbacks can be seen in this example. PageWidth will do all of the heavy lifting, while exactly what is rendered can change, depending on the render function that is passed.