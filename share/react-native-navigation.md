# React Native Navigation

* How to install React Navigation
* How to use
    * Creating a stack navigator
    * Moving between screens & Passing parameters
    * Configuring
    * TabNavigator

## How to install React Navigation
* `yarn add react-navigation`
* Or `npm install --save react-navigation`

## How to use

### Creating a stack navigator
* create Screen(View) Component: Home
* create StackNavigator: NavigationContainer
    * import createStackNavigator
    * create NavigationRouteConfigMap
    * create StackNavigatorConfig
* create App(React.Component)
    * reference Navigator
    * render StackNavigator

```
// home.js
import React from 'react';
import { View, Text } from 'react-native';

export default class HomeScreen extends React.Component {
    render() {
        return (
            <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
                <Text>Home Screen</Text>
            </View>
        );
    }
}
```

```
// navigator.js
import React from 'react';
import { createStackNavigator } from 'react-navigation';
import Home from './home';

const NavigationRouteConfigMap = {
    Home: {
        screen: Home,
        navigationOptions: {
            header: null,
            gesturesEnabled: false,
        },
    },
};

const StackNavigatorConfig = {
    initialRouteName: 'Home',
};

export default createStackNavigator(NavigationRouteConfigMap, StackNavigatorConfig);
```

```
// app.js
import React, { PureComponent } from 'react';
import AppNavigator from './navigator';

export default class App extends PureComponent {
    render() {
        return <AppNavigator />;
    }
}
```

```
// index.js
import {AppRegistry} from 'react-native';
import App from './src/app';
import {name as appName} from './app.json';

AppRegistry.registerComponent(appName, () => App);
```

### Moving between screens & Passing parameters
* create Screen(View) Component: Demo
* add Screen to Navigator
* `this.props.navigation.navigate('Demo', { test: 'hello'})`
* `this.props.navigation.state.params`

```
// demo.js
import React from 'react';
import { View, Text } from 'react-native';

export default class DemoScreen extends React.Component {
    render() {
        return (
            <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
                <Text>Demo Screen</Text>
            </View>
        );
    }
}
```

```
// navigator.js
//...

const NavigationRouteConfigMap = {
    //...
    Demo: {
        screen: Demo,
        navigationOptions: {
            title: 'Demo'
        },
    },
};

//...
```

```
// home.js
import React from 'react';
import { View, Text, Button } from 'react-native';

export default class HomeScreen extends React.Component {
    render() {
        return (
            <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
                <Text>Home Screen</Text>
                <Button 
                    title="Go to Demo"
                    onPress={() => this.props.navigation.navigate('Demo', { test: 'hello' })}
                >
                </Button>
            </View>
        );
    }
}
```

### Configuring

### TabNavigator
* StackNavigatorConfig
    * headerMode
    * initialRouteName
    * mode
    * cardStyle
    * navigationOptions
        * headerTitleAllowFontScaling
        * headerStyle
        * headerLeft
        * headerRight
* NavigationRouteConfigMap
    * screen
    * navigationOptions
        * title
        * left
        * right

### TabNavigator
* create TabNavigator: NavigationContainer
    * create NavigationRouteConfigMap
    * create BottomTabNavigatorConfig
* add TabNavigator to StackNavigator

```
// tab.js
import React from 'react';
import { createBottomTabNavigator } from 'react-navigation';
import Home from './home';
import Demo2 from './demo2';

const NavigatorTab = {
    Home: {
        screen: Home,
        navigationOptions: {
            tabBarLabel: 'Home',
            showIcon: true,
        },
    },
    Demo2: {
        screen: Demo2,
        navigationOptions: {
            tabBarLabel: 'Demo',
            showIcon: true,
        },
    },
};

const TabOptions = {
    animationEnabled: true,
};

export default createBottomTabNavigator(NavigatorTab, TabOptions);
```

```
// navigator.js
// ...

const NavigationRouteConfigMap = {
    // ...
    Tab: {
        screen: Tab,
        navigationOptions: {
            header: null,
            gesturesEnabled: false,
        },
    }
};

const StackNavigatorConfig = {
    initialRouteName: 'Tab',
};

export default createStackNavigator(NavigationRouteConfigMap, StackNavigatorConfig);
```
