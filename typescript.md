### Stateless React component

```
const Component: React.SFC<any> = ({ prop1 }) => {
    return (
        <div className="style">
            {prop1}
        </div>
    )
}

export default Component
```

### Class-based React component
```
interface IProps {
    something: string,
}

interface IState {
    somethingelse: string,
}

class App extends Component<IProps, IState> {
    constructor(props: any) {
        super(props)

        this.state = {
            somethingelse: '',
        }
    }
    
    render() {
        ...
    }
    
    export default App
}
```

### Return type of React class which returns an array of React components
```
const CardList: React.SFC<any> = ({ robots }): ReactElement<any> => {
    return (
        robots.map((robot: IRobot) => 
            <Card 
                key={robot.id} 
                name={robot.name} 
                email={robot.email} 
            />
        )
    )
}

export default CardList
```
