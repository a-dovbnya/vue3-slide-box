<script setup lang="ts">
import {ref} from 'vue'
import SlideBox from './components/SlideBox.vue'

const open = ref<boolean>(false)
</script>

<template>
    <div class="box">
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Nihil et explicabo voluptatibus dolorum impedit, odio dicta est odit adipisci animi voluptatum dolorem error, tempore sit tenetur voluptates asperiores. Fugit, quod.
        <br />
        <a href="#" @click.prevent="open = !open">Show more</a>
        <SlideBox :open="open">
            Lorem ipsum, dolor sit amet consectetur adipisicing elit. Sapiente praesentium pariatur quos eum quia eligendi
            qui
            doloremque, hic vero quas consequatur blanditiis sed ex reiciendis, veniam, iusto delectus consectetur quasi!
        </SlideBox>
    </div>
</template>

<style scoped>
.box {
    width: 200px;
    border: 1px solid;
    font-size: 14px;
}
</style>