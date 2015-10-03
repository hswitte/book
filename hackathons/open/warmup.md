# Warmup

Complete this warmup exercise to get an idea how to put all the different pieces
together to generate an end-to-end data analysis viz report.

<a name="top"/>
<div id="autonav"></div>

{% data src="../fcq/fcq.clean.json" %}
{% enddata %}

{% viz %}

{% title %}

What is the distribution of courses across colleges?

{% solution %}

var groups = _.groupBy(data, function(d){
    return d['CrsPBAColl']
})

var counts = _.map(groups, function(d, key) {
    var new_obj = {}
    new_obj.name = key
    new_obj.count = _.size(d)
    return new_obj
})


console.log(counts)

// TODO: modify the code below to produce a nice vertical bar charts

function computeX(d, i) {
    return i*20
}

function computeHeight(d, i, data) {
    var obj = _.max(data,'count') 
    return d.count * (300/obj.count)
}

function computeWidth(d, i) {
    return 20 
}

function computeY(d, i,data) {
    return 300 - computeHeight(d,i,data)
}

function computeColor(d, i) {
    return 'red'
}



var viz = _.map(counts, function(d, i, data){
 
            return {
                x: computeX(d, i),
                y: computeY(d, i,data),
                height: computeHeight(d, i, data),
                width: computeWidth(d, i),
                color: computeColor(d, i)
            }
         })
console.log(viz)

var result = _.map(viz, function(d){
         // invoke the compiled template function on each viz data
         return template({d: d})
     })
return result.join('\n')

{% template %}

<rect x="${d.x}"
      y="${d.y}"
      height="${d.height}"
      width="${d.width}"
      style="fill:${d.color};
             stroke-width:3;
             stroke:rgb(0,0,0)" />

{% endviz %}