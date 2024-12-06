<script>
import {Accessibility, Moon, Translate, FontIncrease, Switch} from '$lib/index.js';

let darkModeId = "toggle-darkmode";
let fontSizeId = "toggle-font-size";

let isExpanded = false;

function toggleMenu() {
    isExpanded = !isExpanded;
}

</script>

<form aria-label="Accessibility Options">
    <input type="checkbox" class="toggle-menu"
            id="menu"
            aria-expanded={isExpanded ? "true" : "false"}
            aria-controls="accessibility-options" 
            on:change={toggleMenu}>
    <label title="Accessibility Options" for="menu" class="icon">
         <Accessibility />
    </label>
    <div class="content" id="accessibility-options" aria-hidden={!isExpanded}>
        <ul>
            <li>
                <label for="toggle-darkmode">
                    <span class="label-container">
                        <Moon/>Dark mode
                    </span>
                    <Switch id={darkModeId}/>
                </label>
            </li>
            <li>
                <label for="toggle-font-size">
                    <span class="label-container">
                        <FontIncrease/>Increase Text
                    </span>
                    <Switch id={fontSizeId}/>
                </label>
            </li>
            <li id="google_translate_element">
                <span class="label-container">
                    <Translate/>Translate this website 
                </span>  
            </li>
        </ul>
    </div>
</form>

<style>

form {
    position: fixed;
    bottom: 2rem;
    right: 0;
    z-index: 10000;
    display: flex;
    flex-direction: column-reverse;
    align-items: flex-end;
    transition: right 0.3s;
}

.toggle-menu:checked ~ .content{
	right: 0;
	transition: right 0.5s ease-in-out;
}

.toggle-menu:checked ~ .icon {
	border-radius: 0 0 0 0.6rem;
	transition: border-radius 0.5s ease-in-out
}

.toggle-menu:not(:checked) ~ .content ul {
	display: none;
}

.toggle-menu:not(:checked) ~ .content {
    height: 9rem;
}

input[type="checkbox"][id^="menu"] {
    position: absolute;
    right: -50%;
}

.toggle-menu:focus-visible ~ label {
    outline: 2px solid #005CDC;
    outline-offset: -0.25rem;
}

.icon {
    padding: 0.3rem;
    width: 4rem;
    height: 4rem;
    background-color: #fff;
    border-radius: 0.6rem 0px 0px 0.6rem;
    cursor: pointer;
    transition: border-radius 0.5s ease-in-out;
}

.content {
	position: absolute;
	bottom: 4rem;
	right: -15rem;
	background: #fff;
	height: 9rem;
	width: 15rem;
	border-radius: 0.6rem 0 0 0.6rem;
	transition: right 0.5s ease-in-out;
}

.content ul {
    padding: 0.5rem;
}

.content ul li {
    list-style: none;
    margin: 0.4rem;
}

.content label {
    display: flex;
    justify-content: space-between;
    padding: 2px;
    border-radius: 0.3rem;
}

.content label:hover,
.content label:focus-within {
	background: #d4eaff;
    outline: 2px solid #005CDC;
    cursor: pointer;
}

.label-container {
    display: flex;
    align-items: center;
    gap: 1rem;
}

</style>