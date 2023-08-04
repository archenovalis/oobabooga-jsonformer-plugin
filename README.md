# oobabooga-jsonformer-plugin

This is a plugin for the [oobabooga/text-generation-webui](https://github.com/oobabooga/text-generation-webui)

This plugin forces models to output valid JSON of a specified schema. Most of the code was strongly inspired by [JSONFormer](https://github.com/1rgs/jsonformer) but adapted for use with oobabooga. Includes some additional tweaks to better handle the weird variety of output that can come from smaller models.

Forked from https://github.com/hallucinate-games/oobabooga-jsonformer-plugin

Changes:
* added check to force llms to follow a user designated schema. place jsonSchema_id={schemas.json array index} on the last line of your prompt
* added schemas.json file
* enabled normal (non-schema output) when schema_id not found or invalid

Limitations:
* coded to work within a docker container
* does not use text_generation.generate_reply_HF
* hardcoded text_generation.generate_reply_custom

Todo:
* allow for use outside of docker container
* remove hardcoded generate_reply_custom
* enable generate_reply_HF


example schema (append to schemas.json) incrementing the id of the last schema by 1:

```
,,,
{	
    "jsonSchema_id": 2,
    "$schema": "http://json-schema.org/draft-07/schema#",
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

You can install in via the `text-generation-webui` using the github link to this repo or if you prefer to install it manually:

Install by cloning this repo into the `extensions` directory of `text-generation-webui` e.g.
```shell
$ cd text-generation-webui/extensions
$ git clone https://github.com/hallucinate-games/oobabooga-jsonformer-plugin.git jsonformer
```
Then restart the server with `--extensions jsonformer` or enable it via the UI.
