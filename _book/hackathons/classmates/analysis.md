# Analysis

{% import './data.html' as data %}

After completing the warmup exercises, your task is to do four more slightly
more challenges analyses.

## How many students like sushi as their favorite food?

{% lodash %}

return _.size(_.filter(_.pluck(data.comments, "body"), function(n){
	return _.last(n.toLowerCase().split(" ")) == 'sushi'
}))

{% endlodash %}

The answer is {{result}}.

## Who are the students liking Python the most?

{% lodash %}

var py = _.filter(_.pluck(data.comments, "body"), function(n){
	return (n.toLowerCase().match(/.*python.*/))
})
var x = []
_.forEach(py, function(n){
	x.push( ((n.split('\n')[0]).split(':'))[1] )
})

return x

{% endlodash %}

Their names are {{result}}.

## Are there more Javascript lovers or Java lovers?

{% lodash %}

var js = _.size(_.filter(_.pluck(data.comments, "body"), function(n){
	return (n.toLowerCase().match(/.*javascript.*/))
	})) 

var java = _.size(_.filter(_.pluck(data.comments, "body"), function(n){
	return (n.toLowerCase().match(/.*java[^s].*/))
}))

if (js > java){return "Javascript"}
else if (java > js){return "Java"}
else {return "Java and Javascript are equally loved"}

{% endlodash %}

The answer is {{result}}.

## Who like the same food as `kjblakemore`?

{% lodash %} 
var food = _.last(_.pluck(_.filter(data.comments , function(n){ 
	return n.user.login == 'kjblakemore'
}),'body')[0].toLowerCase().split("food: ")) 
return _.pluck(_.pluck(_.filter(data.comments, function(n){ 
	return (_.last(n.body.toLowerCase().split("food: "))== food)
}), 'user'),'login') 

{% endlodash %}

Their names are {{result | json}}.


