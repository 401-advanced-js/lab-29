# Testing
## The form component
```js
import React from 'react';
import App from '../app.js';

describe('<App />', () => {
  it('is alive at application start', () => {
    let app = shallow(<App /> );
    expect(app.find('h1').exists()).toBeTruthy);
  })

  it('toggles state.stuff on a click', () => {
    let app = mount(<App />);
    let form = app.find('form');

    let petfield = app.find('input[name="pet"]');
    petField.simulate('change', {target:{name:'pet',value:'Rosie'}})

    form.simulate('submit');
    expect(app.state().formData.pet).toEqual('Rosie');
  })
})
```

# State

## App.js
```js
import React from 'react';

import Counter from './counter/counter.js';
import Form from 'FORM_PATH';

class App extends React.Component{
  constructor(props){
    super(props)
    state = {
      count = 0,
      names:{
        people:['Spence','Spen','Spook','Paige']
      },
      formData: {}
    }
  }
  
  goDown = () => {
    this.setState({count: this.state.count - 1});
    this.props.dinger();
  }
  goUp = () => {
    this.setState({count: this.state.count + 1});
    this.props.dinger();
  };

  ringTheBell = () => {
    alert('ding');
  };

  getFormData = (formData) => {
    this.setState({formData});
  };

  componentDidMount(){
    fetch('https://swapi.co/api/people');
      .then( results => results.json() )
      .then( list => this.setState({swapi:list.results}));

  }

  render() {
    console.log('Render...');
    let names = this.state.names && this.state.names.people && Array.isArray(this.state.names.people) && this.state.names.people || [];
    return 
      <>
        <Counter 
          title="Button"
          goDown={this.goDown} 
          goUp={this.goUp} 
          currentCount={this.state.count};
        />
        {If condition={names.length>2}>
          <ul>
          // This lists all the names
            {
              names.map((item,idx) => {
                <li key={idx}>{item}</li>
              })
            }
          </ul>
        </If>
        <ul>
          // This lists all the names
            {this.state.swapi && this.state.swapi.map((person,idx) => (
              <li key={idx}>{person.name}</li>
            ))
            }
          </ul>
        <h1>Form</h1>
        <Form handler={this.getFormData}/>

      </>
  }
```
## Counter.js
```js
import React from 'react';

const Counter = (props) {

    render(){
      return (
        <>
         
          <div>
            <h2>{props.title}</h2>
            <h2>{props.count}</h2>
            <button onClick={props.goDown}>-</button>
            <button onClick={props.goUp}>+</button>
          </div>
        </>
      )
    }
}
export default Counter;
```

---

## Forms
```js
import React from 'react';

import Button from 'PATH';
class Form extends React.Component{

  constructor(props){
    super(props);
    this.state = {}
  }

  handleInput = (e) => {
    this.setState({ [e.target.name]: e.target.value })
  }

  handleSubmit = e => {
    e.preventDefault();
    this.props.handler(this.state);
  };

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input 
          onChange={this.handleInput} 
          name="person" 
          placeholder="Type Your Name" 
        />
        <input 
          onChange={this.handleInput} 
          name="Pet" 
          placeholder="Type Your pet name" 
        />
        <select onChange={this.handleInput} name="icecream">
          <option />
          <option value="chocolate">Chocolate</option>
          <option value="vanilla">Vanilla</option>
        </select>

        <Button text="Save" />
      </form>
    )
  }

}


```