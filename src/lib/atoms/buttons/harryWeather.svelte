<script>
    import { onMount } from 'svelte'; 
    import Harry from '$lib/atoms/harry.svelte';

    // Details
    export let textTemp;
    // export let name;

    // Weather
    let city = 'Amsterdam';
    let weather;

    // Harry
    let mood = 'twerk';
    let environment = 'neutraal';
    let sentence = 'Ik ben even in de war';
    let detail = '';

    const numericTextTemp = parseFloat(textTemp) || 25;

    async function getWeather() {
        const res = await fetch(
            `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=52d2a223715e49a67159446b130d4482&lang=nl&units=metric`
        );
        weather = await res.json();

        const currentTemp = weather.main.temp;
        const currentRain = weather.rain ? weather.rain['1h'] : null;

        let tempDifference = currentTemp - numericTextTemp;

        function addRainSentence(base) {
            return base + (currentRain ? ` Ook nog ${currentRain.toFixed(1)}mm regen per uur!` : '');
        }

        switch (true) {
            case currentTemp >= numericTextTemp + 15:
                mood = 'boos';
                environment = 'zonnig';
                sentence = addRainSentence(`Het is echt te heet! ${currentTemp.toFixed(0)}°C.`);
                detail = ` De ${name} vindt dit echt te warm.`;
                break;

            case currentTemp >= numericTextTemp + 10:
                mood = 'verdrietig';
                environment = 'zonnig';
                sentence = addRainSentence(`Het is warm... ${currentTemp.toFixed(0)}°C.`);
                detail = ` De ${name} voelt zich niet helemaal comfortabel.`;
                break;

            case currentTemp >= numericTextTemp + 5:
                mood = 'neutraal';
                environment = 'zonnig';
                sentence = addRainSentence(`Het is aangenaam. ${currentTemp.toFixed(0)}°C.`);
                detail = ` De ${name} vindt het wel aangenaam.`;
                break;

            case currentTemp >= numericTextTemp - 5 && currentTemp <= numericTextTemp + 5:
                mood = 'blij';
                environment = 'neutraal';
                sentence = addRainSentence(`Perfect weer! ${currentTemp.toFixed(0)}°C.`);
                detail = ` De ${name} is helemaal tevreden.`;
                break;

            case currentTemp <= numericTextTemp - 15:
                mood = 'boos';
                environment = 'koud';
                sentence = addRainSentence(`Het is veel te koud! ${currentTemp.toFixed(0)}°C.`);
                detail = ` De ${name} heeft het veel te koud.`;
                break;

            case currentTemp <= numericTextTemp - 10:
                mood = 'verdrietig';
                environment = 'koud';
                sentence = addRainSentence(`Het is behoorlijk koud, ${currentTemp.toFixed(0)}°C.`);
                detail = ` De ${name} heeft het liever iets warmer.`;
                break;

            case currentTemp <= numericTextTemp - 5:
                mood = 'neutraal';
                environment = 'koud';
                sentence = addRainSentence(`Het is een beetje fris. ${currentTemp.toFixed(0)}°C.`);
                detail = ` De ${name} vindt het net te frisjes.`;
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
