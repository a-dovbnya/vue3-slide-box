<template>
    <transition
        @before-enter="onBeforeEnter"
        @enter="onEnter"
        @after-enter="onAfterEnter"
        @before-leave="onBeforeLeave"
        @leave="onLeave"
        mode="in-out"
        :css="false"
    >
        <div v-show="props.open">
            <slot />
        </div>
    </transition>
</template>

<script setup lang="ts">

const props = defineProps({
    open: {
        type: Boolean,
        default: false,
    },

    duration: {
        type: Number,
        default: 250
    }
})

const onBeforeEnter = (el: Element): void => {
    el.setAttribute(
        'style',
        'max-height: 0; opacity: 0; overflow: hidden;'
    )
}

const onEnter = (el: Element, done: () => void): void => {
    el.setAttribute(
        'style',
        `max-height: ${el.scrollHeight}px; opacity: 1; overflow: hidden; transition: ${props.duration/1000}s linear;`
    )

    setTimeout(() => {
        el.setAttribute('style', '')
        done() 
    }, props.duration)
}

const onAfterEnter = (el: Element): void => {
    el.setAttribute(
        'style',
        ''
    )
}

const onBeforeLeave = (el: Element): void => {
    el.setAttribute(
        'style',
        `max-height: ${el.clientHeight}px; opacity: 1; overflow: hidden;`
    )
}

const onLeave = (el: Element, done: () => void): void => {
    const test = getComputedStyle(el).willChange
    console.log('duration 1 ', test)

    el.setAttribute(
        'style',
        `max-height: 0; opacity: 0; overflow: hidden; transition: ${props.duration/1000}s linear;`
    )

    setTimeout(() => {
        el.setAttribute('style', '')
        done()
    }, props.duration)
}
</script>
