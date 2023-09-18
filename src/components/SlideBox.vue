<template>
    <transition
        @before-enter="onBeforeEnter"
        @enter="onEnter"
        @after-enter="onAfterEnter"
        @before-leave="onBeforeLeave"
        @leave="onLeave"
        :css="false"
    >
        <div v-show="props.open" class="content">
            <div>
                <slot />
            </div>
        </div>
    </transition>
</template>

<script setup lang="ts">

const props = defineProps({
    open: {
        type: Boolean,
        default: false,
    },
})

const onBeforeEnter = (el: Element): void => {
    el.setAttribute(
        'style',
        'max-height: 0; opacity: 0; overflow: hidden;'
    )
}

const onEnter = (el: Element, done: () => void): void => {
    const duration = 250

    el.setAttribute(
        'style',
        `max-height: ${el.scrollHeight}px; opacity: 1; overflow: hidden;`
    )
    setTimeout(() => {
        el.setAttribute('style', '')
        done() 
    }, duration)
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
    const duration = parseFloat(getComputedStyle(el).transitionDuration) * 1000 || 250

    el.setAttribute(
        'style',
        'max-height: 0; overflow: hidden;'
    )

    setTimeout(() => {
        el.setAttribute('style', '')
        done()
    }, duration)
}
</script>

<style>
.content {
    will-change: contents;
    transition: 0.25s linear;

}
</style>