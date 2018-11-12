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
