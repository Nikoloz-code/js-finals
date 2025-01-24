```javascript
yargs.command({
	command: 'getweather',
	description: 'Get weather information',
	builder: {
		names: {
			description: "Names of the cities.",
			type: 'string',
			demandOption: true
		}
	},
	handler: (argv) => {
		// 'tbilisi', 'batumi', 'gori' — if more than one argument
		// tbilisi — if only one argument
		const nameList = argv.names.split(',')
		if (nameList.length > 1){
            // bevri qalaqi
	        weather.getWeatherList(nameList)
        }
        else if (nameList.length === 1){
            // erti qalaqi
            weather.getWeather(nameList[0])
        }
        else {
            //error
            console.log(chalk.red("No city name provided"))
        }
	}
});
```
- `yargs.command({});` საშუალებით ვქმნით ახალ `command`-ს.
- `command: 'commandName'` არის `command`-ის სახელი, რითაც ვიძახებთ.
- `description: 'Description.'` არის აღწერა თუ რას აკეთებს.
- `builder: {}` გამოიყენება იმისათვის რომ ჩვენ `command`-ს ჰქონდას პარამეტრები.
- `name` არის პარამეტრი სახელად `name`.
- `type` პარამეტრების ტიპები: `string`,`boolean`,`number`,`array`. 
- `demandOption`-ის საშუალებით ჩვენ შეგვიძლია რაიმე პარამეტრი გავხადოთ აუცილებელი თუ დავწერთ `demandOption = true;`, ისე კი ყოველთვის დაყენებული არის `false`-ზე.
- `handler: () => {}` ფუნქცია რაც გაიშვება `command`-ის გამოყენების დროს.
- `argv` არის არგუმენტები რასაც ვაწვდით ჩვენ `yargs`-ს. 
```javascript
yargs.parse()
```
აუცილებელია იმისათვის, რომ ჩვენი `yargs` კომანდები იმუშაოს.