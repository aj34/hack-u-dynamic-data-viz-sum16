### Assignment
For all problems use the Oregon Summary Data to calculate range/domain as needed.

### Oregon Summary Data ~ Nov 2010 - Aug 2016
```js
// This is summary data for OR to compare with
const summary = [
  {
    "in":353648758.9, // total amount contributed
    "out":341146543.64, // total amount donated
    "from_within":91600635.93, // donations from within OR
    "to_within":71467440.8999999, // donations within OR
    "from_outside":262048122.97, // donations from out of state
    "to_outside":269679102.74, // donated/sent out of state
    "total_grass_roots":44503420.66, // each contribution was under $250
    "total_from_in_state":233200288.870001
  }
];
```

### Top contributors by type
```
http://54.213.83.132/hackoregon/http/oregon_individual_contributors/{NUMBER}/
http://54.213.83.132/hackoregon/http/oregon_committee_contributors/{NUMBER}/
http://54.213.83.132/hackoregon/http/oregon_business_contributors/{NUMBER}/
```
For example - top 20 individual contributors would be:
http://54.213.83.132/hackoregon/http/oregon_individual_contributors/20/
Change the last number to vary the amount of data you want to work with.

### Problems

1. Make a bar graph that given an API endpoint shows top 10 contributors of each type.
2. Make a pie chart that also depicts the same data.

###### Use OR_sum.json for the following
3. Make a scatter plot/bubble chart that shows total_from_in_state and total_from_the_outside over time.

Extra
Use the Oregon Summary Data to calculate ranges
Using contributors by type:
1. Make a grouped bar chart that shows all 3 types against each other.
2. Make 1 bar chart that transitions based on the data it's given.
3. Make a pie chart that transitions based on the amount of 'slices' it has.

