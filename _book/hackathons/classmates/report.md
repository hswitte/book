{% import './data.html' as data %}

# Report

As a class, we brainstormed and came up with a long list of further questions we can ask based
on the "self-introduction" data. Out of these questions, our team chose to tackle on
the following:

# Who wrote the most for thier comment

{% lodash %}
return _.pluck(_.sortBy(data.comments, function(comment){
		return _.size(comment.body.split(""));
		}).reverse(), 'user.login');

{% endlodash %}

{{result | json}}

# Which student has been a member of GitHub the longest?
(This assumes that userIDs were assigned consecutively)

{% lodash %}
return _.first(_.pluck(_.sortBy((_.pluck((data.comments), "user")), "id"), "login"))
{% endlodash %}
Result is {{result | json}}

# Who has updated their comment?

{% lodash %}
return _.pluck(_.filter(data.comments, function(comment){
	return comment['created_at'] !== comment['updated_at'];
}), 'user.login');
{% endlodash %}

{{result | json}}

# Who uses the most punctuation?

{% lodash %}
return _.chain(data.comments)
    .map(function(obj) {
        return [obj.user.login,
                obj.body.match(/[`~!@#\$%\^&\*\(\)_\+-=\[\]\\\{\}\|;':",\./<>\?]/g).length];
    })
    .sortBy(function(tup) {
        return tup[1];
    })
    .map(function(tup) {
        return tup[0];
    })
{% endlodash %}

Results sorted least to most.

{{ result | json }}
