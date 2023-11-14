# create.js

This is a frontend framework for reusable HTML components. There are no actual source code files in this repo because this is the whole library, just copy-paste it:

```js
function create(name)
{
    let content;
    let template = document.querySelector("template[name=\"" + name + "\"]");
    
    if (template)
    {
        content = template.content.cloneNode(true /* deep clone */);

        if (template.hasAttribute("rooted"))
        {
            content = content.firstElementChild;
        }

        let boundTags = content.querySelectorAll("[id]");
        for (let tag of boundTags)
        {
            content[tag.id] = tag;
            tag.removeAttribute("id");
        }
    }
    else
    {
        content = document.createElement(name);
    }
    
    return content;
}
```

Example usage:

index.html
```html
<!DOCTYPE html>
<html>
    <head>
        <script src="create.js"></script>
        <script src="main.js" defer></script>
    </head>
    <body>
        <div id="commentsContainer">
        </div>
    </body>
    <template name="comment" rooted>
        <div>
            <span id="author"></span>
            <span id="content"></span>
        </div>
    </template>
</html>
```

main.js
```js
{
    let comment = create("comment");
    comment.author.innerText = "Jack";
    comment.content.innerText = "hello";
    commentsContainer.appendChild(comment);
}

{
    let comment = create("comment");
    comment.author.innerText = "John";

    let content = create("strong");
    content.innerText = "hi";
    comment.content.appendChild(content);

    commentsContainer.appendChild(comment);
}
```
