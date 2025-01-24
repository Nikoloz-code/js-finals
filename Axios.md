```javascript
function fetchAllJokes() {
    return axios.get("https://official-joke-api.appspot.com/jokes/random");
}
```
- `return axios.get(URL);` გაგზავნის `GET` მეთოდს ფუნქციაში მოცემულ `URL`-ზე.
```javascript
function fetchBooks(name) {
    return axios
        .get('https://openlibrary.org/search.json', { //get our json
            params: {
                q: name //and the parameter we need from it, name
            }
        })
}
```
```javascript
function getCityTemp(lon, lat){ //longitude and latitude
    return axios.get('https://api.open-meteo.com/v1/forecast', {
        params: {
            latitude: lat,
            longitude: lon,
            daily: 'temperature_2m_max,temperature_2m_min'
        }
    }).catch( () => {
        console.log(chalk.red("ERROR: Can't get temperature data for \nlon - " + lon + "\nlat - " + lat))
    })
}
```
- `return axios.get(URL);` გაგზავნის `GET` მეთოდს ფუნქციაში მოცემულ `URL`-ზე იმ პარამეტრებით, რასაც მივუთითებთ.
- `params` არის თვით `API`-ს პარამეტრები, რომელსაც გაითვალისწინებს ინფორმაციის დაბრუნების დროს.
```javascript
app.get('/my-api', (req, res) => {
    api.fetchAllJokes().then(response => {
        res.json(response.data);
    }).catch(() => {
        res.json({});
    });
})
```
- `app.get()` არის [ExpressJS](ExpressJS) რაუტი.
- `api.fetchAllJokes()` გვიბრუნებს `JSON` ფაილს.
- `.then( (response) => {} )` თუ არ ამოგვიგდო 404, ვაგრძელებთ მუშაობას. ამ სიტუაციაში, `/my-api` რაუტს ვუგზავნით ჩვენი ფაილის მონაცემებს.
- `.catch( () => {} )` თუ ამოგვიგდო 404, ან რაიმე სხვა შეცდომა მოხდა, დაგვიბრუნოს არაფერი.