{% for milestone in site.data.milestones %}

### Milestone {{forloop.index}}: [{{milestone.name}}](/manifests/{{milestone.dir}}/README.html)

{{milestone.desc}}

{% endfor %}