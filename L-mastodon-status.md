<%*
/*
Script and template to fetch and populate a single mastodon status.

This template is intended to be used inline and it will just insert the status
content with a link to the original post.  Note that the content itself may be
multi-line.
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
-%>


<%content%> ([view post](<%permalink%>))