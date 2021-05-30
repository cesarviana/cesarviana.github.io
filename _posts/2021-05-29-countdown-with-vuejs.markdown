---
title: Creating a countdown timer with VueJS
tags: [VueJS, JavaScript, Web Development]
style: border
color: primary
description: Describing the creation of a countdown timer.
---

The idea is to create a banner having a countdown timer, zeroed on [TV Gaspar](https://tvgaspar.com.br) birthday. The approach for this was, first, of course, search for an existent countdown timer. I’ve tried [vuejs-countdown-timer](https://www.npmjs.com/package/vuejs-countdown-timer), that has everything I thought be needed, like props for changing the language. But, for some reason, I could not add it to my environment. The time just don’t appeared.

The second approach was to create a component with just the countdown timer.

```javascript

<template>

    <div class="timer">
        <date-field v-if="months > 0" :value="months" singular="mês" plural="meses"/>
        <date-field :value="days" singular="dia" plural="dias"/>
        <date-field :value="hours" singular="hora" plural="horas"/>
        <date-field :value="minutes" singular="minuto" plural="minutos"/>
        <date-field :value="seconds" singular="segundo" plural="segundos"/>
    </div>

</template>

<script>
export default {
    // ...
    created() {
        this.tickTimer();
        this.interval = setInterval(this.tickTimer, 1000)
    },
    destroyed() {
        clearInterval(this.interval)
    },
    methods: {
        tickTimer() {
            const now = this.$moment();
            const targetDate = this.$moment(this.targetDate);
            if(targetDate.isAfter(now))
            {
                const remainingTime = this.$moment.duration(targetDate.diff(now));
                this.months = remainingTime.months();
                this.days = remainingTime.days();
                this.hours = remainingTime.hours();
                this.minutes = remainingTime.minutes();
                this.seconds = remainingTime.seconds();
            }
        }
    }
    // ...

```
We can se a couple of thinks there. A `date-field` element, an interval handled where the component is created and destroyed, a `tickTimer()` function. We can also see the use of `momentjs`.

The `date-field` component has three props: singular, plural, and value. If the value is 1, the singular word is used, like 1 mês, and 2 meses. 

```html
<template>
    <div class="date-field">
        <span class="number">{{ value }}</span>
        <span class="time-unit">{{ value === 1 ? singular : plural }}</span>
    </div>
</template>
```
The `setInterval` starts when the component is created, and ticks the `tickTimer` every second. This function sets the component data (months, days, hours, minutes and seconds) as the remaining time between now and targetDate.

{% include elements/figure.html image='https://i.ibb.co/XzMh3S1/banner.png' caption='Banner in production' %}

Of course, until here just the countdown was done. The banner also has text wrapping the timer. So, the final component structure was that:

- BirthdayBanner
    - CountdownTimer
        - CountdownDateField

The last thing that is worth to mention is the use of css classes. Without the `scoped` property, the style affects other components. So, the `BirthdayBanner` can styles the `CountdownDateField`.

`BirthdayBanner.vue`
```css
<style lang="scss">
.birthday-banner {
    .timer {
        .date-field {
            background-color: white;
            padding: 2vw 0;
            border-radius: 15%;
            box-shadow: darken($primary-color, 20) 1px 1px 1px;
            min-width: 12vw;
            color: $primary-color;
            text-shadow: #2e3237 1px -1px -1px;
            margin-left: .3vw;
            .number {
                font-size: 2em;
            }
            .time-unit {
                font-size: .8em;
            }
        }
    }
}
```
This is a really simple post, about an ordinary thing. But I hope it could be useful for some other developer who needs something like it.