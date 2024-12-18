- jest.fn
- jest.spyOn
- jest.mock

Sugar syntax over the .mock property
- https://jestjs.io/docs/mock-functions#custom-matchers
- https://jestjs.io/docs/mock-functions#mock-property

# Function mocks

### Mock func

```javascript
const mockFn = jest.fn();

mockFn.mockClear();
mockFn.mockReset();
```

### Mock object func

```javascript
const spyMockFn = jest.spyOn(object, 'greet').mockReturnValue('Hi!');

spyMockFn.mockClear();
spyMockFn.mockReset();
spyMockFn.mockRestore(); // Supported only by jest.spyOn
```
- Spies on an existing function, keeping the original implementation that can be restore.
- All other methods and properties remain unchanged.

# Async function mocks

### Mock async func

```javascript
const asyncMockFn = jest.fn().mockResolvedValue(43);
```

### Mock object async func

```javascript
const asyncSpyMockFn = jest.spyOn(object, 'greet').mockResolvedValue('Hi!');
```

# Module mocks

- https://jestjs.io/docs/es6-class-mocks#the-4-ways-to-create-an-es6-class-mock

Types
- Automatic mocks
- Manual mocks
- Mock factory
- Mock Replacing

### Automatic module mock

```javascript
import axios from 'axios';

jest.mock('axios');

axios.get.mockResolvedValue({ mockedResult: 'ok' });
```
Or
```javascript
import math from "./math";

jest.mock('./math.js')
```
- Module methods are automatically mocked with `jest.fn()`.
- Module `properties(NOT methods)` and their values are removed. The properties must be explicitly defined or mocked if needed.
  - ```javascript
    // Example of moking the property with custom data
    axios.defaults = {
      baseURL: 'https://mockapi.example.com',
    };
    ```
  - ```javascript
    // Example of moking the property with the module original data
    axios.defaults = jest.requireActual('axios').defaults;
    ```
#### Mock module partionaly

https://jestjs.io/docs/mock-functions#mocking-partials

### Manual module mock

- Provide your own custom implementation of a module in a __mocks__ directory.

Example `__mocks__/math.js` file
```javascript
module.exports = { add: jest.fn(() => 5) };
```

Usage
```javascript
jest.mock('./math'); // Automatically uses the manual mock
const { add } = require('./math');
expect(add()).toBe(5);
```

### Factory module mock

```javascript
// const config = require('config') // this import not needed with factory mock

jest.mock('config', () => ({
  mysql: {
    hostname: 'testhostname'
  }
}));
```

# Class mocks

- The same as module mocks, but with classes
```javascript
jest.mock('./User');
const User = require('./User');

const mockUserInstance = new User();
mockUserInstance.sayHello.mockReturnValue('Hi!');
expect(mockUserInstance.sayHello()).toBe('Hi!');
```
