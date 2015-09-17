{% data src="../../hackathons/fcq/fcq.clean.json" %}
{% enddata %}

# Report

As a team, answer all the questions the team's members submitted on our
[course forum](https://github.com/bigdatahci2015/forum/issues/14). Each
team member is responsible for one question. But everyone should work together
to come up with a good solution. Your answer should consist of Lodash code
and a brief writeup. Utilize `_.map`, `_.filter`, `_.group` ...etc. Do not
use any for loop.

It is important for everyone to understand all the solutions and make sure you
will be able to independently reproduce these solutions when asked to do so.
Coming up, we will incorporate variations of these questions into a future hackathon
 and you are expected to be capable of reproducing and adapting your solutions.

# What is the average course rating by department? by Heather Witte
{% lodash %}
var grps = _.groupBy(data, 'Subject')
var myvar =  _.mapValues(grps, function(group){
    var rating = [0,0]
    _.map(group, function(n){
    	if (n.AvgCourse != null) {
    	rating[0] += n.AvgCourse
    	rating[1] += 1}
    return rating
    })
    return rating[0]/rating[1]
})
return myvar
{% endlodash %}

{{result | json}}


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

# (Question 5) by (Name)

{% lodash %}
return "[answer]"
{% endlodash %}
