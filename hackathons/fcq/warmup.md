{% data src="fcq.clean.json" %}
{% enddata %}

# Warmup

Next, complete the following warmup exercises as a team.

## How many unique subject codes?

{% lodash %}
// TODO: replace with code that computes the actual result
return _.size(_.uniq(_.pluck(data, "Subject")))
//return 113
{% endlodash %}

There are {{ result }} unique subject codes.

## How many computer science (CSCI) courses?

{% lodash %}
// TODO: replace with code that computes the actual result
return _.size(_.filter(_.pluck(data, "Subject"), function(d){
    return d.match(/CSCI/)
}))
//return 63
{% endlodash %}

There are {{ result }} computer science courses.

## What is the distribution of the courses across subject codes?

{% lodash %}
// TODO: replace with code that computes the actual result
return _.mapValues(_.groupBy(data, "Subject"), function(n){
    return n.length
})
//return {"HIST": 78,"HONR": 20,"HUMN": 17,"IAFS": 20,"IPHY": 134}
{% endlodash %}


<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>

## What subset of these subject codes have more than 100 courses?

{% lodash %}
// TODO: replace with code that computes the actual result
var grps = _.groupBy(data, 'Subject')
var ret = _.pick(_.mapValues(grps, function(d){
    return d.length
}), function(x){
    return x > 100
})
return ret
//return {"IPHY": 134,"MATH": 232,"MCDB": 117,"PHIL": 160,"PSCI": 117}
{% endlodash %}

<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>

## What subset of these subject codes have more than 5000 total enrollments?

{% lodash %}
// TODO: replace with code that computes the actual result
var grps = _.groupBy(data, 'Subject')
//return _.mapValues(grps, function(g){
//    return _.map(g, function(n){
//        return _.reduce(n, function(a, b){
            
//        })
//    })
//})
var grps = _.groupBy(data, 'Subject')
//_.mapValues(grps, function(g){
//    return _.map(g.N.Enroll, function(e){
//        return _.reduce(e, function(a, b){
//            return a+b
//    }
    //return enrollment over 5000
//})

var myvar =  _.mapValues(grps, function(group){
    var total = 0
    _.map(group, function(n){
    total += n.N.ENROLL
    return total
    })
    return total
})
//return myvar
return _.pick(myvar, function(x) {return x > 5000})

//return _.chain(data).groupBy
//return {"IPHY": 5507,"MATH": 8725,"PHIL": 5672,"PHYS": 8099,"PSCI": 5491}
{% endlodash %}

<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>

## What are the course numbers of the courses Tom (PEI HSIU) Yeh taught?

{% lodash %}
// TODO: replace with code that computes the actual result
var myvar = _.flatten(_.map(data, function(d){
    return _.map(d.Instructors, function(f){
        return new Object({"number": d.Course, "instructor": f.name})
    })
}))

return _.uniq(_.pluck(_.filter(myvar, function(m){
    return m.instructor == "YEH, PEI HSIU"
}), "number"))

{% endlodash %}

They are {{result}}.
