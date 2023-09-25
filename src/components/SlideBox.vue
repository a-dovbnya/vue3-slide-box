<template>
    <transition
        @before-enter="onBeforeEnter"
        @enter="onEnter"
        @before-leave="onBeforeLeave"
        @leave="onLeave"
        appear
        mode="in-out"
        :css="false"
    >
        <div v-show="props.open">
            <slot />
        </div>
    </transition>
</template>

<script setup lang="ts">

type ElementStyles = {
    duration: number,
    timingFn: string
}

const props = defineProps({
    open: {
        type: Boolean,
        default: false,
    },

    duration: {
        type: Number,
        default: 250
    },
})

const onBeforeEnter = (el: Element): void => {
    el.setAttribute(
        'style',
        getElementStyles(el)
    )
}

const onEnter = (el: Element, done: () => void): void => {
    el.setAttribute(
        'style',
        getElementStyles(el, true)
    )

    setTimeout(() => {
        el.setAttribute('style', '')
        done() 
    }, props.duration)
}

const onBeforeLeave = (el: Element): void => {
    const styles = getElementStyles(el, true)

    el.setAttribute(
        'style',
        styles
    )
}

const onLeave = (el: Element, done: () => void): void => {
    requestAnimationFrame(() => {
        el.setAttribute(
            'style',
            getElementStyles(el)
        )
    })

    setTimeout(() => {
        el.setAttribute('style', '')
        done()
    }, props.duration)
}

const getTtransitionParams = (el: Element): ElementStyles => {
    const elementStyles = getComputedStyle(el)
    
    return {
        duration: props.duration / 1000,
        timingFn: elementStyles.transitionTimingFunction || 'linear'
    }
}

const getElementStyles = (el: Element, open: Boolean = false) => {
    const { duration, timingFn } = getTtransitionParams(el)
    const maxHeight = open ? `${el.scrollHeight}px` : 0
    const opacity = open ? 1 : 0

    return `
        max-height: ${maxHeight};
        opacity: ${opacity};
        overflow: hidden;
        transition: ${duration}s ${timingFn};
    `
}
</script>
