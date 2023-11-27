---
description: >-
  Please note Animation names must be unique in each file. Credit to DerToaster
  for this feature.
---

# How to use the inclusion feature in Animation files

In your animation JSON, you will want to add the `includes` section to the file like so:

```json
{
	"format_version": "1.8.0",
	"includes": [
		{
			"file_id": "animationfilename",
			"animations": [
				"aninName1",
				"aninName2"
			]
		}
	],
	"animations": {
	},
	"azurelib_format_version": 2
}
```
