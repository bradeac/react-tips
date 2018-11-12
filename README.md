# react-tips
Just useful and important bits of React that I learnt over time. Also may include Typescript related stuff.

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

