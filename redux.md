### Nice way to console.log all the data flow

Install redux-logger and then:
```
import { createStore, applyMiddleware } from 'redux'
import { createLogger } from 'redux-logger'

const logger = createLogger()
const store = createStore(searchRobots, applyMiddleware(logger))
```
