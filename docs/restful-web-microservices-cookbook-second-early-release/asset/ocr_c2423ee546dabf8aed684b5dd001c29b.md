```
<form name="create-template"
  action="http://api.example.org/users"
  method="post" enctype="application/x-www-form-urlencoded">
  <input name="id" value="q1w2e3r4" />
  <input name="familyName" value="Mark" />
  <input name="givenName" value="Morkelson" />
  <input name="telephone" value="+1-555-123-4567" />
  <input name="email" value="mork@example.org" />
  <input name="status" value="active" />
  <input type="submit" />
</form>
```

The vocabulary information in this form is: `create-template`, `id`, `familyName`, `givenName`, `telephone`, `email`, and `status`. Both the API service and the API consumer need to recognize and “understand” what these values represent. Notice that I did not include the *values* for those elements (e.g. `id=q1w2e3r4`), just the names. Also, I did not include the media-type’s structural elements in my list (e.g. `name`, `action`, `method`, `enctype`, etc.).

And it is not enough just the identify the vocabulary elements. You also need to offer some explanation of what they represent and how they may appear in a message. For example, the `create-template` semantic value may appear as the `name` value of a `form` element in HTTP messages. This identifies the form clients can use to add new records to the system.

You can also include additional information in your semantic profile documents such as the source of the string value. For example, `familyName` is defined in the online dictionary Schema.org 53.

# Example

Below are some example vocabulary documents (aka semantic profiles) for `person` API listed in [Link to Come]. Here’s some sample output from that service:

```
{
  "collection": {
    "version": "1.0",

```