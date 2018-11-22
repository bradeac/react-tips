### Stateless React component

```
interface IProps {
    field: string,
}

const Component: React.SFC<IProps> = ({ field }: IProps): JSX.Element => {
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
    constructor(props: IProps) {
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
interface CardListProps {
    robots: Array<IRobot>,
}

interface IRobot {
    name: string,
    email string,
}

const CardList: React.SFC<CardListProps> = ({ robots }): JSX.Element => {
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

Component which returns children. Example of 'type' usage instead of 'interface'
```
import * as React from 'react';

type Props = {
  children: JSX.Element
}

const Scroll = (props: Props) => {
  return (
    <div>
      {props.children}
    </div>
  );
};

export default Scroll;
```
