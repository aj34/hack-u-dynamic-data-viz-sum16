# Week 3

### Monday
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

We also reviewed the code from week 2 and some new syntax for es2017 and React.


### Wednesday

After reviewing the HW we worked in class on d3.layout.pie and d3.layout.tree

### Example of d3.layout.pie
```js
// constants
const width = 750;
const height = 500;
const radius = Math.min(width, height) / 2;

// fetch data & render inside for static visual
d3.json('http://54.213.83.132/hackoregon/http/oregon_individual_contributors/5/', data => {
  // append our svg with constants for w, h & offsets
  const svg = d3.select('#content')
    .append('svg')
    .attr({
      width: width,
      height: height
    })
    .append('g')
    .attr('transform', 'translate(' + radius + ',' + radius + ')');

  // this example uses sort & ascending - try d3.descending
  const pie = d3.layout.pie()
    .sort((a,b) => d3.ascending(a.sum,b.sum))
    .value(d => d.sum); // choosing the data to visualize

  // arc generator function gives us a slice per datum
  const sliceOfPie = d3.svg.arc()
    .innerRadius(40) // allows us to make donuts vs pies
    .outerRadius(radius - 10)
    .startAngle(d => d.startAngle)
    .endAngle(d => d.endAngle);

  // a separate arc function for our labels later
  const labelArc = d3.svg.arc()
    .outerRadius(radius-60)
    .innerRadius(radius-60);

  // our color function this time uses a built in helper
  const colors = d3.scale.category20c();

  // for less string interpolations
  const translate = (a,b) => `translate(${a},${b})`;

  // formatting the money
  const formatMoney = d3.format("$,");

  // creating and appending our slices
  const group = svg.selectAll('.slice')
    .data(pie(data)) // calling the pie func on data
    .enter()
    .append('g')
    .attr("class", "slice");

  // appending the path for each slice
  group.append('path')
    .attr({
      d: sliceOfPie, // data attribute
      stroke: 'white',
      'stroke-width': 4,
      fill: (d, i) => colors(i)
    })

  // adding our labels - note how both labels are done
  group.append("text")
    .attr("transform", d => translate(...labelArc.centroid(d))) // using spread operator for 2 arguments from array
    .text(d => `${d.data.contributor_payee}`);
  group.append('text')
  .attr("transform", d => translate(labelArc.centroid(d)[0],(labelArc.centroid(d)[1]+20))) // using the centroid numbers but customizing offsets
  .text(d => `${formatMoney(d.data.sum)}`);
});
```

### Example of d3.layout.tree
```js
// constants
const width = 1000;
const height = 400;
const margin = {
  top: 10,
  left: 80,
  bottom: 10,
  right: 200
};
const nodeRadius = 6;

// basic svg appended
const svg = d3.select('#content')
  .append('svg')
  .attr({
    width: width,
    height: height
  });

// layout for tree
const tree = d3.layout.tree()
  .size(
    [height - (margin.bottom + margin.top),
      width - (margin.left + margin.right)
    ]);

// tree has nodes (elements or people etc..)
const nodes = tree.nodes(data);
// & links(relationships)
const links = tree.links(nodes);

// diagonal cubic Bézier path generator to connect nodes + links
const diagonal = d3.svg.diagonal()
  .projection(datum => [datum.y, datum.x]);

// because I hate writing out string interpolations
const translate = (a, b) => `translate(${a},${b})`;

// start drawing the links group first
const linksGroup = svg.append('g')
  .attr('transform', translate(margin.left, margin.top))

// it is important to draw our links path first
linksGroup.selectAll('path')
  .data(links)
  .enter()
  .append('path', 'g')
  .attr({
    d: diagonal, // path data is generated with diagonal
    fill: 'none',
    stroke: 'cornflowerblue',
    'stroke-width': 1
  });
// try switching them to see what happens and think about why that happened

// draw the nodes group next
const nodesGroup = linksGroup.selectAll('g')
  .data(nodes)
  .enter()
  .append('g')
  .attr('transform', datum => translate(datum.y, datum.x));

// append a shape for the nodes themselves
nodesGroup.append('circle') // you can choose other shapes
  .attr({
    r: nodeRadius, // radius is determined by our constant
    fill: '#fff',
    stroke: 'tomato',
    'stroke-width': 1
  });

// append labels next
nodesGroup.append('text')
  .text(datum => datum.name)
  .attr('y', datum => datum.children ? (-nodeRadius * 2) : (nodeRadius))
  .attr('x', datum => datum.children ? 0:(nodeRadius+3))
  .attr('text-anchor', datum => datum.children ? 'middle':'left')
  // we use our nodeRadius constant to calculate offsets for the labels & whether they are children to adjust location of labels
```

