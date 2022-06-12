---
title: Markdown Syntax
date: 2022-06-12 14:25:40 +2
categories: [PROGRAMING]
tags: [programming]     # TAG names should always be lowercase
---

# Markdown Cheatsheet

## Headers

### Ilyen header

```markdown
### example
```

> **Header**-t a "**#**" karakterrel lehet létrehozni, minél több a "#", annál "alkategóriájúbb" a header!
{: .prompt-info }

## Alap wordformatting

*Ilyen döntés*
```markdown
*example*
_example_
```

**Ilyen félkövér**
```markdown
**example**
```

~~Ilyen áthúzás~~
```markdown
~~example~~
```

Ilyen fájlhely `/dockerconfig/portainer`{: .filepath }:
```markdown
`examplefolder/examplefile.txt`{: .filepath }
```

Ilyen Kódblokk:
```python
def code(codeblock):
	if codeblock == python:
		return True
	else:
		return False
```
```markdown
	```yaml
	entity_id: sensor.example
	```
```

## Prompts

> Ilyen tipp block:
{: .prompt-tip }
```markdown
> example
{: .prompt-tip }
```

> Ilyen info block:
{: .prompt-info }
```markdown
> example
{: .prompt-info }
```

> Ilyen warning block:
{: .prompt-warning }
```markdown
> example
{: .prompt-warning }
```

> Ilyen danger block:
{: .prompt-danger }
```markdown
> example
{: .prompt-danger }
```


