# Week 3

We started with a real-life example for rendering amounts from top 5 donors on Behind The Curtain's data.

##### In class challenge
```js

// dummy data
const donors = [
  {"name":"John Arnold","value":2750000},
  {"name":"Michael Bloomberg","value":2385000},
  {"name":"Loren Parks","value":896800},
  {"name":"Connie Ballmer","value":505000},
  {"name":"Tom Hormel","value":500000},
  {"name":"John K", "value":25}
];
// given the following function
const colorBlend = d3.interpolateRgb('#CDF3F2', '#08519c');

// make a function that returns to us objects that let us visualize the data
function donorPercent(amount,donors) {
  //  value of the highest donor

  // % for size  
  size: '100%'

  // color
  color: '#08519c'

  // at 0 it should be size: 0%, color: #fff
}

```

We then applied it to a tiny React project:
##### React example
```js
import React, { Component } from 'react';
import d3 from 'd3';
import './App.css';

const colorBlend = d3.interpolateRgb('#CDF3F2', '#08519c');
function donorPercent(amount,donors) {
  if(!amount) {
    return  {
      size: '0%',
      color: '#fff'
    }
  }
  let donorMax = d3.max(donors, d => d.value);
  console.log(donorMax);
  let donorSize = d3.scale.linear().domain([0,donorMax]).range([0,1]);
  console.log(donorSize);

  return {
    size: `${100 * donorSize(amount)}%`,
    color: colorBlend(donorSize(amount))
  }

}

const DonorRow = ({ amount, donors }) => {
  const { size, color } = donorPercent(amount, donors);
  return(
    <div style={{
        height:'1.4rem',
        flex:'1',
        display:'block'
      }}>
      <div style={{
          width: size,
          backgroundColor: color
        }}>
        { amount }
      </div>
    </div>
  );
}

class App extends Component {
  render() {
    const donors = [
      {"name":"John Arnold","value":275000},
      {"name":"Michael Bloomberg","value":2456000},
      {"name":"Loren Parks","value":834500},
      {"name":"Connie Ballmer","value":2505000},
      {"name":"Tom Hormel","value":500000}
    ];

    return (
      <div className="App">
        { donors.map((d,index) => (
          <DonorRow key={index} donors={donors} name={d.name} amount={d.value}/>
        ))}
      </div>
    );
  }
}

export default App;


```