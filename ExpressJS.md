## Get Started
```javascript
const express = require('express')
const app = express()
const PORT = 3000
```
- `express()` სერვერის ინიციალიზაციას ახდენს და ექსპრესის ინსტანცია ინახავს მას `app`-ში.
- `PORT` არის ჩვენი სერვერის პორტი.
```javascript
app.use(express.static(path.join(__dirname, './public')));
app.set('view engine', 'hbs');
app.set('view', path.join(__dirname, './views'));
```
- `app.use(express.static('public'));` გვაძლევს საშუალებას რომ გამოივიყენოთ სტატიკური ფაილები, როგორიცაა: *სურათები*, *CSS* და *JavaScript ფაილები*:
  
  `public/`
  
  `├─ css/ `
  
  `├─ js/ `
  
- `app.set('view engine', 'hbs');` გვაძლევს საშუალებას, რომ შევცვალოთ *view engine* ვებსაიტის რენდერინგისთვის.
- `app.set('views', path.join(__dirname, './views'));` აპლიკაციას ვაწვდით ჩვენ ვებ-გვერდებს. *directory*-ს სახელი უნდა იყოს `views`:
  `views/ ` 
  `├─ partials/ `
- `path.join()` და `__dirname` არის შესაძლებელი მარტო `path` მოდულით, რომელიც მოყვება `NodeJS`-ს. იგი არ არის `ExpressJS`-ის მოდულის ნაწილი.
### Project Structure
ჩვენი საბოლოო პროექტის სტრუქტურა უნდა იყოს შემდეგნაირი:
  
`public/` 

`├─── css/ `

`├─── js/ `

`views/ `

`├─── partials/ `

`app.js`
  
