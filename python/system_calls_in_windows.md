# Quotation of System Call Strings in Windows

## Summary
- Always quote paths before injecting them into a string that becomes part of a systems call. 
- Always use `"`, never use `'` as they are not recognised by Windows

## Examples

### Bad

```python
s = "ffmpeg -i '{}' '{}'".format(input, output)
```
### Good

```python
s = 'ffmpeg -i "{}" "{}"'.format(input, output)
```
