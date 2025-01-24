## Partials
```javascript
hbs.registerPartials(path.join(__dirname, "./views/partials"))
```
- თუ ჩვენ `.hbs` დოკუმენტში გვაქ რაიმე სექცია რომელიც გვინდა გამოვიყენოთ რამოდენიმე დოკუმენტში, მას გადავაქცევთ როგორც `Partials`, რომელიც არის უბრალოდ 
- `hbs.registerPartials` გვაძლევს საშუალებას რომ მივუთითოთ `HBS` მოდულს თუ სად მდებარეობს ჩვენი `Partials` დოკუმენტები:
  `views/ `
  `├─── partials/ `
## Using Data
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Blog List</title>
    <link rel="stylesheet" href="/css/main.css">
</head>
<body>
    {{> header}}
    
    <main>
        <div class="blog-list">
            {{#each data}}
                {{> blogCard title=this.title blog=this.blog created=this.created updated=this.updated}}
            {{/each}}
        </div>
    </main>

    {{> footer}}
</body>
</html>
```
- `{{> header}}` მიუთითებს `header.hbs` `Partials` ფაილზე. 
- `{{#each data}}` იწყებს `foreach` ციკლს `data`-ში, რომელსაც იღებს [ExpressJS](ExpressJS###Sending%20Data%20To%20A%20View)-იდან. `{{/each}}` არის ციკლის დამთავრება.
- `{{> blogCard title=this.title blog=this.blog created=this.created updated=this.updated}}` ასევე არის `Partials`, რომელსაც გააჩნია თავისი ლოკალური ცვლადები.
**ეს არის blogCard .hbs დოკუმენტი:**
```html
<a class="blog-card" href="/blog/{{id}}">
    <h3>{{ title }}</h3>
    <p>{{ blog }}</p>
    <span>Created on: {{ created }}</span><span>Updated on: {{ updated }}</span>
</a>
```