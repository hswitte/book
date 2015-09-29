{% data src="birdstrike.json" %}
{% enddata %}

# Report

As a team, answer a subset of the questions submitted during the hackathon.
But instead of using Tableau, you will need to write Javascript/Lodash code
to derive your answers. Similar to before, each team member is responsible for
one question. But everyone should work together to come up with a good solution.
Your answer should consist of Lodash code and a brief writeup.
Utilize `_.map`, `_.filter`, `_.group` ...etc. Do not se any for loop.

This time, the data is not already prepared for you in a nice JSON format. You
will need to do it on your own, replacing the placeholder `birdstrike.json` with
real data.

This report is prepared by
* [Kari Santos](https://github.com/karisantos)
* [Heather Witte](https://github.com/hswitte)
* [Zachary Lamb](https://github.com/ZachLamb)
* [Fadhil Suhendi](https://github.com/fadhilfath)
* [Denis Kazakov](https://github.com/94kazakov)

# which airlines have the worst luck with birdstrikes in terms of damage caused?

{% lodash %}
var groups = _.groupBy(data, function(n){
	return n['Aircraft: Airline/Operator']
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
var sorted = _.sortBy(_.pairs(costs), function(d){
	return d[1]
})

return _.slice(_(sorted).reverse().value(), 0,10)
{% endlodash %}


<table><tr><td>Airline</td>
	<td>Total Cost</td></tr>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>



# what is the most common flight phase where a birdstrike occurred?
{% lodash %}
var groups = _.groupBy(data, function(n) {
    return n['When: Phase of flight']
})

var counts = _.mapValues(groups, function(d){
    return d.length
})
var sorted = _.sortBy(_.pairs(counts), function(d) {
    return d[1]
})

return _.slice(_(sorted).reverse().value(), 0,10)
{% endlodash %}
<table><tr><td>Flight Phase</td>
	<td>Strike Count</td></tr>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>


# Which plane strikes the most birds? 
{% lodash %}
var groups = _.groupBy(data, function(n) {
    return n['Aircraft: Make/Model']
})

var counts = _.mapValues(groups, function(d){
    return d.length
})

var sorted = _.sortBy(_.pairs(counts), function(d) {
    return d[1]
})

return _.slice(_(sorted).reverse().value(), 0,10)
{% endlodash %}
The table lists the top 10 aircraft makes that had the most incidents. 
<table><tr><td>Aircraft Make/Model</td>
	<td>Strike Count</td></tr>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>

# what airports have the most expensive average accident?

{% lodash %}
var groups = _.groupBy(data, function(n) {
    return n['Airport: Name']
})
var i = 0
var costs = _.mapValues(groups, function(d){
	var avg = _.reduce(d, function(total,e){
		var cost = e['Cost: Total $']
		if (_.isString(cost)){
			var temp1 = cost.replace(/,/g,'')
			cost = _.parseInt(temp1)	
		}
		return total+cost
	},0)/_.size(d)
	return avg
})
var sorted = _.sortBy(_.pairs(costs), function(d) {
    return d[1]
})

return _.slice(_(sorted).reverse().value(), 0,10)
{% endlodash %}

<table><tr><td>Airline</td>
	<td>Total Cost</td></tr>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>


# what state had the highest number of bird strikes?

{% lodash %}
var grps = _.groupBy(data, 'Origin State')

var map = _.mapValues(grps, function(d){
    return d.length
})

var newvar = _.map(map, function(v,k){
	return {"state": k, "count":v}
})

_.remove(newvar, function(n){
 	return n.state == 'N/A'	
})

return _.sortBy(newvar, "count").reverse()

{% endlodash %}

<table><tr><td>State</td><td>Strike Count</td></tr>
{% for item in result %}
<tr> <td>{{item.state}}</td> <td>{{item.count}}</td> </tr>
{% endfor %}
</table>
