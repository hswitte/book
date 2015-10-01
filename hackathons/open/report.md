# Report

Use only Javascript and SVG to produce a data analysis / visualization report.

# Authors

This report is prepared by:
* [Heather Witte](https://github.com/hswitte)
* [Fadhil Suhendi](https://github.com/fadhilfath)

<a name="top"/>
<div id="autonav"></div>


{% data src="../../learning/week5/birdstrike.json" %}
{% enddata %}


{% viz %}

{% title %}

What is the total cost of damage inflicted by birds of various sizes?

{% solution %}

var groups = _.groupBy(data, function(n){
	return n['Wildlife: Size']
})



var i = 0
var costs = _.mapValues(groups, function(d){
	var total = _.reduce(d, function(total, e){
		var cost = e['Cost: Total $']
		if (_.isString(cost)){
			var temp1 = cost.replace(/,/g,'')
			var temp2 = _.parseInt(temp1)
			cost = temp2
		}
		return total+cost
	},0)
	return total
})

var newvar = _.map(costs, function(v,k){
	return {"size": k, "cost":v}
})

_.remove(newvar, function(n){
 	return n.size == ''	
})

var d = _.sortBy(newvar, "cost").reverse()



function computeX(d, i) {
    return i*20
}

function computeHeight(d, i) {
    return d.cost/1000000
}

function computeWidth(d, i) {
    return 20 
}

function computeY(d, i) {
    return 300 - d.cost/1000000
}

function computeColor(d, i) {
    return 'green'
}

function computeLabel(d, i) {
    return d.size + ": $" + d.cost
}

var viz = _.map(d, function(d, i){
            return {
                x: 0,
                y: computeX(d, i),
                height: computeWidth(d, i),
                width: computeHeight(d, i),
                color: computeColor(d, i),
                label: computeLabel(d, i)
            }
         })
console.log(viz)

var result = _.map(viz, function(d){
         // invoke the compiled template function on each viz data
         return template({d: d})
     })
return result.join('\n')

{% template %}
<g transform="translate(0 ${d.y})">
<rect x="150"
      y="0"
      height="${d.height}"
      width="${d.width}"
      style="fill:${d.color};
             stroke-width:3;
             stroke:rgb(0,0,0)" />
<text  transform="translate(0 15)">${d.label}</text>
</g>

{% endviz %}
