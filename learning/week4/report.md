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
//return myvar
var myvar = _.mapValues(grps, function(group){
	return group.AvgCourse	
})
return myvar
{% endlodash %}

{{result | json}}


#What should my major be if I want have a high gpa by Andrey Shprengel

{% lodash %}
var grps = _.groupBy(data, function(c){return c.Subject})
var grades = _.mapValues(grps, function(grp){return _.compact(_.pluck(grp, 'AVG_GRD'))})

var grades = _.mapValues(grades, function(group){
	var total = 0
	_.map(group, function(n){
			//console.log()
			total += n
			return total
				}
				)
				return total/(_.size(group))})
grades = _.map(grades, function(value, key){
	return {"subject": key, "grade":value}})
return _.sortBy(grades, "grade").reverse()
{% endlodash %}

{{result | json}}

# What professors should be fired? by Will Farmer

If the professor has a rating less than two standard deviations away, he's not
a good one.

{% lodash %}
var professors =  _.chain(data)
        .map(function(obj) {
            return _.map(obj.Instructors, function(inst) {
                return {"name": inst.name,
                        "rating": obj.AvgInstructor}
            })
        }).flatten()
        .filter(function(obj) {
            return obj.rating;
        })
        .value();
var rating_avg = _.reduce(professors, function(p, n) {
            return (p + n.rating);
        }, 0) / professors.length;
var rating_stddev = Math.sqrt(_.reduce(professors, function(p, n) {
            return (p + Math.pow(n.rating - rating_avg, 2));
        }, 0) / professors.length);
return [rating_avg, rating_stddev,
     _.chain(professors)
            .filter(function(obj) {
                return (obj.rating < (rating_avg - (2 * rating_stddev)));
            }).map(function(obj) {
                return obj.name + ", " + obj.rating.toString();
            }).uniq()
            .value()];
{% endlodash %}

Average professor rating is: {{ result[0] }}.

Standard Deviation is: {{ result[1] }}.

{{ result[2] | json}}
