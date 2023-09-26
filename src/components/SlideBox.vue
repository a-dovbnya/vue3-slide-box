<template>
    <transition
        @before-enter="onBeforeEnter"
        @enter="onEnter"
        @before-leave="onBeforeLeave"
        @leave="onLeave"
        mode="in-out"
        :css="false"
    >
        <div v-if="props.open">
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
    },

    timingFn: {
        type: String,
        default: 'linear'
    },

    opacityAnimate: {
        type: Boolean,
        default: true
    }
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
    el.setAttribute(
        'style',
        getElementStyles(el, true)
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

const getElementStyles = (el: Element, open: Boolean = false) => {
    const maxHeight = open ? `${el.scrollHeight}px` : 0
    const transitionParams = `${props.duration / 1000}s ${props.timingFn}`
    let transitionValue = `max-height ${transitionParams}`
    let opacity = ''

    if (props.opacityAnimate) {
        transitionValue += `, opacity ${transitionParams}`
        opacity = `opacity: ${open ? 1 : 0};`
    }

    return `max-height: ${maxHeight}; overflow: hidden; transition: ${transitionValue}; ${opacity}`
}
</script>
