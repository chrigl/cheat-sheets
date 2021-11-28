---
tags: [jinja2, python, template]
---
# Update field in a dict

```
{% set _ = extra_args.update({"advertise-address": "$(HOST_IP)"}) -%}
```

if `jinja2.ext.do` is enabled (what is not the case in ansible)

```
{% do extra_args["advertise-address"="$(HOST_IP)"] -%}
```
