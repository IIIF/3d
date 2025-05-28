{% for milestone in site.data.milestones %}

### Milestone {{forloop.index}}: [{{milestone.name}}]({{"/manifests/" | append: milestone.dir | append: "/README.html" | relative_url }})

{{milestone.desc}}

{% endfor %}