### Project structure
Create 'components' and 'containers' folders in 'src' and place stateless components
in 'components' and stateful components in 'containers'

### How to use async / await with fetch (no more .then())

```
const response = await fetch('url')  
const data = await response.json()
```

### Create a Scrollable component that wraps a list / grid

Tip: if the list / grid uses a grid layout, it doesn't display nicely.

```
const Scrollable: React.SFC<any> = ({ children }) => {
    return (
        <div style={style}>
            {children}
        </div>
    )
}

const style = {
    overflowY: 'scroll', 
    border: '5px solid black',
    height: '500px',
}

export default Scrollable
```

### Search textbox with React and Typescript

Search component:
```
const SearchBox: React.SFC<any> = ({ searchChange }) => {
    return (
        <div className="">
            <input 
                type="search" 
                placeholder="Search ..."
                onChange={searchChange}
            />
        </div>
    )
}

export default SearchBox
```

Parent component:
```
interface IProps {

}

interface IState {
    robots: IRobot[],
    searchField: string,
}

class App extends Component<IProps, IState> {
    constructor(props: any) {
        super(props)

        this.state = {
            robots: [],
            searchField: '',
        }
    }

    async componentDidMount() {
        const robots = // fetched data
        
        this.setState({ robots: robots })
    }

    onSearchChange = (event: any) => {
        this.setState({ searchField: event.target.value })
    }

    render() {
        const filteredRobots = this.state.robots.filter((robot: IRobot)=>
            robot.name.toLowerCase().includes(this.state.searchField.toLowerCase())
        )

        return (
                <SearchBox searchChange={this.onSearchChange}/>
                <div className="main">
                    <CardList robots={filteredRobots} />
                </div>
            </>
        )
    }   
}

export default App
```

### Route-based code-splitting (vanilla)

AsyncComponent:
```
import React from 'react'

export default function asyncComponent(importComponent) {
    class AsyncComponent extends React.Component {
        constructor(props) {
            super(props)

            this.state = {
                component: null
            }
        }

        async componentDidMount() {
            const { default: component } = await importComponent()
            
            this.setState({ component })        
        }

        render() {
            const Component = this.state.component

            return Component ? <Component {...this.props} /> : null
        }
    }

    return AsyncComponent
}
```

Main.js:
```
import React from 'react'
import { Switch, Route } from 'react-router-dom'

import AsyncComponent from './AsyncComponent'

const AsyncHome = AsyncComponent(() => import ('./Home'))
const AsyncDashboard = AsyncComponent(() => import ('./Dashboard'))
const AsyncAbout = AsyncComponent(() => import ('./About'))

const Main = () => (
  <main>
    <Switch>
      <Route exact path='/' component={AsyncHome}/>
      <Route path='/dashboard' component={AsyncDashboard}/>
      <Route path='/about' component={AsyncAbout}/>
    </Switch>
  </main>
)

export default Main

```

### Route-based code-splitting (react-loadable)

Loadable.js
```
import ReactLoadable from 'react-loadable'

const Loadable = opts =>
    ReactLoadable({
        delay: 300,
        ...opts
    })

export default Loadable
```

Main.js:
```
import React from 'react'
import { Switch, Route } from 'react-router-dom'

import Loadable from './Loadable'

const AsyncHome = Loadable({
	loader: () => import (/* webpackChunkName: "home" */ './Home'), // this names the file home.chunk.js
	loading(props) {
		if (props.error) {
			return <div>Error !</div>
		}

		if (props.pastDelay) {
			return <div>Loading ...</div>
		}

		return null
	}
})

const AsyncDashboard = Loadable({
	loader: () => import (/* webpackChunkName: "dashboard" */ './Dashboard'),
	loading(props) {
		if (props.error) {
			return <div>Error !</div>
		}

		if (props.pastDelay) {
			return <div>Loading ...</div>
		}

		return null
	}
})

const AsyncAbout = Loadable({
	loader: () => import (/* webpackChunkName: "about" */ './About'),
	loading(props) {
		if (props.error) {
			return <div>Error !</div>
		}

		if (props.pastDelay) {
			return <div>Loading ...</div>
		}

		return null
	}
})

const Main = () => (
	<main>
		<Switch>
			<Route exact path='/' component={AsyncHome}/>
			<Route path='/dashboard' component={AsyncDashboard}/>
			<Route path='/about' component={AsyncAbout}/>
		</Switch>
	</main>
)

export default Main

```

### Code-splitting - fetch and run js only when component is visible on screen (react-loadable-visibility)

Main.js:
```
import LoadableVisibility from 'react-loadable-visibility/react-loadable'

const AsyncFooter = LoadableVisibility({
	loader: () => import (/* webpackChunkName: "footer" */ './Footer'),
	loading(props) {
		if (props.error) {
			return <div>Error !</div>
		}

		if (props.pastDelay) {
			return <div>Loading ...</div>
		}

		return null
	}
})
```

### Component-based code-splitting (react-loadable)

ComponentSplitting.js (Label.js is a simple stateless component):
```
import React from 'react'

import Loadable from './Loadable'

class ComponentSplitting extends React.Component {
    constructor(props) {
        super(props)

        this.state = {
            clicked: false
        }
    }

    onHandleClick = () => {
        this.setState((prevState) => ({ clicked: !prevState.clicked }))
    }

    render() {
        const { clicked } = this.state
        const AsyncComponentSplitting = Loadable({
            loader: () => import (/* webpackChunkName: "label" */ './Label'),
            loading(props) {
                if (props.error) {
                    return <div>Error !</div>
                }
        
                if (props.pastDelay) {
                    return <div>Loading ...</div>
                }
        
                return null
            }
        })

        return (
            <>
                <button onClick={this.onHandleClick}>Click me ...</button>
                {clicked && <AsyncComponentSplitting />}
            </>
        )
    }
}

export default ComponentSplitting
```

setState has a callback function:
```
this.setState((prevState) => {
		return ({ counter: this.state.counter + 1 })
	},
	() => console.log('callback' + this.state.counter)
)
```


Context API with HOC
contextWrapper:
```
function contextWrapper(WrappedComponent, Context) {
  return class extends React.Component {
    render() {
      return (
        <Context.Consumer>
          { context => <WrappedComponent context={context} { ...this.props } /> }
        </Context.Consumer>
      )
    }
  }
}
```

Stateless child component:
```
class Child extends React.Component {
  render() {
    console.log(this.props.context)
    return <div>Child</div>
  }
}

const ChildWithContext = contextWrapper(Child, AppContext)
```

How to change context:
```
function contextProviderWrapper(WrappedComponent, Context, initialContext) {
  return class extends React.Component {
    constructor(props) {
      super(props)
      this.state = { ...initialContext }
    }
    
    // define any state changers
    changeContext = () => {
      this.setState({ foo: 'baz' })
    }

    render() {
      return (
        <Context.Provider value={{
          ...this.state,
          changeContext: this.changeContext
        }} >
          <WrappedComponent />
        </Context.Provider>
      )
    }
  }
}
```

