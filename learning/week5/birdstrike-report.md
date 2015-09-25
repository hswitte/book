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

# (Question 1) by (Name)

{% lodash %}
return "[answer]"
{% endlodash %}


# (Question 2) by (Name)

{% lodash %}
return "[answer]"
{% endlodash %}


# (Question 3) by (Name)

{% lodash %}
return "[answer]"
{% endlodash %}

# (Question 4) by (Name)

{% lodash %}
return "[answer]"
{% endlodash %}


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
