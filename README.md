# If React Redux was in Java

## A React Component 

### In React

#### Props

```
interface Props {
    name: string;
}
```



#### Component

```
export const Component: React.SFC = (props: Props) => (<div>My name is {props.name}</div>)
```

#### Main
```
ReactDOM.render(
  <Component
    name="Matteo"
  />,
  document.getElementById('root')
);
```


### In Java

#### Props

```
public final class AttributeProps {
    public String name;

    public AttributeProps(String name) {
        this.name = name;
    }
}
```

#### Component
```
public interface ReactComponent {
    String render();
}
```

```
public class Component implements ReactComponent {
    private AttributeProps props;

    private Component(AttributeProps props) {
        this.props = props;
    }

    static Component create(AttributeProps props) {
        return new Component(props);
    }

    @Override
    public String render() {
        return String.format("<div>My name is %s</div>", props.name);
    }
}
```

#### Main
```
class ReactDOM {
    public String render() {
        ReactComponent reactComponent = Component.create(new AttributeProps("Matteo"));
        return reactComponent.render();
    }
}
```

## A Connected React-Redux Component with State 

### In React

#### Props

```
interface AttributeProps {
    name: string;
}

interface StateProps {
    address: string;
}

type Props = AttributeProps & StateProps;
```

#### Component

```
const Component: React.SFC = (props: Props) => (<div>My name is {props.name}, I live in {props.address}</div>)


const mapStateToProps(state: State): StateProps => (
    {
        address: state.address
    }
);

export default connect(mapStateToProps)(Component)

```

#### Main
```
ReactDOM.render(
<Provider store={store}>
  <Component
    name="Matteo"
  />
  </Provider>
  ,
  document.getElementById('root')
);
```

### In Java

#### Props

```
public final class StateProps {
    public String address;

    public StateProps(String address) {
        this.address = address;
    }
}
```
```
public final class AttributeProps {
    public String name;

    public AttributeProps(String name) {
        this.name = name;
    }
}
```

#### Component
```
public interface ReactComponent {
    String render();
}
```

```
public class Component implements ReactComponent {
    private AttributeProps attributeProps;
    private StateProps stateProps;

    private Component() {}

    public static ReactComponent create(AttributeProps attributeProps) {
        return ReactRedux.connect(mapStateToProps(), attributeProps);
    }

    private static StateProps mapStateToProps() {
        return new StateProps("vicolo corto");
    }

    private Component(StateProps stateProps, AttributeProps attributeProps) {
        this.attributeProps = attributeProps;
        this.stateProps = stateProps;
    }

    @Override
    public String render() {
        return String.format("<div>My name is %s, I live in %s</div>", attributeProps.name, stateProps.address);
    }

    private static class ReactRedux {
        static ReactComponent connect(StateProps stateProps, AttributeProps attributeProps) {
            return new Component(stateProps, attributeProps);
        }
    }
}
```

#### Main
```
class ReactDOM {
    public String render() {
        ReactComponent reactComponent = Component.create(new AttributeProps("Matteo"));
        return reactComponent.render();
    }
}
```

## A Connected React-Redux Component with State and Dispatch

### In React

#### Props

```
interface AttributeProps {
    name: string;
}

interface StateProps {
    address: string;
}

interface DispatchProps {
    onClick: () => void;
}

type Props = AttributeProps & StateProps & DispatchProps;
```


#### Component

```
const Component: React.SFC = (props: Props) => (<div>My name is {props.name}, I live in {props.address}</div>)


const mapStateToProps = (state: State): StateProps => (
    {
        address: state.address
    }
);

const mapDispatchToProps = (dispatch: Dispatch<Action>): DispatchProps => (
    {
        onClick: () => {
            dispatch({type: "SOME_ACTION"});
            }
    }
);

export default connect(mapStateToProps, mapDispatchToProps)(Component)

```

#### Main
```
ReactDOM.render(
<Provider store={store}>
  <Component
    name="Matteo"
  />
  </Provider>
  ,
  document.getElementById('root')
);
```

### In Java

#### Props

```
public final class StateProps {
    public String address;

    public StateProps(String address) {
        this.address = address;
    }
}
```
```
public final class AttributeProps {
    public String name;

    public AttributeProps(String name) {
        this.name = name;
    }
}
```
```
public final class DispatchProps {
    public Runnable runnable;

    public DispatchProps(Runnable runnable) {
        this.runnable = runnable;
    }
}
```
```
public interface Runnable {
    void run();
}
```
```
public class Action {
    public final String type = "SOME_ACTION";
}
```

#### Component
```
public interface ReactComponent {
    String render();
}
```

```
public class Component implements ReactComponent {
    private AttributeProps attributeProps;
    private StateProps stateProps;
    private DispatchProps dispatchProps;

    public static ReactComponent create(AttributeProps attributeProps) {
        return Component.ReactRedux.connect(mapStateToProps(), mapDispatchToProps(), attributeProps);
    }

    private static StateProps mapStateToProps() {
        return new StateProps("vicolo corto");
    }

    private static DispatchProps mapDispatchToProps() {
        return new DispatchProps(() -> ReactRedux.dispatch(new Action()));
    }

    private Component(StateProps stateProps, DispatchProps dispatchProps, AttributeProps attributeProps) {
        this.attributeProps = attributeProps;
        this.stateProps = stateProps;
        this.dispatchProps = dispatchProps;
    }

    @Override
    public String render() {
        return String.format("<div onClick=%s >My name is %s, I live in %s</div>", attributeProps.name, stateProps.address, dispatchProps.runnable);
    }

    private static class ReactRedux {
        static void dispatch(Action action) {
        }

        static ReactComponent connect(StateProps stateProps, DispatchProps dispatchProps, AttributeProps attributeProps) {
            return new Component(stateProps, dispatchProps, attributeProps);
        }
    }
}
```

#### Main
```
class ReactDOM {
    public String render() {
        ReactComponent reactComponent = Component.create(new AttributeProps("Matteo"));
        return reactComponent.render();
    }
}
```


