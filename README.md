# oobabooga-jsonformer-plugin

This is a plugin for the [oobabooga/text-generation-webui](https://github.com/oobabooga/text-generation-webui)

This plugin forces models to output valid JSON of a specified schema. Most of the code was strongly inspired by [JSONFormer](https://github.com/1rgs/jsonformer) but adapted for use with oobabooga. Includes some additional tweaks to better handle the weird variety of output that can come from smaller models.

Forked from https://github.com/hallucinate-games/oobabooga-jsonformer-plugin

Changes:
* output now follows a user designated schema from the schemas.json file by placing js={schemas.json array index} on its own line in the prompt (last line seems to be best)
* enabled normal (non-schema output) when js=# is not in the prompt and on its own line


example schema (append to schemas.json):

```
,,,
{	
    "id": "yourSchemaName",
    "type": "object",
    "properties": {
        "name": {"type": "string"},
        "age": {"type": "number"},
        "is_student": {"type": "boolean"},
        "courses": {
            "type": "array",
            "items": {"type": "string"},
            "allowed_empty": "true"
        },
    "required": ["name", "age"]
    }
}
```

example prompt using a local WizardLM llm:
```
Input
Goal: Research Tacos

Create a list of tasks to accomplish the goal.

js=yourSchemaName
### Response:
```

example output:
```
[
    "Research taco history",
    "Learn about different types of tacos",
    "Explore regional variations",
    "Understand ingredients used in making tacos",
    "Discover popular taco toppings and fillings",
    "Investigate cultural significance of tacos",
    "Find recipes for homemade tacos"
]
```

You can install in via the `text-generation-webui` using the github link to this repo or if you prefer to install it manually:

Install by cloning this repo into the `extensions` directory of `text-generation-webui` e.g.
```shell
$ cd text-generation-webui/extensions
$ git clone https://github.com/hallucinate-games/oobabooga-jsonformer-plugin.git jsonformer
```
Then restart the server with `--extensions jsonformer` or enable it via the UI.