`Partials`-ის შესახებ შეგიძლიათ წაიკითხოთ [ამ ბმულზე](HBS.md##Partials).
```javascript
app.use(express.urlencoded())
app.use(express.json())
```
- `express.json()` არის მეთოდი რომელიც არის ჩაშენებული `Express`-ში იმ მიზნით რომ დაამუშაოს `Request` ობიექტი `JSON`ობიექტად.
- `express.urlencoded()` არის მეთოდი რომელიც არის ჩაშენებული იმისათვის, რომ შემომავალი `Request` ობიექტები იყოს აღქმული როგორც `string` ან და `array`.
## GET Method Routing
- `GET` მეთოდი არის გამოყენებული იმისათვის, რომ მოვითხოვოთ რაიმე ინფორმაცია სპეციფიკური რესურსიდან.
### Sending Answer To The Route
```javascript
app.get('/', (req, res) => {
    res.send('Hello World');
})
```
- `req -> Request` არის კლიენტისაგან გაგზავნილი თხოვნა რომ მოქმედება მოხდეს სერვერზე.
- `res -> Response` არის თხოვნაზე მოცემული პასუხი, რაც სერვერს შეუძლია გამოიყენოს.
- `app.get(route, (req, res) => { } )` არის `GET` მეთოდის რაუტი, რითაც ჩვენ შეგვიძლია გამოვაჩინოთ ჩვენი ვებ-გვერდი.
- `res.send();` საშუალებით, ჩვენ შეგვიძლია გავგზავნოთ რაიმე ან `Buffer` ობიექტი, `String`, `Boolean` ან `Array`. ტექსტი (String) ავტომატურად გადაიქცევა როგორც `HTML`.
### Sending Data To A Route
```javascript
app.get('/', (req, res) => {
    res.json({user: 'tobi'});
})
```
- `res.json();` აგზავნის ***JSON*** პასუხს. ფუნქციაში მოცემული ***JSON*** ფაილი არის კონვერტირებული როგორც ***JSON String***, `JSON.stringify()` ფუნქციის საშუალებით.
### Sending Data To A View
```javascript
app.get("/", (req, res) => {
    res.render('blogList', {
        data: JSON.parse(fs.readFileSync('./blog-data.json'))
    })
})
```
- `res.render(view);` ერთ-ერთ ჩვენ **_view_**-ს გადააქცევს ***HTML String***-ად კლიენტისთვის. ***აუცილებელია, რომ ფუნქციაში მოცემული view იყოს string, რომელიცაა view ფაილამდე მისაღწევი გზა. შეიძლება იყოს აბსოლუტური გზა (absolute path), ან  და თუ გაქვთ გამოყენებული `app.set('views')`, მის გზის მიხედვით მოცემული გზა (relative path).**
- `res.render(view, [,locals]); -> locals` არის ობიექტი რომლის მონაცემები წარმოადგენს view-ს ცვლადებს. თუ ვიყენებთ [HBS](HBS.md)-ს, ცვლადების გამოყენების მაგილითი შეგვიძლია ვნახოთ [აქ](https://github.com/Nikoloz-code/js-finals/blob/main/HBS.md#using-data).
### URL Parameters
```javascript
app.get("/:id", (req, res) => {
    const fileData = JSON.parse(fs.readFileSync('./blog-data.json'))
    const blogData = fileData.find(el => el.id == req.params.id)
    if (!blogData) {
        res.send('Blog not Found!')
        return
    }
    res.render('blogDetail', {
        data: blogData
    })
})
```
- `:id` არის `URL` პარამეტრი. ჩვენ შეგვიძლია მაგ პარამეტრს მივწდეთ `req.params.id`-ის საშუალებით.
### Axios/Express Relationship
```javascript
app.get('/my-api', (req, res) => {
    api.fetchAllJokes().then(response => {
        res.json(response.data);
    }).catch(() => {
        res.json({});
    });
})
```
- `api.fetchAllJokes().then( () => {} ).catch( () => {} )` არის [Axios](Axios.md)-ის HTTP Request-ების დამამუშავებელი მოდული. იგი არ არის `ExpressJS`-ის ნაწილი.
- რომ გადავიდეთ `http:localhost:PORT/my-api`-ზე, ჩვენ მივიღებთ `JSON` ინფორმაციას, რასაც `Axios` მოგვაწვდის და ვაგზავნით `res.json()`-ის საშუალებით.
### Star-Route
```javascript
app.get('*', (req, res) => { //this needs to be at the bottom
    res.send('404: PAGE NOT FOUND')
})
```
- `'*'` მიგვანიშნებს თუ მომხმარებელი არის რაიმე `URL`-ზე, რომლისთვისაც არ გვაქს რაიმე კოდი დაწერილი.
## POST Method Routing
- `POST` მეთოდი არის გამოყენებული იმისათვის, რომ გავგზავნათ ინფორმაცია სერვერთან რომ შევქმნათ/განვაახლოთ რაიმე რესურსი.
```javascript
app.post('/blog', (req, res) => {
	const newBlog = req.body
	
	//create new blog with data
	newBlog.id = Math.ceil(Math.random() * 999999999)
	const newDate = new Date()
	newBlog.created = newDate.toLocaleString()
	newBlog.updated = newDate.toLocaleString()
	
	const fileData = JSON.parse(fs.readFileSync('./blog-data.json'))
	fileData.unshift(newBlog)
	fs.writeFileSync('./blog-data.json', JSON.stringify(fileData))
	
	res.redirect('/')
})
```
- `req.body` დააბრუნებს ინფორმაციას, რაც იგზავნება `POST` მეთოდით.
- `unshift()` ფუნქცია ამატებს ინფორმაციას დოკუმენტის დაწყებაში.
- `res.redirect()` ფუნქციას გადავყავართ სხვა რაუტზე.
`app.post('/blog')` არის გააქტიურებული როდესაც ვებსაიტი გზავნის `POST Request`-ს სპეციფიკურად `/blog` რუტზე, როგორც მაგალითად ეს [.hbs](HBS.md) დოკუმენტი:
```html
    <main>
        <form action="/blog" method="post" enctype="application/json">
            <label for="title">Blog Title</label>
            <input value="{{data.title}}" type="text" name="title" id="title" required>

            <label for="blog">Blog Body</label>
            <textarea name="blog" id="blog" required>{{data.blog}}</textarea>

            <input type="hidden" value="{{data.id}}" name="id">

            <button class="link-btn" type="submit">{{submitText}}</button>
        </form>
    </main>
```
- `<form action="/blog" method="post">` წყვეტს თუ რომელ `URL`-ზე (რუტზე) იგზავნება ჩვენი `method="post"`-ით გაგზავნილი ინფორმაცია. 
## DELETE Method Routing
```javascript
app.delete('/blog/:id', (req, res) => {
    const fileData = JSON.parse(fs.readFileSync('./blog-data.json'))
    const blogIndex = fileData.findIndex(el => el.id == req.params.id)
    if (blogIndex !== -1) {
        fileData.splice(blogIndex, 1);
        fs.writeFileSync('./blog-data.json', JSON.stringify(fileData))
    }
    res.send()
})
```
- `res.send()` აგზავნის პასუხს რომ `DELETE` მეთოდმა დაამთავრა მუშაობა.
რომ გავააქტიუროთ `DELETE` მეთოდი, ვებ-გვერდზე შეიძლება გვქონდეს შემდეგი კოდი:
```html
<script>
const btn = document.getElementById("delete-blog")
btn.addEventListener('click', function() {
	const deletePost = confirm("Are you sure you want to delete the blog post?")
	if(deletePost){
		fetch('/blog/' + btn.attributes.blogid.value, {
			method: 'delete'
		})
			.then(function() {
				window.location.replace("/")
			})
	}
})
</script>
```
- `.then()` გაიშვება როდესაც სერვერი `res.send()`-ით გაგზავნის პასუხს. 
## LISTEN to Host/Port
```javascript
app.listen(PORT, () => {
    console.log('App listening to port: http://localhost:' + PORT);
})
```
- `app.listen(PORT, function() )` აკავშივერს და უსმენს კავშირებს მოცემულ `host` (მასპინძელი) და `port` (პორტზე). ჩვენ სიტუაციაში, ვიყენებთ მარტო `PORT`-ს. ხოლო ჩვენი `host` კი იქნება `http://localhost:PORT`.
