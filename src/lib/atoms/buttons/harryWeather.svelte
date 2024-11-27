<script>
    import { onMount } from 'svelte'; 
    import Harry from '$lib/atoms/harry.svelte';

    // Details
    export let textTemp;
    export let name;

    // Weather
    let city = 'Amsterdam';
    let weather;

    // Harry
    let mood = 'ass';
    let environment = 'neutraal';
    let sentence = 'Ik ben even in de war';
    let detail = '';

    const numericTextTemp = parseFloat(textTemp) || 20;

    async function getWeather() {
        const res = await fetch(
            `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=52d2a223715e49a67159446b130d4482&lang=nl&units=metric`
        );
        weather = await res.json();

        const currentTemp = weather.main.temp;
        let tempDifference = currentTemp - numericTextTemp;

        switch (true) {
            case currentTemp >= numericTextTemp + 15:
                mood = 'boos';
                environment = 'zonnig';
                sentence = `Het is echt te heet! ${currentTemp.toFixed(0)}°C.`;
                detail = ``;
                break;

            case currentTemp >= numericTextTemp + 10:
                mood = 'verdrietig';
                environment = 'zonnig';
                sentence = `Het is warm... ${currentTemp.toFixed(0)}°C.`;
                detail = ``;
                break;

            case currentTemp >= numericTextTemp + 5:
                mood = 'neutraal';
                environment = 'zonnig';
                sentence = `Het is aangenaam. ${currentTemp.toFixed(0)}°C.`;
                detail = ``;
                break;

            case currentTemp >= numericTextTemp - 5 && currentTemp <= numericTextTemp + 5:
                mood = 'blij';
                environment = 'neutraal';
                sentence = `Perfect weer! ${currentTemp.toFixed(0)}°C.`;
                detail = ``;
                break;

            case currentTemp <= numericTextTemp - 15:
                mood = 'boos';
                environment = 'koud';
                sentence = `Het is veel te koud! ${currentTemp.toFixed(0)}°C.`;
                detail = ``;
                break;

            case currentTemp <= numericTextTemp - 10:
                mood = 'verdrietig';
                environment = 'koud';
                sentence = `Het is behoorlijk koud, ${currentTemp.toFixed(0)}°C.`;
                detail = ` ${numericTextTemp}°C is ideaal voor de ${name}!`;
                break;

            case currentTemp <= numericTextTemp - 5:
                mood = 'neutraal';
                environment = 'koud';
                sentence = `Het is een beetje fris. ${currentTemp.toFixed(0)}°C.`;
                detail = ``;
                break;
        }
    }
    onMount(() => {
        getWeather(); 
    });
</script>

{#if weather}
<aside>
    <div class="weather-bubble">
        <blockquote>
            “{sentence}<span class="home_page"> {detail}</span>”
        </blockquote>
    </div>
    <Harry {mood} {environment} {textTemp}/>
</aside>
{/if}
