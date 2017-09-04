# Errors

<!-- <aside class="notice">This error section is stored in a separate file in `includes/_errors.md`. Slate allows you to optionally separate out your docs into many files...just save them to the `includes` folder and add them to the top of your `index.md`'s frontmatter. Files are included in the order listed.</aside>
-->
The ModerationPipe API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Required input is missing or invalid.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- Account has been disabled.
404 | Not Found -- The requested resource does not exist.
429 | Too Many Requests -- You have reached API rate limit or your total limit!

Also, each service may return **errors** array with list of errors.
