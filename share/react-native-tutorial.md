# React Native 极速指南

* How to install React Native
* ABC
    * Hello React Navite
    * Props
    * State
    * Style
    * Flex
    * Handling Text Input
    * ScrollView
    * FlatList and SectionList
    * Network

## How to install React Native

* [development environment refer to](./react-native-environment.md)
* `brew install node`
* `brew install watchman`
* `npm install -g react-native-cli`
* `react-native init ReactNativeTuroral`
* `cd ReactNativeTuroral`
* `react-native run-ios --port 8089` Or `react-native run-android --port 8089`

## ABC

### Hello React Navite

```
// hello-world.js
import React, { Component } from 'react';
import { Text, View } from 'react-native';

const styles = {
    container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
    }
};

export default class HelloWorld extends Component {
    render() {
        return (
            <View style={styles.container}>
                <Text>
                    Hello World!
                </Text>
            </View>
        );
    }
}
```

### Props

* Component's property
* `<Com name={'abc'}></Com>`

```
// my-component.js
import React, { Component } from 'react';
import { Text } from 'react-native';

export default class MyComponent extends Component {

    render() {
        return (
            <Text>Hello {this.props.text}</Text>
        )
    }
}
```

```
// hello-world.js
//...
import MyComponent from './my-component';

//...

export default class HelloWorld extends Component {
    render() {
        return (
            <View style={styles.container}>
                <Text>
                    Hello World!
                </Text>
                <MyComponent text="My Component"></MyComponent>
            </View>
        );
    }
}
```

### State

* Must use `setState()` change state;
* When `setState()`, UI will re-render
* `setState()` is async

```
// my-state.js
import React, { Component } from 'react';
import { Text, View } from 'react-native';

export default class MyState extends Component {

    state = {
        message: '',
    };

    componentDidMount() {
        setTimeout(() => {
            this.setState({
                message: 'Hello State!'
            });
        }, 1000)
    }

    render() {
        return (
            <View>
                <Text>
                    {this.state.message}
                </Text>
            </View>
        );
    }
}

```

### Style

* Camel casing CSS: `{ backgroundColor: '#fff' }`;
* Use `StyleSheet.create`: `const styles = StyleSheet.create({...})`;
* Multiple style Use Array: `style={[style1, style2]}`;

### Flex

* flexDirection: 'column'
* justifyContent: 
* alignItems: 
* [Refer to](https://github.com/lijiakof/frontend-book/blob/master/share/cssboxmodel-vs-flexbox.md)

### Handling Text Input

* import `TextInput` Component
* `onChangeText` Event
* `setState`

```
// my-input.js
import React, { Component } from 'react';
import { Text, View, TextInput } from 'react-native';

export default class MyInput extends Component {

    state = {
        text: '',
    };

    render() {
        return (
            <View>
                <TextInput onChangeText={(text) => this.setState({ text })}></TextInput>
                <Text>{this.state.text}</Text>
            </View>
        );
    }
}


```

### ScrollView

* import `ScrollView` Component;
* Can scroll both vertically and horizontally, by setting `horizontal` property;



### FlatList and SectionList
### Network





