# Week 1

We reviewed the basics of d3 and progressed through 10 branches on:
https://github.com/hackoregon/hack-university-data-visualization/tree/lesson1/lessons

##### Basic bar chart
```js
const dataSet = [10, 20, 15, 30, 40, 55, 30, 50, 60];

const svg = d3.select('#content').append('svg')
  .attr('width', 600)
  .attr('height', 250);

svg.selectAll('rect')
  .data(dataSet) // taking in our data
  .enter() // starting d3
  .append('rect') // appending rect element
  .attr('class', 'bar') // assigning class
  .attr('x', (data, index) => (index * 20))
  .attr('y', data => (250 - data))
  .attr('width', 15)
  .style('height', data => data) // returning data for height value
```

##### Scaling
```js
d3.scale.linear()
  .domain(/*...*/)
  .range(/*...*/)

d3.scale.ordinal()
  .domain(/*...*/)
  .rangeBands([0, width], 0.25, 0.25); // (width of data), padding between, padding outside

```
##### Axis
```js
// .axis() method
d3.svg.axis()
  .scale(/* x or y scale*/)
  .orient(/* top bottom left or right */)
  .ticks(/* amount of ticks in the axis as integer*/)
  .innerTickSize(/* integer */)
  .outerTickSize(/* integer */)
  .tickPadding(/* integer */);

// using .call to execute functions like yAxis
.call(/* reference to function */);
```

##### Transitions and events
```js
// Toanimate your visualizations
  .transition(/* start animated transition */)
  .delay(/* milliseconds */)
  .duration(/* milliseconds */)
  .ease(/* eg: in, out, in-out, out-in, elastic, bounce */)
  .attr(/* change an attribute with a callback and datum */)
  .style(/* change style - mind where it happens */);

// events similar to native js or jQuery
  .on('mouseover', callback(){
    /* do stuff here when you hover  */
  })
// some more events: mouseup, mouseout, mousedown, keydown, keyup..

// handy d3 function to toggle a class
d3.select(this).classed(/* className */, true|false);
```

##### d3 time formatting
```js
// formatting time in d3 from available data
d3.time.format(/* specify the format, eg: %Y/%m/%d */)

const newDate = '2016-03-13'
const formattedDate = d3.time.format('%Y-%m-%d').parse(newDate);
// formattedDate is Date object == Sun Mar 13 2016 00:00:00 GMT-0800 (PST)
d3.time.scale() // functionality for scaling time
```

##### Fetching data & setting up line charts
```js
// ways to get data from files or online
d3.json == $.getJSON
d3.csv

d3.svg.line() // d3 has layouts and this is one for line charts
.interpolate(/* eg: basis */) //  Read more at https://github.com/d3/d3-interpolate
```

##### Nest & Rollup
```js
d3.extent(/* an array */) // to get the min & max

// grouping data and working with it
d3.nest() // allows us to group data
    .key( function(d) {return d./* key */}) // the key you want to group by
    .sortKeys(d3.ascending) // can sort here as well
    .rollup(function(values) { // rollup allows us to run function on each "leaf" element
      return d3.sum(values,  function(d) { // values are the leaf elements here
        return d./* some value you want to work with */;
      })
    })
    .entries(/* array */); // this applies the nest operator to the array of data

```