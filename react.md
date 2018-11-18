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
	loader: () => import ('./Home'),
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
	loader: () => import ('./Dashboard'),
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
	loader: () => import ('./About'),
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
