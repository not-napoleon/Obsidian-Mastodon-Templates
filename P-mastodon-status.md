<%*
/*
Script and template to fetch and populate a mastodon user page from a status.

This template is intended to be used as a new page template for mastodon users. It will set up a section with the user's links and avatar, etc.
*/

// show a prompt to enter a note title
const sourceURL = await tp.system.prompt("Post URL:");
// Parse the URL;  This assumes the /@<name>/<post_id> form, not the
// /name/statuses/<post_id> form.
const [,,domain,user,statusId] = sourceURL.split("/");

// Construct the API call
const apiUrl = "https://" + domain + "/api/v1/statuses/" + statusId

// Execute the API call
const response = await fetch(apiUrl)
const jsonData = await response.json()

// The content is HTML.  Note, content may be multi-line (and indeed, may be quite long)
const content = tp.obsidian.htmlToMarkdown(jsonData.content)
// I really don't know if I should prefer the uri field or the url field.
const permalink = jsonData.uri
// Avatar Image URL
const avatarStatic = jsonData.account.avatar_static
// Author's display name
const displayName = jsonData.account.display_name

tp.file.rename("Mastodon Posts from " + user + "@" + domain)
-%>

![rw-book-cover](<%avatarStatic%>)

## Metadata
- **Author**: <% displayName %>
- **category**: #microblogs
- 
## Highlights
- <%content%> ([view post](<%permalink%>))